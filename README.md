public ArrayList getQueryResult(String filterValues, String queryString, String sourceMode, String searchFilterValues, String listAll, String clientId) throws Exception {
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

    // Build the query and criteria
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

        // Use connection to prepare and execute query
        try (Connection conn = getConnection(); PreparedStatement stmt = conn.prepareStatement(popupQuery.toString())) {
            int paramIndex = 1;

            // Bind clientId as the first parameter
            if (clientId != null && !clientId.trim().isEmpty()) {
                stmt.setString(paramIndex++, clientId);
            }

            // Bind other parameters from filterCriteria
            for (int i = 0; i < filterCriteria.size(); i++) {
                String[] criteria = (String[]) filterCriteria.get(i);
                if (criteria[1].equalsIgnoreCase("STRING")) {
                    stmt.setString(paramIndex++, criteria[2]);
                } else if (criteria[1].equalsIgnoreCase("DATE")) {
                    stmt.setTimestamp(paramIndex++, java.sql.Timestamp.valueOf(criteria[2] + " 00:00:00"));
                } else if (criteria[1].equalsIgnoreCase("NUMERIC")) {
                    stmt.setBigDecimal(paramIndex++, new BigDecimal(criteria[2]));
                }
            }

            // Execute query and process result set
            try (ResultSet rs = stmt.executeQuery()) {
                resultArray = new ArrayList();
                while (rs.next()) {
                    ArrayList row = new ArrayList();
                    row.add(rs.getString("TEMPLATE_ID"));
                    row.add(rs.getString("DOC_DESC"));
                    row.add(rs.getInt("SORT_SEQ_NO"));
                    resultArray.add(row);
                }
            }
        } catch (Exception e) {
            throw new RuntimeException("Error executing the query", e);
        }
    }

    return resultArray;
}
