public ArrayList getQueryResult(String filterValues, String queryString, String sourceMode, String searchFilterValues, String listAll, String clientId) throws Exception {
    String[] filterValuesArray = null;
    String[] filterDataTypeArray = null;
    String[] searchFilterValuesArray = null;
    int filterCriteriaCount = 0;
    ArrayList filterCriteria = new ArrayList();
    ArrayList resultArray = null;
    String refreshOnLoad = Constant.YES;
    StringBuffer popupQuery = new StringBuffer();

    // Parse search filter values
    if (searchFilterValues != null && !searchFilterValues.trim().equals("")) {
        searchFilterValuesArray = STKGeneral.split(searchFilterValues, Constant.DELIMITER_CARRET);
    }

    // Determine if source mode is required
    if (searchFilterValuesArray != null && searchFilterValuesArray.length > 0) {
        for (String searchFilterValue : searchFilterValuesArray) {
            String[] tempArray = STKGeneral.split(STKGeneral.nullCheck(searchFilterValue).trim(), Constant.DELIMITER_PIPE);
            if (tempArray != null && tempArray.length > 0 && (!STKGeneral.nullCheck(tempArray[0]).trim().equals(""))) {
                sourceMode = Constant.SEARCH_MODE;
                break;
            }
        }
    }

    // Build query and filter criteria
    if (refreshOnLoad.equalsIgnoreCase(Constant.CHAR_YES) || sourceMode.equalsIgnoreCase(Constant.SEARCH_MODE)) {
        filterValuesArray = STKGeneral.split(filterValues, Constant.DELIMITER_CARRET);
        popupQuery.append(STKGeneral.nullCheck(queryString).trim())
                  .append(" INNER JOIN client c ON d.client_id = ? ") // Add the clientId join condition
                  .append("WHERE TEMPLATE_ID NOT IN ('PWI_AGREEMENT_DOC') ")
                  .append("AND TEMPLATE_ID LIKE ? ")
                  .append("AND DOC_DESC LIKE ? ")
                  .append("ORDER BY SORT_SEQ_NO");

        // Add criteria from filterValues
        if (!filterValues.equals("") && filterValuesArray != null) {
            for (String filterValue : filterValuesArray) {
                filterDataTypeArray = STKGeneral.split(STKGeneral.nullCheck(filterValue).trim(), Constant.DELIMITER_PIPE);

                if (filterDataTypeArray != null && filterDataTypeArray.length == 2) {
                    filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE +
                        STKGeneral.nullCheck(filterDataTypeArray[0]).trim() + Constant.DELIMITER_PIPE +
                        STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
                }
            }
        }

        // Add criteria from search filter values
        if (searchFilterValuesArray != null && !searchFilterValues.equals("")) {
            for (String searchFilterValue : searchFilterValuesArray) {
                filterDataTypeArray = STKGeneral.split(STKGeneral.nullCheck(searchFilterValue).trim(), Constant.DELIMITER_PIPE);

                if (filterDataTypeArray != null && filterDataTypeArray.length == 2) {
                    if (STKGeneral.nullCheck(listAll).trim().equalsIgnoreCase(Constant.CHAR_YES)) {
                        filterCriteria.add(STKGeneral.split(++filterCriteriaCount + "|%|" +
                            STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
                    } else {
                        filterCriteria.add(STKGeneral.split(++filterCriteriaCount + Constant.DELIMITER_PIPE +
                            STKGeneral.nullCheck(filterDataTypeArray[0]).trim() + "%|" +
                            STKGeneral.nullCheck(filterDataTypeArray[1]).trim(), Constant.DELIMITER_PIPE));
                    }
                }
            }
        }

        // Add clientId as the first parameter
        filterCriteria.add(0, new String[]{String.valueOf(++filterCriteriaCount), "STRING", clientId});

        // Execute query using getResultset
        resultArray = getResultset(filterCriteria, popupQuery.toString());
    }

    return resultArray;
}
