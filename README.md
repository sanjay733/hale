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
            
            modalselectionPopupForm.setGridData(gridData);
            ArrayList columnCaptionArray = selectionPopupVO.getColumnCaptionArray();
            ArrayList displayColArray = selectionPopupVO.getDisplayColArray();
            ArrayList populateColArray = modalselectionPopupForm.getPopulateColArray();
            String argumentStr = selectionPopupVO.getArgumentStr();
            String SearchColsCaptions = selectionPopupVO.getSearchColsCaptions();
            String SearchFltrValsDataType = selectionPopupVO.getSearchFltrValsDataType();

            if (!STKGeneral.nullCheck(SearchColsCaptions).trim().equals(""))
            {
                searchColCaptionArray = STKGeneral.split(STKGeneral.nullCheck(SearchColsCaptions).trim(), Constant.DELIMITER_CARRET);
                if (searchColCaptionArray != null && searchColCaptionArray.length > 0)
                {
                    //htmlStr.append("<DIV STYLE=\"position: absolute; width: 100%; float: center; z-index: 2; top: 50px; left: 30px\">");
                    
                    htmlStr.append("<TABLE align=\"center\" cellspacing=2 cellpadding=2 border=0>");
                    htmlStr.append("<TR >");
                    searchCntlCount = searchColCaptionArray.length;
                    for (int col = 0; col < searchCntlCount; col++)
                    {
                        htmlStr.append("<TD ><FONT class=\"LblFont\">").append(STKGeneral.decodeString(searchColCaptionArray[col], Constant.DECODE_HTML)).append("</FONT></TD>");
                    }
                    htmlStr.append("</TR>");
                    htmlStr.append("<TR >");

                    searchFilterArray = STKGeneral.split(STKGeneral.nullCheck(SearchFltrValsDataType).trim(), Constant.DELIMITER_CARRET);
                    for (int col = 0; col < searchCntlCount; col++)
                    {
                        searchCntl = STKGeneral.squeezeChar(STKGeneral.squeezeChar(searchColCaptionArray[col].trim(), " "), ":");
                        htmlStr.append("<TD><INPUT TYPE='TEXT' NAME='").append(searchCntl).append("' style=\"TEXT-TRANSFORM: uppercase\"></TD>");
                        if (searchFilterArray != null && searchFilterArray.length > 0 && col < searchFilterArray.length)
                        {
                            searchValues = STKGeneral.split(STKGeneral.nullCheck(searchFilterArray[col]).trim(), Constant.DELIMITER_PIPE);
                        }

                        if (searchValues != null && searchValues.length <= 2)
                        {
                            htmlStr.append("<SCRIPT LANGUAGE=javascript>");
                            htmlStr.append("searchControl = eval('document.modalselectionPopupForm.").append(searchCntl).append("'); ");
                            htmlStr.append("if (trim(document.modalselectionPopupForm.listAll.value) != YES)");
                            htmlStr.append(" searchControl.value = '").append(STKGeneral.decodeString(searchValues[0], Constant.DECODE_SCRIPT)).append("';");
                            htmlStr.append(" else ");
                            htmlStr.append("searchControl.value = ''; ");
                            htmlStr.append("if (trim(document.modalselectionPopupForm.searchFilterDataType.value) == '')");
                            htmlStr.append(" document.modalselectionPopupForm.searchFilterDataType.value ='").append(STKGeneral.nullCheck(searchValues[1]).trim()).append("';");
                            htmlStr.append(" else ");
                            htmlStr.append(" document.modalselectionPopupForm.searchFilterDataType.value += '").append(Constant.DELIMITER_PIPE).append(STKGeneral.nullCheck(searchValues[1]).trim()).append(
                                    "';");
                            htmlStr.append("</SCRIPT>");
                        }

                        if (searchCntlNames.trim().equals(""))
                            searchCntlNames += searchCntl;
                        else
                            searchCntlNames += Constant.DELIMITER_CARRET + searchCntl;
                    }
                    htmlStr.append("</TR></TABLE>");
                    //htmlStr.append("</DIV>");
                    //htmlStr.append("<BR>");
                }
            }

            
            htmlStr.append("<TABLE ALIGN=CENTER border=0 cellspacing=5 cellpadding=5>");
            htmlStr.append("<TR ALIGN='top'>");

            if (!STKGeneral.nullCheck(SearchColsCaptions).trim().equals(""))
            {
                htmlStr.append("<TD align=center>");
				htmlStr.append("<input name=\"btnList\" style=\"width:80px\" type=\"button\" class=\"action\" "); 
				htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\" ");
				htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\" value=\"List\" onclick=\"javascript:List_Click('").append(STKGeneral.decodeString(searchCntlNames, Constant.DECODE_SCRIPT)).append("','");
                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getFormName(), Constant.DECODE_SCRIPT)).append("','");
                htmlStr.append(Constant.DELIMITER_CARRET).append("')\" Title=\"List\">");
                htmlStr.append("</TD>");
		
                htmlStr.append("<TD align=center>");
				htmlStr.append("<input name=\"btnListAll\" style=\"width:80px\" type=\"button\" class=\"action\" "); 
				htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\" ");
				htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\" value=\"List All\" onclick=\"javascript:ListAll_Click('").append(STKGeneral.decodeString(searchCntlNames, Constant.DECODE_SCRIPT)).append("','");
                htmlStr.append(Constant.DELIMITER_CARRET).append("')\" Title=\"List All\">");
                htmlStr.append("</TD>");
		
            }

            if (selectionPopupVO.getClearControl().trim().equals(Constant.CHAR_YES))
            {
                //                htmlStr.append("<TD align=center><A
                // HREF=\"Javascript:setValues('','");
                //                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getFormName(),
                // Constant.DECODE_SCRIPT)).append("','");
                //                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getCntrlName(),
                // Constant.DECODE_SCRIPT)).append("','");
                //                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getCallParentfunct(),
                // Constant.DECODE_SCRIPT)).append("','");
                //                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getCallPopupArg(),
                // Constant.DECODE_SCRIPT)).append("','");
                //                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getPopData(),
                // Constant.DECODE_SCRIPT)).append("','").append(Constant.CHAR_YES).append("
                // ','");
                //                htmlStr.append(Constant.DELIMITER_CARRET).append("')\"><IMG
                // border=0
                // src=\"").append(request.getContextPath()).append("/images/btnReset.gif
                // \"").append(" title=\"Reset\"> </A></TD>");
            }
           
            htmlStr.append("<TD align=center>");
			htmlStr.append("<input name=\"btnClose\" style=\"width:80px\" type=\"button\" class=\"action\" "); 
			htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\" ");
			htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\" value=\"Close\" onclick=\"javascript:window_close();\" Title=\"Close\"> </TD>");
            htmlStr.append("</TR>");
            htmlStr.append("</TABLE>");
            
            
            // *********************  For pagination  **********************************// 
            if(selectionPopupVO.getPagination() != null && selectionPopupVO.getPagination().trim().equals(Constant.YES))
            {
				
            	if (resultArrayList != null && resultArrayList.size() > 1)
 	            {
 	            	modalselectionPopupForm.setNavigationPage	((String) resultArrayList.get(6));
 	            }
            	htmlStr.append("<DIV POSITION: absolute; WIDTH:99%; z-index:1; BACKGROUND: visible; left:10px;\">");
            	htmlStr.append("<TABLE align=right border=0>");
            	htmlStr.append("<TR>");
				htmlStr.append("<TD align=right><FONT class=LblFontMand>").append(STKGeneral.nullCheck(modalselectionPopupForm.getNavigationPage())).append("</FONT> </TD>");
				htmlStr.append("<TD>");
				htmlStr.append("<TABLE align=right border=0>");
				htmlStr.append("<TR>");
				htmlStr.append("<TD>");
				htmlStr.append("<input name=\"btnFirst\" type=\"button\" class=\"action\" style=\"width:25px\" ");
				htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\" "); 
				htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\" ");
				htmlStr.append("value=\"|&lt;\" ");
				htmlStr.append("onclick=\"javascript:onClickFirst();\" title=\"First\" > ");			
				htmlStr.append("</TD>");
				htmlStr.append("<TD>");
				htmlStr.append("<input name=\"btnPrevious\" type=\"button\" class=\"action\" style=\"width:20px\" ");
				htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\" ");
				htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\" ");
				htmlStr.append("value=\"&lt;\" ");
				htmlStr.append("onclick=\"javascript:onClickPrevious();\" title=\"Previous\" > ");			
				htmlStr.append("</TD> ");
				htmlStr.append("<TD> ");
				htmlStr.append("<input name=\"btnNext\" type=\"button\" class=\"action\" style=\"width:20px\" ");
				htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\"  ");
				htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\"  ");
				htmlStr.append("value=\"&gt;\" ");
				htmlStr.append("onclick=\"javascript:onClickNext();\" title=\"Next\" > ");			
				htmlStr.append("</TD> ");
				htmlStr.append("<TD> ");
				htmlStr.append("<input name=\"btnLast\" type=\"button\" class=\"action\" style=\"width:25px\" ");
				htmlStr.append("onmouseover=\"javascript:buttonOnMouseOver(this,").append("'"+appId+"')\"  "); 
				htmlStr.append("onmouseout=\"javascript:buttonOnMouseOut(this,").append("'"+appId+"')\"  ");
				htmlStr.append("value=\"&gt;|\" ");
				htmlStr.append("onclick=\"javascript:onClickLast();\" title=\"Last\" > "); 			
				htmlStr.append("</TD> "); 
				htmlStr.append("</TR> "); 
				htmlStr.append("</TABLE> ");
				htmlStr.append("</TR> ");
				htmlStr.append("</TABLE>");
				htmlStr.append("</DIV>");
            }
            // *************************************************************************//
            
            htmlStr.append("<DIV class=\"DivGrid\" STYLE=\" BORDER-RIGHT: 12px; OVERFLOW: auto;  BORDER-BOTTOM: 12px;");
            htmlStr.append("POSITION: absolute; WIDTH:99%; z-index:1; HEIGHT: 170pt;");
            if(selectionPopupVO.getPagination() != null && selectionPopupVO.getPagination().trim().equals(Constant.YES))
            {
            	htmlStr.append(" TOP: 170px; HEIGHT: 250pt;");
            }
            htmlStr.append(" BACKGROUND: visible; left:10px\">");
            htmlStr.append("<TABLE border=\"1\" cellspacing=\"0\" cellpadding=\"5\" width=100% align=\"center\">");
            htmlStr.append("<TR >");
            columnCaptionArraySize = columnCaptionArray.size();
            if (columnCaptionArray != null && columnCaptionArraySize > 0)
            {
                java.util.Iterator it = columnCaptionArray.iterator();
                while (it.hasNext())
                {
                    htmlStr.append("<TH  ALIGN=CENTER class=\"gridHdrTH\">").append(it.next()).append("</TH>");
                }
            }

            htmlStr.append("</TR>");

            if (gridData != null && gridData.size() > 0)
            {
                java.util.ArrayList record = null;
                gridDataSize = gridData.size();
                for (int rowCount = 0; rowCount < gridDataSize; rowCount++)
                {
                    record = (java.util.ArrayList) gridData.get(rowCount);
                    recordSize = record.size();
                    if (record != null && recordSize > 0)
                    {
                        argumentStr = "";

                        for (int colCount = 0; colCount < recordSize; colCount++)
                        {
                            //valueStr = STKGeneral.nullCheck((String) record.get(colCount)).trim();
                            if (record.get(colCount) != null)
                                dataType = STKGeneral.nullCheck((String) (Object) (record.get(colCount)).getClass().getName()).trim();

                            if (dataType.equals("java.sql.Timestamp") && record.get(colCount) != null)
                                valueStr = DateUtil.formatDisplayDate(((java.sql.Timestamp) record.get(colCount)).toString());
                            else if (dataType.equals("java.sql.Date") && record.get(colCount) != null)
                                valueStr = DateUtil.formatDisplayDate(((java.sql.Date) record.get(colCount)).toString());
                            else
                            {
                                valueStr = STKGeneral.nullCheck((String) record.get(colCount)).trim();
                                
                                // E1253 : If the value is blank, then replace it with " "
                                
                                if(valueStr.equals(""))
                                    valueStr = " ";
                            }

                            if (argumentStr.equals(""))
                                argumentStr = valueStr;
                            else
                                argumentStr += Constant.DELIMITER_CARRET + valueStr;
                        }

                        htmlStr.append("<TR onmouseover=\"Javascript:this.style.cursor = 'hand';\" onmouseout=\"Javascript:this.style.cursor = 'default';\" ");
                                htmlStr.append("onClick=\"Javascript:setValues('");
                                htmlStr.append(STKGeneral.decodeString(argumentStr, Constant.DECODE_HREF)).append("','");
                                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getFormName(), Constant.DECODE_HREF)).append("','");
                                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getCntrlName(), Constant.DECODE_HREF)).append("','");
                                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getCallParentfunct(), Constant.DECODE_HREF)).append("','");
                                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getCallPopupArg(), Constant.DECODE_HREF)).append("','");
                                htmlStr.append(STKGeneral.decodeString(selectionPopupVO.getPopData(), Constant.DECODE_HREF)).append("','");
                                htmlStr.append(Constant.CHAR_NO).append("','");
                                htmlStr.append(Constant.DELIMITER_CARRET).append("')\">");

                        for (int colCount = 0; colCount < recordSize; colCount++)
                        {
                            displayColumn = false;                            

                            if (record.get(colCount) != null)
                                dataType = STKGeneral.nullCheck((String) (Object) (record.get(colCount)).getClass().getName()).trim();

                            if (dataType.equals("java.sql.Timestamp") && record.get(colCount) != null)
                            {
                            	valueStr = DateUtil.formatDisplayDate(((java.sql.Timestamp) record.get(colCount)).toString());
                            }
                            else if (dataType.equals("java.sql.Date") && record.get(colCount) != null)
                            {
                                valueStr = DateUtil.formatDisplayDate(((java.sql.Date) record.get(colCount)).toString());
                            }
                            else
                                valueStr = STKGeneral.nullCheck((String) record.get(colCount)).trim();

                            if (displayColArray != null && populateColArray != null)
    						{
                                displayColArraySize = displayColArray.size();
                                populateColArraySize = populateColArray.size();
    							for(int i=0; i < displayColArraySize; i++)
    							{  							       								
    								if (colCount < populateColArraySize && STKGeneral.nullCheck(displayColArray.get(i).toString()).trim().equals(STKGeneral.nullCheck(populateColArray.get(colCount).toString()).trim()))
    								{
    									displayColumn = true;
    									break;
    								}
    							}
    						}
                            if(displayColumn)
                            	htmlStr.append("<TD class=gridDataTD>").append(STKGeneral.decodeString(valueStr, Constant.DECODE_HTML)).append("</TD>");
                        }
                        htmlStr.append("</TR>");
                    }
                }
            }
            else if (buildTable && (selectionPopupVO.getRefreshOnLoad().trim().equalsIgnoreCase(Constant.CHAR_YES) || selectionPopupVO.getSourceMode().trim().equalsIgnoreCase(Constant.SEARCH_MODE)))
            {
                htmlStr.append("<TR>");
                htmlStr.append("<TD COLSPAN=").append(columnCaptionArraySize).append(" ALIGN=center><FONT face='ARIAL' color=RED size=2><B>").append(Constant.NO_RECORDS_FOUND);
                htmlStr.append("</B></FONT></TD>");
                htmlStr.append("</TR>");
            }
            htmlStr.append("</TABLE>");
            htmlStr.append("</DIV>");
            //logger.debug("The Final Html String is "+htmlStr);
            
            
            if(selectionPopupVO.getPagination() != null && selectionPopupVO.getPagination().trim().equals(Constant.YES))
            {
	            if (resultArrayList != null && resultArrayList.size() > 1)
	            {
	            	//modalselectionPopupForm.setAbaInfoList		((ArrayList) gridData.get(0));
	            	modalselectionPopupForm.setStartSeqNo		((String) resultArrayList.get(1));
	            	modalselectionPopupForm.setEndSeqNo			((String) resultArrayList.get(2));
	            	modalselectionPopupForm.setRowCount			((String) resultArrayList.get(3));
	            	modalselectionPopupForm.setMinSeqNo			((String) resultArrayList.get(4));
	            	modalselectionPopupForm.setMaxSeqNo			((String) resultArrayList.get(5));
	            	modalselectionPopupForm.setNavigationPage	((String) resultArrayList.get(6));
	            	modalselectionPopupForm.setPageNo			((String) resultArrayList.get(7));
	            }
	            if (modalselectionPopupForm.getMinSeqNo() == null || modalselectionPopupForm.getStartSeqNo().equals(modalselectionPopupForm.getMinSeqNo()))
	            	modalselectionPopupForm.setNavigationControlFirst(Constant.IS_FIRST);
	            else
	            	modalselectionPopupForm.setNavigationControlFirst(Constant.NORMAL);
	
	            if (modalselectionPopupForm.getMaxSeqNo() == null || modalselectionPopupForm.getEndSeqNo().equals(modalselectionPopupForm.getMaxSeqNo()))
	            	modalselectionPopupForm.setNavigationControlLast(Constant.IS_LAST);
	            else
	            	modalselectionPopupForm.setNavigationControlLast(Constant.NORMAL);
	            if(modalselectionPopupForm.getMode() == null || (!modalselectionPopupForm.getMode().equals(Constant.UPDATE_MODE)))
	            	modalselectionPopupForm.setMode(Constant.INSERT_MODE);
            }
            
            request.setAttribute("htmlStr", htmlStr.toString());
            request.setAttribute("queryIndex", modalselectionPopupForm.getQueryIndex());
            request.setAttribute("fltrValsDataType", modalselectionPopupForm.getFltrValsDataType());
            request.setAttribute("formcaption", modalselectionPopupForm.getFormcaption());
            request.setAttribute("columnCaption", modalselectionPopupForm.getColumnCaption());
            request.setAttribute("popData", modalselectionPopupForm.getPopData());
            request.setAttribute("formName", modalselectionPopupForm.getFormName());
            request.setAttribute("cntrlName", modalselectionPopupForm.getCntrlName());
            request.setAttribute("linkReqd", modalselectionPopupForm.getLinkReqd());
            request.setAttribute("callParentfunct", modalselectionPopupForm.getCallParentfunct());
            request.setAttribute("callPopupArg", modalselectionPopupForm.getCallPopupArg());
            request.setAttribute("searchCols", modalselectionPopupForm.getSearchCols());
            request.setAttribute("searchColsCaptions", modalselectionPopupForm.getSearchColsCaptions());
            request.setAttribute("refreshOnLoad", modalselectionPopupForm.getRefreshOnLoad());
            request.setAttribute("searchFltrValsDataType", modalselectionPopupForm.getSearchFltrValsDataType());
            request.setAttribute("displayCols", modalselectionPopupForm.getDisplayCols());
            request.setAttribute("clearControl", modalselectionPopupForm.getClearControl());
            request.setAttribute("sourceMode", modalselectionPopupForm.getSourceMode());
            request.setAttribute("listAll", modalselectionPopupForm.getListAll());
            request.setAttribute("pagination", modalselectionPopupForm.getPagination());
            request.setAttribute("paginationKeyField", modalselectionPopupForm.getPaginationKeyField());
            
            if(modalselectionPopupForm.getSearchfilterValues()==null)
            {
            	modalselectionPopupForm.setSearchfilterValues(modalselectionPopupForm.getSearchFltrValsDataType());
            }
        }
        catch (SessionExpiredException se)
        {
            target = "popupSessionExpired";
            super.handleUserException(request, se, "Error while executing viewAll method : ");
        }
        catch (Exception e)
        {
            target = "fatalException";
            target = super.handleUnhandledException(request, e, target, "Error while executing viewAll method : ");
        }

        return mapping.findForward(target);
    }
}
