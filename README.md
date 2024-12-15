  public ArrayList getQueryResult(String filterValues, String queryString, String sourceMode, String searchFilterValues, String listAll,String clientId) throws Exception
    {
    	    String[] filterValuesArray = null;
    	    String[] filterDataTypeArray = null;
    	    String[] searchFilterValuesArray = null;
    	    int filterCriteriaCount = 0;
    	    ArrayList filterCriteria = new ArrayList();
    	    ArrayList resultArray = null;
    	    String refreshOnLoad = Constant.YES;
    	    StringBuffer popupQuery = new StringBuffer();

    	    // Split searchFilterValues into an array
    	    if (searchFilterValues != null && !searchFilterValues.trim().equals("")) {
    	        searchFilterValuesArray = STKGeneral.split(searchFilterValues, Constant.DELIMITER_CARRET);
    	    }

    	    // Check for source mode
    	    if (searchFilterValuesArray != null && searchFilterValuesArray.length > 0) {
    	        String[] tempArray = null;
    	        for (int i = 0; i < searchFilterValuesArray.length; i++) {
    	            tempArray = STKGeneral.split(STKGeneral.nullCheck(searchFilterValuesArray[i]).trim(), Constant.DELIMITER_PIPE);
    	            if (tempArray != null && tempArray.length > 0 && (!STKGeneral.nullCheck(tempArray[0]).trim().equals(""))) {
    	                sourceMode = Constant.SEARCH_MODE;
    	                break;
    	            }
    	        }
    	    }

    	    // Build the SQL query if refresh is needed or in search mode
    	    if (refreshOnLoad.equalsIgnoreCase(Constant.CHAR_YES) || sourceMode.equalsIgnoreCase(Constant.SEARCH_MODE)) {
    	        filterValuesArray = STKGeneral.split(filterValues, Constant.DELIMITER_CARRET);
    	        popupQuery.append(STKGeneral.nullCheck(queryString).trim());

    	        // Add filter criteria from filterValues
    	        if (!filterValues.equals("") && filterValuesArray != null) {
    	            for (int i = 0; i < filterValuesArray.length; i++) {
    	                filterDataTypeArray = STKGeneral.split(STKGeneral.nullCheck(filterValuesArray[i]).trim(), Constant.DELIMITER_PIPE);

    	                if (filterDataTypeArray != null && filterDataTypeArray.length == 2) {
    	                    filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE +
    	                        STKGeneral.nullCheck(filterDataTypeArray[0]).trim() + Constant.DELIMITER_PIPE +
    	                        STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
    	                }
    	            }
    	        }

    	        // Add search filter criteria
    	        if (searchFilterValuesArray != null && !searchFilterValues.equals("")) {
    	            for (int i = 0; i < searchFilterValuesArray.length; i++) {
    	                filterDataTypeArray = STKGeneral.split(STKGeneral.nullCheck(searchFilterValuesArray[i]).trim(), Constant.DELIMITER_PIPE);

    	                if (filterDataTypeArray != null && filterDataTypeArray.length == 2) {
    	                    if (STKGeneral.nullCheck(listAll).trim().equalsIgnoreCase(Constant.CHAR_YES)) {
    	                        filterCriteria.add(STKGeneral.split(++filterCriteriaCount + "|%|" + STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
    	                    } else {
    	                        filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE +
    	                            STKGeneral.nullCheck(filterDataTypeArray[0]).trim() + "%|" +
    	                            STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
    	                    }
    	                }
    	            }
    	        }

    	        // Add clientId as a filter
    	        if (clientId != null && !clientId.trim().isEmpty()) {
    	            filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE + clientId.trim() + Constant.DELIMITER_PIPE + "S", Constant.DELIMITER_PIPE));
    	        }

    	        resultArray = getResultset(filterCriteria, popupQuery.toString());
    	    }

    	    return resultArray;
    	}
