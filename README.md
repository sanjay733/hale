  public ArrayList getQueryResult(String filterValues, String queryString, String sourceMode, String searchFilterValues, String listAll,String keyField,String sqlQuery,
    		String aStartSeqNo, String aEndSeqNo, int aRowCount, String aNavigationValue, String pageNo,String clientId) throws Exception
    {
        String[] filterValuesArray = null;
        String[] filterDataTypeArray = null;
        String[] searchFilterValuesArray = null;
        int filterCriteriaCount = 0;
        ArrayList filterCriteria = new ArrayList();
        ArrayList resultArray = null;
        String refreshOnLoad = Constant.YES;
        StringBuffer popupQuery = new StringBuffer();
        StringBuffer tempPopupQuery = new StringBuffer();
        
        Connection connObj = null;
        PreparedStatement stmtObj = null;
        ResultSet resultSet = null;
        String sqlStr1 = null;
        
        int dbType = STKGeneral.getInteger(DBProperties.getInstance().getDBType());
        String findSql = null;
        String countSql = null;
        
        try
		{
        	
			if (searchFilterValues != null && !searchFilterValues.trim().equals(""))
	            searchFilterValuesArray = STKGeneral.split(searchFilterValues, Constant.DELIMITER_CARRET);
	
	        if (searchFilterValuesArray != null && searchFilterValuesArray.length > 0)
	        {
	            String[] tempArray = null;
	            for (int i = 0; i < searchFilterValuesArray.length; i++)
	            {
	                tempArray = STKGeneral.split(STKGeneral.nullCheck(searchFilterValuesArray[i]).trim(), Constant.DELIMITER_PIPE);
	                if (tempArray != null && tempArray.length > 0 && (!STKGeneral.nullCheck(tempArray[0]).trim().equals("")))
	                {
	                    sourceMode = Constant.SEARCH_MODE;
	                    break;
	                }
	            }
	        }
	
	        if (refreshOnLoad.equalsIgnoreCase(Constant.CHAR_YES) || sourceMode.equalsIgnoreCase(Constant.SEARCH_MODE))
	        {
	            filterValuesArray = STKGeneral.split(filterValues, Constant.DELIMITER_CARRET);
	            popupQuery.append(STKGeneral.nullCheck(queryString).trim());
	
	            if (!filterValues.equals("") && filterValuesArray != null)
	            {
	                for (int i = 0; i < filterValuesArray.length; i++)
	                {
	                    filterDataTypeArray = STKGeneral.split(STKGeneral.nullCheck(filterValuesArray[i]).trim(), Constant.DELIMITER_PIPE);
	
	                    if (filterDataTypeArray != null && filterDataTypeArray.length == 2)
	                    {
	                        filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE + STKGeneral.nullCheck(filterDataTypeArray[0]).trim() + Constant.DELIMITER_PIPE
	                                + STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
	                    }
	                }
	            }
	
	            if (searchFilterValuesArray != null && !searchFilterValues.equals(""))
	            {
	                for (int i = 0; i < searchFilterValuesArray.length; i++)
	                {
	                    filterDataTypeArray = STKGeneral.split(STKGeneral.nullCheck(searchFilterValuesArray[i]).trim(), Constant.DELIMITER_PIPE);
	
	                    if (filterDataTypeArray != null && filterDataTypeArray.length == 2)
	                    {
	                        if (STKGeneral.nullCheck(listAll).trim().equalsIgnoreCase(Constant.CHAR_YES))
	                        {
	                            filterCriteria.add(STKGeneral.split(++filterCriteriaCount + "|%|" + STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
	                        }
	                        else
	                        {
	                            filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE + STKGeneral.nullCheck(filterDataTypeArray[0]).trim() + "%|"
	                                    + STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
	                        }
	                    }
	                }
	            }
	
	            //resultArray = getResultset(filterCriteria, popupQuery.toString());
	            
	            connObj = getConnection();
	            
	            
	            if(dbType == Constant.DB_TYPE_SQLSERVER)
	            {
	            	findSql 	= getFindQuerySQLServer(aStartSeqNo,aEndSeqNo,aRowCount,aNavigationValue,popupQuery.toString(),keyField);
	            	countSql 	= getCountQuerySQLServer(keyField,sqlQuery);
	            }
	            else
	            {
	            	findSql 	= getFindQueryOracle(aStartSeqNo,aEndSeqNo,aRowCount,aNavigationValue,popupQuery.toString(),keyField);
	            	countSql 	= getCountQueryOracle(keyField,sqlQuery);
	            }
	            
	            
	            stmtObj = connObj.prepareStatement(findSql);
	            ArrayList tempFilterCriteria = filterCriteria; 
	            
	            int index =1;
	            if(clientId!=null && !clientId.trim().isEmpty()) {
	            	stmtObj.setString(index++, clientId);
	            }
	            for (int iterator = 0; iterator < filterCriteria.size(); iterator++)
	            {
	                String[] strArray = (String[]) filterCriteria.get(iterator);
	
	                if (strArray[DATATYPE].equals(STRING))
	                {
	                    stmtObj.setString(Integer.parseInt(strArray[FIELD]), strArray[VALUE]);
	                }
	                else if (strArray[DATATYPE].equals(DATE))
	                {
	                    if (strArray[VALUE].length() == 10)
	                        strArray[VALUE] = strArray[VALUE] + " 00:00:00";
	
	                    stmtObj.setTimestamp(Integer.parseInt(strArray[FIELD]), java.sql.Timestamp.valueOf(strArray[VALUE]));
	                }
	                else if (strArray[DATATYPE].equals(NUMERIC))
	                {
	                    stmtObj.setBigDecimal(Integer.parseInt(strArray[FIELD]), new BigDecimal(strArray[VALUE]));
	                }
	            }
	            resultSet =RSWrapper.getProxyResultSet( stmtObj.executeQuery());
	            String startSeqNo 	= "0";
	            String endSeqNo 	= "0";
	            int seqCount 		= 0;
	            if (resultSet != null)
	            {
	                resultArray = new ArrayList();
	            	ResultSetMetaData rsMetaData 	= resultSet.getMetaData();
	                int count = rsMetaData.getColumnCount();

	                while (resultSet.next())
	                {
	                    ArrayList columnArray = new ArrayList();
	                    
	                    if (seqCount == 0)
	                    {
	                    	if(dbType == Constant.DB_TYPE_SQLSERVER)
	                    		startSeqNo =  resultSet.getString(keyField.trim());
	                        else
	                        	startSeqNo =  resultSet.getString("NAV_SEQ_NO");
	                    	
	                    	logger.debug(" The startSeqNo  : " + startSeqNo);
	                        seqCount++;
	                    }
	                    else
	                    {
	                    	if(dbType == Constant.DB_TYPE_SQLSERVER)
	                    		endSeqNo =  resultSet.getString(keyField.trim());
	                        else
	                        	endSeqNo =  resultSet.getString("NAV_SEQ_NO");
	                    }
	                   
	                    
	                    for (int iterator = 1; iterator <= count; iterator++)
	                    {
	                        switch (rsMetaData.getColumnType(iterator))
	                        {

	                            case Types.BIGINT:
	                            case Types.DECIMAL:
	                            case Types.DOUBLE:
	                            case Types.FLOAT:
	                            case Types.INTEGER:
	                            case Types.NUMERIC:
	                            case Types.REAL:
	                            case Types.SMALLINT:
	                            case Types.TINYINT:
	                                columnArray.add(resultSet.getString(iterator));
	                                break;

	                            case Types.VARCHAR:
	                            case Types.CHAR:
	                                columnArray.add(resultSet.getString(iterator));
	                                break;

	                            case Types.DATE:
	                            {
	                                columnArray.add(resultSet.getTimestamp(iterator));
	                                break;
	                            }
	                            case Types.TIMESTAMP:
	                            {
	                                columnArray.add(resultSet.getTimestamp(iterator));
	                                break;
	                            }
	                        }
	                    }
	                    resultArray.add(columnArray);
	                }
	                logger.debug(" The endSeqNo  : " + endSeqNo);
	                closeResources(stmtObj, resultSet);
	                
	                if (endSeqNo!= null && endSeqNo.equals("0"))
	                    endSeqNo = startSeqNo;

	                stmtObj = connObj.prepareStatement(countSql);
	                
	                for (int iterator = 0; iterator < tempFilterCriteria.size(); iterator++)
		            {
		                String[] strArray = (String[]) tempFilterCriteria.get(iterator);
		
		                if (strArray[DATATYPE].equals(STRING))
		                {
		                    stmtObj.setString(Integer.parseInt(strArray[FIELD]), strArray[VALUE]);
		                }
		                else if (strArray[DATATYPE].equals(DATE))
		                {
		                    if (strArray[VALUE].length() == 10)
		                        strArray[VALUE] = strArray[VALUE] + " 00:00:00";
		
		                    stmtObj.setTimestamp(Integer.parseInt(strArray[FIELD]), java.sql.Timestamp.valueOf(strArray[VALUE]));
		                }
		                else if (strArray[DATATYPE].equals(NUMERIC))
		                {
		                    stmtObj.setBigDecimal(Integer.parseInt(strArray[FIELD]), new BigDecimal(strArray[VALUE]));
		                }
		            }
	                resultSet =RSWrapper.getProxyResultSet( stmtObj.executeQuery());
	                String rowCount = "";
	                String maxSeqNo = "";
	                String minSeqNo = "";
	                int distinctIndex = -1;
	                while (resultSet.next())
	                {
	                	rowCount = resultSet.getString("ROW_COUNT");
	                    minSeqNo = resultSet.getString("MIN_SEQ_NO");
	                	if(dbType == Constant.DB_TYPE_SQLSERVER)
	                	{
	                		maxSeqNo = resultSet.getString("MAX_SEQ_NO");
	                	}
	                	else
	                	{
	                		distinctIndex = checkDistinct(sqlQuery);
	                		if(distinctIndex > 0)
		                    {
		                    	maxSeqNo = resultSet.getString("ROW_COUNT");
		                    }
		                    else
		                    {
			                	maxSeqNo = resultSet.getString("MAX_SEQ_NO");
		                    }
	                	}
	                }
	                closeResources(stmtObj, resultSet);
	                int totalPages = Integer.parseInt(rowCount) / Integer.parseInt(Constant.GRID_ROWS_POPUP);

	                if ((Integer.parseInt(rowCount) % Integer.parseInt(Constant.GRID_ROWS_POPUP)) > 0)
	                    totalPages = totalPages + 1;
	                if (totalPages == 0)
	                    totalPages = 1;
	                String pages = null;
	                if (aNavigationValue != null && aNavigationValue.trim().length()>0)
	                {
	                    if (pageNo != null && (!pageNo.trim().equals("")))
	                    {
	                        if (aNavigationValue.equals("N"))
	                        {
	                            pageNo = String.valueOf(Integer.parseInt(pageNo) + 1);
	                            if (Integer.parseInt(pageNo) >= totalPages)
	                                pages = "Page " + totalPages + " of " + totalPages;
	                            else
	                                pages = "Page " + pageNo + " of " + totalPages;
	                        }
	                        else if (aNavigationValue.equals("P"))
	                        {
	                            pageNo = String.valueOf(Integer.parseInt(pageNo) - 1);
	                            pages = "Page " + pageNo + " of " + totalPages;
	                        }
	                        else if (aNavigationValue.equals("F"))
	                        {
	                            int currentPage = 1;
	                            pages = "Page " + currentPage + " of " + totalPages;
	                            pageNo = String.valueOf(currentPage);
	                        }
	                        else
	                        {
	                            pages = "Page " + totalPages + " of " + totalPages;
	                            pageNo = String.valueOf(totalPages);
	                        }
	                    }
	                }
	                else
	                {
	                    if (Integer.parseInt(rowCount) >= 1)
	                    {
	                        pageNo = "1";
	                        pages = "Page " + pageNo + " of " + totalPages;
	                    }
	                }
	                ArrayList resultArrayList = new ArrayList();
	                
	                resultArrayList.add(0,resultArray);
	                resultArrayList.add(1, startSeqNo + "" );
	                resultArrayList.add(2, endSeqNo + "");
	                resultArrayList.add(3, rowCount);
	                resultArrayList.add(4, minSeqNo);
	                resultArrayList.add(5, maxSeqNo);
	                resultArrayList.add(6, pages);
	                resultArrayList.add(7, String.valueOf(pageNo));
	                
	                return (ArrayList) resultArrayList.clone();

	            }
	        }
		}
        catch(SQLException sqlE)
		{
        	throw sqlE;
		}
        catch(Exception e)
		{
        	throw e;
		}
        finally
		{
        	closeResources(connObj,stmtObj,resultSet);
		}
        return resultArray;
    }
