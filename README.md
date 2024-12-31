private List<CBRS> loadTaxLots(ResultSet rs, CBRS asset) throws Exception {
		ArrayList<CBRS> cbrsl = new ArrayList<CBRS>();
		CBRS cbrs = null;
		BigDecimal remainder = new BigDecimal(0);
		remainder.setScale(8);
		int x = 0;
		while (rs.next()) {
			x++;
			BigDecimal qty = rs.getBigDecimal("Share_Quantity");
			LOG.debug("tax Lot qty = "+qty.toPlainString());
			// skip tax lots of zero quantity as per Anthony
			if (qty.compareTo(BigDecimal.ZERO) > 0) {
				cbrs = new CBRS();
				cbrsl.add(cbrs);
				cbrs.setREC_TYPE("T");
				cbrs.setRECORD_LENGTH(1000);
				
				cbrs.setTRANSACTION_TYPE("02");
				cbrs.setTRANSFER_CONTROL_NUMBER(asset.getTRANSFER_CONTROL_NUMBER());
				cbrs.setCONTRA_FIRM_NUMBER(asset.getCONTRA_FIRM_NUMBER()); // ?
				cbrs.setCONTRA_FIRM_TYPE("DTCPRT");
				cbrs.setSUBMITTING_FIRM_NUMBER("00000158");
				cbrs.setSUBMITTING_FIRM_TYPE("DTCPRT");
				cbrs.setRECEIVER_CUSTOMER_ACCOUNT_NUMBER(asset.getRECEIVER_CUSTOMER_ACCOUNT_NUMBER());
				cbrs.setDELIVERER_CUSTOMER_ACCOUNT_NUMBER(asset.getDELIVERER_CUSTOMER_ACCOUNT_NUMBER());
				cbrs.setISIN_COUNTRY_CODE("US");
				cbrs.setISIN_SECURITY_ISSUE_ID(asset.getISIN_SECURITY_ISSUE_ID());
				cbrs.setASSET_DESCRIPTION(asset.getASSET_DESCRIPTION());
				cbrs.setACTION(asset.getACTION());

				// tax lot specific values
				// leave blank as per TOAI 3/28/14
				//cbrs.setTAXES_REPORTED_BY_ISSUER_TRANSFER_AGENT("N");
				BigDecimal share_qty = null;
				LOG.debug("action "+cbrs.getACTION());
				// adjustments to tax lots after freeD has been sent
				// the additional values found in BDTaxLotQty are just a part of the total from share quantity
				// their should be only ONE adjusted tax lot, if more then one this will fail to process properly
				// assigning the asset value to each lot (bad thing)
				if(cbrs.getACTION() != null && cbrs.getACTION().equals("ADJ")){
					if(x > 1){
						String msg = "Not sending, ADJ option can only be applied to an asset with a single tax lot";
						LOG.debug(msg);
						throw new Exception(msg);
					}
					share_qty = asset.getBDTAX_LOT_QUANITY();					
				}else{
					share_qty = rs.getBigDecimal("Share_Quantity");
				}
					
				if(cbrs.getACTION() != null && cbrs.getACTION().equals("RESND")){
					cbrs.setRECORD_CONTENT_INDICATOR("02");
				}else{
					cbrs.setRECORD_CONTENT_INDICATOR("01");
				}
				
				//LOG.debug("found tax lot for with qty = "+share_qty);
				BigDecimal rounded;
				BigDecimal lotremainder = null;
				//if (!isWholeNumber(share_qty)){
				if(share_qty.compareTo(BigDecimal.ZERO) > 0) {
					lotremainder = getTruncatedTaxLotRemainder(share_qty);
					remainder = remainder.add(lotremainder);
					rounded = getTruncatedTaxLot(share_qty,4);
				} else {
					rounded = share_qty;
					LOG.debug("share quanity is 0");
				}
				cbrs.setBDTAX_LOT_QUANITY(rounded);
				cbrs.setTAX_LOT_QUANITY(getCommaDecimalString(rounded, 4));
				cbrs.setPOSITION_CODE("L");// always long as per Anthony
				// set to empty string because optional
				cbrs.setORIGINAL_ACQUISITION_DATE_FOR_WASH_SALE_ADJUSTMENT(rs.getString("Acquisition_Date_Wash_Sale"));
				String covered = rs.getString("Covered_Or_Uncovered");
				BigDecimal acb = BigDecimal.ZERO;
				// as per Joan do not give any cost basis if uncovered ever!
				// last moment changes, if uncovered mark noncovered pending indicator if covered populate acq date
				if (covered.equals("Covered")){
					acb = rs.getBigDecimal("Adj_Cost_Basis");
					if(acb == null){
						String msg = "Not sending, Adjusted Cost Basis is not available for covered tax lot ";
						LOG.error(msg);
						throw new Exception(msg);
					}
					int dec_places = 2;
					BigDecimal trunked = this.getTruncatedTaxLot(acb,dec_places);
					if(trunked.compareTo(BigDecimal.ZERO) == 0){
						cbrs.setZERO_BASIS_INDICATOR("01");
					}else{
						// leave blank						
					}
					cbrs.setTAX_LOT_CURRENT_COST(getCommaDecimalString(acb,2));
					
					String aqd = rs.getString("Acquisition_Date");
					if(aqd == null){
						String msg = "Not sending, Acquisition_Date is not available for a covered tax lot ";
						LOG.error(msg);
						throw new Exception(msg);
					}
					cbrs.setACQUISITION_DATE_OF_TAX_LOT(aqd);
					
				}else{
					cbrs.setNONCOVERED_PENDING_INDICATOR("01");
					cbrs.setZERO_BASIS_INDICATOR("02");
					
				}
				if(cbrs.getACTION() != null && cbrs.getACTION().equals("ADJ")){
					LOG.debug("is an adjustment skipping all other tax lots");
					break;
				}
				++count;
				LOG.debug("Tax Lots processed "+count);
				LOG.debug("Adj Cost Basis "+acb.toPlainString());
				LOG.debug("Tax Lot Quanity "+cbrs.getBDTAX_LOT_QUANITY().toPlainString());
				TaxLotTotal = cbrs.getBDTAX_LOT_QUANITY().add(TaxLotTotal);				
				LOG.debug("Total Tax Lot Quanitity "+TaxLotTotal);
				LOG.debug("Tax Lot Remainder "+lotremainder.toPlainString());
				LOG.debug("Running Remainder "+remainder.toPlainString());
			}else{
				LOG.debug("skipping tax Lot qty = "+qty.toPlainString());
				
			}
			
		}
		
		LOG.debug("total lots found "+x);
		// we have a max decimal in the record but the amounts must still match
		// the total
		// so we take the remainder that would have been lost and add it back to
		// the last tax lot before rounding
		// this way the asset DispisitionHdr rounded value will equal the total
		// of the tax lots DispisitionDtl rounded value
		if (cbrs != null) {
			BigDecimal qty = cbrs.getBDTAX_LOT_QUANITY();
			if (remainder.compareTo(BigDecimal.ZERO) > 0) {
				BigDecimal t;
				LOG.debug("Total Remainder to add "+remainder);
				qty = qty.add(remainder);
				if (qty.compareTo(BigDecimal.ZERO) > 0) {
					t = getTruncatedTaxLot(qty,4);
					cbrs.setTAX_LOT_QUANITY(getCommaDecimalString(t, 4));
					cbrs.setBDTAX_LOT_QUANITY(t);
				}
			}
		}
		return cbrsl;
	}
