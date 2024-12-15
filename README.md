
    try {
        // Establish connection
        connObj = getConnection();
        stmtObj = connObj.prepareStatement(sqlStr);

        // Bind parameters from criteria
        for (int iterator = 0; iterator < criteria.size(); iterator++) {
            String[] strArray = (String[]) criteria.get(iterator);

            logger.debug("Binding parameter at index: " + strArray[FIELD] + ", VALUE: " + strArray[VALUE] + ", DATATYPE: " + strArray[DATATYPE]);

            switch (strArray[DATATYPE]) {
                case STRING:
                    stmtObj.setString(Integer.parseInt(strArray[FIELD]), strArray[VALUE]);
                    break;

                case DATE:
                    String dateValue = strArray[VALUE];
                    if (dateValue.length() == 10) {
                        dateValue += " 00:00:00";
                    }
                    stmtObj.setTimestamp(Integer.parseInt(strArray[FIELD]), java.sql.Timestamp.valueOf(dateValue));
                    break;

                case NUMERIC:
                    stmtObj.setBigDecimal(Integer.parseInt(strArray[FIELD]), new BigDecimal(strArray[VALUE]));
                    break;

                default:
                    throw new IllegalArgumentException("Unsupported data type: " + strArray[DATATYPE]);
            }
        }

        // Execute query and process results
        logger.debug("Executing SQL: " + sqlStr);
        resultSet = stmtObj.executeQuery();

        if (resultSet != null) {
            resultArray = mapResultsList(resultSet);
        }

    } catch (Exception e) {
        logger.error("Exception in getResultset() - ", e);
    } finally {
        // Close resources
        try {
            if (resultSet != null) resultSet.close();
            if (stmtObj != null) stmtObj.close();
            if (connObj != null && !connObj.isClosed()) connObj.close();
        } catch (Exception e) {
            logger.error("Exception in getResultset() - finally block - ", e);
        }
    }

    return resultArray;
}
