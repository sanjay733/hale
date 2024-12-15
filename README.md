for (int iterator = 0; iterator < criteria.size(); iterator++) {
            String[] strArray = (String[]) criteria.get(iterator);
            logger.debug("Criteria " + iterator + ": FIELD=" + strArray[FIELD] + ", VALUE=" + strArray[VALUE] + ", DATATYPE=" + strArray[DATATYPE]);

            if (strArray[DATATYPE].equals(STRING)) {
                stmtObj.setString(Integer.parseInt(strArray[FIELD]), strArray[VALUE]);
                logger.debug("Bound STRING at index " + strArray[FIELD] + ": " + strArray[VALUE]);
            } else if (strArray[DATATYPE].equals(DATE)) {
                if (strArray[VALUE].length() == 10) {
                    strArray[VALUE] = strArray[VALUE] + " 00:00:00";
                }
                stmtObj.setTimestamp(Integer.parseInt(strArray[FIELD]), java.sql.Timestamp.valueOf(strArray[VALUE]));
                logger.debug("Bound DATE at index " + strArray[FIELD] + ": " + strArray[VALUE]);
            } else if (strArray[DATATYPE].equals(NUMERIC)) {
                stmtObj.setBigDecimal(Integer.parseInt(strArray[FIELD]), new BigDecimal(strArray[VALUE]));
                logger.debug("Bound NUMERIC at index " + strArray[FIELD] + ": " + strArray[VALUE]);
            }
        }

        logger.debug("Executing SQL: " + sqlStr);

        // Execute query and map results
        resultSet = RSWrapper.getProxyResultSet(stmtObj.executeQuery());
        if (resultSet != null) {
            resultArray = mapResultsList(resultSet);
        }
    } catch (Exception e) {
        logger.error("Exception in getResultset() - ", e);
        throw e; // Re-throw for visibility
    } finally {
        try {
            if (resultSet != null) resultSet.close();
            if (stmtObj != null) stmtObj.close();
            if (connObj != null && !connObj.isClosed()) connObj.close();
        } catch (Exception e) {
            logger.error("Exception in getResultset() - finally block  - ", e);
        }
    }
    return resultArray;
}
