sqlQueryList.set(LETTER_TEMPLATE_LIST, 
    "SELECT DISTINCT TEMPLATE_ID, (DOC_DESC + ' ' + d.CLIENT_ID) DOC_DESC, SORT_SEQ_NO " + 
    "FROM " + TableConstants.DOC_TEMPLATE + " d " + 
    "INNER JOIN client c ON d.client_id = ? " +  // Join condition with clientId
    "WHERE TEMPLATE_ID NOT IN ('PWI_AGREEMENT_DOC') " + 
    "AND TEMPLATE_ID LIKE ? " +  // Filter by TEMPLATE_ID using LIKE
    "AND DOC_DESC LIKE ? " +     // Filter by DOC_DESC using LIKE
    "ORDER BY SORT_SEQ_NO");
