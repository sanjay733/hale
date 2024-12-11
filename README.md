 public ArrayList<ArrayList<Comparable>> getResultset(ArrayList criteria, String sqlStr)
    {
        Connection connObj = null;
        PreparedStatement stmtObj = null;
        ResultSet resultSet = null;
        String sqlStr1 = null;
        ArrayList<ArrayList<Comparable>> resultArray = null;
        try
        {
            connObj = getConnection();
            stmtObj = connObj.prepareStatement(sqlStr);
            for (int iterator = 0; iterator < criteria.size(); iterator++)
            {
                String[] strArray = (String[]) criteria.get(iterator);

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
            if (resultSet != null)
            {
                resultArray = mapResultsList(resultSet);
            }
        }
        catch (Exception e)
        {
            logger.error("Exception in getResultset() - ", e);
        }
        finally
        {
            try
            {
                if (resultSet != null)
                    resultSet.close();
                if (stmtObj != null)
                    stmtObj.close();
                if (connObj != null && !connObj.isClosed())
                    connObj.close();
            }
            catch (Exception e)
            {
                logger.error("Exception in getResultset() - finally block  - ", e);
            }
        }
        return resultArray;
    }

    public static ArrayList<ArrayList<Comparable>> mapResultsList(ResultSet rslt) throws Exception
    {
        ArrayList<ArrayList<Comparable>> resultArray = new ArrayList<ArrayList<Comparable>>();
        ResultSetMetaData rsMetaData = rslt.getMetaData();
        int count = rsMetaData.getColumnCount();

        while (rslt.next())
        {
            ArrayList<Comparable> columnArray = new ArrayList<Comparable>();

            for (int iterator = 1; iterator <= count; iterator++)
            {
                switch (rsMetaData.getColumnType(iterator))
                {

                    case Types.BIGINT:
                    case Types.INTEGER:                   
                    case Types.REAL:
                    case Types.SMALLINT:
                    case Types.TINYINT:
                        columnArray.add(rslt.getString(iterator));
                        break;
                      
                    case Types.NUMERIC:
                    case Types.DECIMAL:
                    case Types.DOUBLE:
                    case Types.FLOAT:
                    	columnArray.add(new BigDecimal(rslt.getString(iterator),MathContext.DECIMAL128).toPlainString());
                        break;
                        
                    case Types.VARCHAR:
                    case Types.NVARCHAR:
                    case Types.CHAR:
                        columnArray.add(rslt.getString(iterator));
                        break;

                    case Types.DATE:
                    {
                        columnArray.add(rslt.getTimestamp(iterator));
                        break;
                    }
                    case Types.TIMESTAMP:
                    {
                        columnArray.add(rslt.getTimestamp(iterator));
                        break;
                    }
                }
            }
            resultArray.add(columnArray);
        }
        return (ArrayList<ArrayList<Comparable>>) resultArray.clone();
    }
    
