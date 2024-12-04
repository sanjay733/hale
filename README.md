  public ActionForward viewAll(ActionMapping mapping, ActionForm form, HttpServletRequest request, HttpServletResponse response)
    {
        String target = "loadModalPopupSuccess";
        SelectionPopupDAO selectionPopupDAO = new SelectionPopupDAO();
        SelectionPopupValueObject selectionPopupVO = null;
        ModalSelectionPopupForm modalselectionPopupForm = null;
        StringBuffer htmlStr = new StringBuffer();
        boolean buildTable = false;
        boolean displayColumn = false;
        
        String valueStr = "";
        String[] searchColCaptionArray = null;
        String[] searchFilterArray = null;
        String[] searchValues = null;
        String[] sortColsCaptionsArray = null;
        String[] sortColsArray = null;
        String[] imagePathArray = null;
        String[] invokeFunctionArray = null;
        int searchCntlCount = 0;
        String searchCntlNames = "";
        String searchCntl = "";
        String dataType = "";
        int gridDataSize =0;
        int recordSize = 0;
        int displayColArraySize = 0;
        int populateColArraySize = 0;
        int columnCaptionArraySize = 0;
        ArrayList resultArrayList = null;
        HttpSession session = null;
        String appId = "";
        
        try
        {
            isSessionExpired(request, response);
            modalselectionPopupForm = (ModalSelectionPopupForm) form;
            session = request.getSession(false);
            appId = (String) session.getAttribute(SessionConstants.APP_ID);

            selectionPopupVO = modalselectionPopupForm.createValueObject();

            logger.debug("POPUP QRY INDEX::::>'" + selectionPopupVO.getQueryIndex()+"'");
            logger.debug("POPUP QRY :::::::::>'" + SQLQuery.getQuery(STKGeneral.getInteger(selectionPopupVO.getQueryIndex())) + "'");
            String clientId         = selectionPopupVO.getClientId();
            System.out.println(clientId);

            ArrayList gridData = null;
            
            if(selectionPopupVO.getPagination() != null && selectionPopupVO.getPagination().equals(Constant.YES))
            {
            	
            	String startSeqNoStr 	= selectionPopupVO.getStartSeqNo();
                String endSeqNoStr 		= selectionPopupVO.getEndSeqNo();
                String rowCountStr 		= selectionPopupVO.getRowCount();
                String startSeqNo 		= "0";
                String endSeqNo 		= "0";
                int rowCount 			= 0;

                if (startSeqNoStr != null && startSeqNoStr.trim().length() > 0)
                    startSeqNo = startSeqNoStr;

                if (endSeqNoStr != null && endSeqNoStr.trim().length() > 0)
                    endSeqNo = endSeqNoStr;

                if (rowCountStr != null && rowCountStr.trim().length() > 0)
                    rowCount = STKGeneral.getInteger(rowCountStr);

                String navigationValue = selectionPopupVO.getNavigationValue();
                resultArrayList = new ArrayList();
                String sqlQuery = SQLQuery.getQuery(STKGeneral.getInteger(selectionPopupVO.getQueryIndex())).toUpperCase();
                resultArrayList = selectionPopupDAO.getQueryResult(selectionPopupVO.getFltrValsDataType(), sqlQuery, selectionPopupVO
                         .getSourceMode(), selectionPopupVO.getSearchFltrValsDataType(), selectionPopupVO.getListAll(),
                         selectionPopupVO.getPaginationKeyField(),sqlQuery,startSeqNo, endSeqNo, rowCount, navigationValue, selectionPopupVO.getPageNo());
                gridData =(ArrayList) resultArrayList.get(0);
            	 
            }
            else
            {
            	gridData = selectionPopupDAO.getQueryResult(selectionPopupVO.getFltrValsDataType(), SQLQuery.getQuery(STKGeneral.getInteger(selectionPopupVO.getQueryIndex())), selectionPopupVO
                        .getSourceMode(), selectionPopupVO.getSearchFltrValsDataType(), selectionPopupVO.getListAll());
            }
