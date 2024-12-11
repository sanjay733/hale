
SELECT DISTINCT TEMPLATE_ID, (DOC_DESC + ' ' + CLIENT_ID) DOC_DESC, SORT_SEQ_NO
FROM DOC_TEMPLATE WHERE TEMPLATE_ID NOT IN ('PWI_AGREEMENT_DOC') AND 
TEMPLATE_ID LIKE '%' AND DOC_DESC LIKE '%' AND CLIENT_ID= 'ATP001' ORDER BY SORT_SEQ_NO
