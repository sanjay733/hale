  sqlQueryList.set(LETTER_TEMPLATE_LIST," SELECT TEMPLATE_ID, (DOC_DESC + ' ' + CLIENT_ID) DOC_DESC, SORT_SEQ_NO FROM " + TableConstants.DOC_TEMPLATE + // ALETFILE-25
        " WHERE TEMPLATE_ID NOT IN ('AGREEMENT_TEMPL', 'PWI_AGREEMENT_DOC') AND TEMPLATE_ID LIKE ? AND DOC_DESC LIKE ? ORDER BY SORT_SEQ_NO ");


SELECT DISTINCT TEMPLATE_ID, (DOC_DESC + ' ' + d.CLIENT_ID) DOC_DESC, 
SORT_SEQ_NO FROM DOC_TEMPLATE d inner join client c on d.client_id = 'ATP001' WHERE TEMPLATE_ID NOT IN ('PWI_AGREEMENT_DOC') 
 AND TEMPLATE_ID LIKE ? AND DOC_DESC LIKE ? ORDER BY SORT_SEQ_NO
