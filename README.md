# hale

<%@ page import="com.softeon.eso.shared.utils.STKGeneral" %>
<%@ page import="com.softeon.eso.shared.utils.Constant" %>
<%@ page import="com.softeon.eso.shared.utils.SessionConstants" language="java" %> 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<%@ taglib uri="/WEB-INF/struts-html.tld" prefix="html"%>
<%@ taglib uri="/WEB-INF/struts-bean.tld" prefix="bean"%>
<%@ taglib uri="/WEB-INF/struts-logic.tld" prefix="logic"%>

<jsp:useBean id="htmlStr" scope="request" type="java.lang.String" />
<jsp:useBean id = "queryIndex"              scope = "request" class = "java.lang.String" />
<jsp:useBean id = "fltrValsDataType"        scope = "request" class = "java.lang.String" />
<jsp:useBean id = "formcaption"             scope = "request" class = "java.lang.String" />
<jsp:useBean id = "columnCaption"           scope = "request" class = "java.lang.String" />
<jsp:useBean id = "popData"                 scope = "request" class = "java.lang.String" />
<jsp:useBean id = "formName"                scope = "request" class = "java.lang.String" />
<jsp:useBean id = "cntrlName"               scope = "request" class = "java.lang.String" />
<jsp:useBean id = "linkReqd"                scope = "request" class = "java.lang.String" />
<jsp:useBean id = "callParentfunct"         scope = "request" class = "java.lang.String" />
<jsp:useBean id = "callPopupArg"            scope = "request" class = "java.lang.String" />
<jsp:useBean id = "searchCols"              scope = "request" class = "java.lang.String" />
<jsp:useBean id = "searchColsCaptions"      scope = "request" class = "java.lang.String" />
<jsp:useBean id = "refreshOnLoad"           scope = "request" class = "java.lang.String" />
<jsp:useBean id = "searchFltrValsDataType"  scope = "request" class = "java.lang.String" />
<jsp:useBean id = "displayCols"             scope = "request" class = "java.lang.String" /> 
<jsp:useBean id = "clearControl"            scope = "request" class = "java.lang.String" />
<jsp:useBean id = "sourceMode"              scope = "request" class = "java.lang.String" />
<jsp:useBean id = "listAll"                 scope = "request" class = "java.lang.String" />
<jsp:useBean id = "pagination"              scope = "request" class = "java.lang.String" /> 
<jsp:useBean id = "paginationKeyField"      scope = "request" class = "java.lang.String" />
<jsp:useBean id = "searchfilterValues"      scope = "request" class = "java.lang.String" />
<html:html>
<head>
<meta http-equiv="X-UA-Compatible" content="IE=5,chrome=1" />
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>

<script language="JavaScript" src="<%=request.getContextPath()%>/js/messagedialog.js"></script>
<script language="JavaScript" src="<%=request.getContextPath()%>/js/GeneralUIFunctions.js"></script>
<script language="JavaScript" src="<%=request.getContextPath()%>/js/stkgenfunctions.js"></script>
<script language="JavaScript" src="<%=request.getContextPath()%>/js/constants.js"></script>    
<title>Selection Popup</title>

<BODY id="bodySilver">
<jsp:include page="/jsp/standardheader.jsp" flush="true" />
<SCRIPT>
var contextPath ="<%=request.getContextPath()%>";
</SCRIPT>
<html:form action="/modalselectionPopup.do" method="post" onsubmit="return preventMultipleFormSubmission();">
        <INPUT TYPE = HIDDEN NAME = "queryIndex"                value = "<%= STKGeneral.decodeString(queryIndex,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "fltrValsDataType"          value = "<%= STKGeneral.decodeString(fltrValsDataType,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "formcaption"               value = "<%= STKGeneral.decodeString(formcaption,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "columnCaption"             value = "<%= STKGeneral.decodeString(columnCaption,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "popData"                   value = "<%= STKGeneral.decodeString(popData,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "formName"                  value = "<%= STKGeneral.decodeString(formName,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "cntrlName"                 value = "<%= STKGeneral.decodeString(cntrlName,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "linkReqd"                  value = "<%= STKGeneral.decodeString(linkReqd,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "callParentfunct"           value = "<%= STKGeneral.decodeString(callParentfunct,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "callPopupArg"              value = "<%= STKGeneral.decodeString(callPopupArg,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "searchCols"                value = "<%= STKGeneral.decodeString(searchCols,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "searchColsCaptions"        value = "<%= STKGeneral.decodeString(searchColsCaptions,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "refreshOnLoad"             value = "<%= STKGeneral.decodeString(refreshOnLoad,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "searchFltrValsDataType"    value = "<%= STKGeneral.decodeString(searchFltrValsDataType,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "displayCols"               value = "<%= STKGeneral.decodeString(displayCols,Constant.DECODE_HTML) %>">
        <INPUT TYPE = HIDDEN NAME = "clearControl"              value = "<%= STKGeneral.decodeString(clearControl,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "sourceMode"                value = ""> 
        <INPUT TYPE = HIDDEN NAME = "listAll"                   value = "<%= STKGeneral.decodeString(listAll,Constant.DECODE_HTML) %>">   
        <INPUT TYPE = HIDDEN NAME = "searchFilterDataType"      value = "">   
        <INPUT TYPE = HIDDEN NAME = "pagination"              	value = "<%= STKGeneral.decodeString(pagination,Constant.DECODE_HTML) %>">      
        <INPUT TYPE = HIDDEN NAME = "paginationKeyField"        value = "<%= STKGeneral.decodeString(paginationKeyField,Constant.DECODE_HTML) %>">      
       
        
        <html:hidden property="navigationPage" />
        <html:hidden property="startSeqNo" />
		<html:hidden property="endSeqNo" />
		<html:hidden property="rowCount" />
		<html:hidden property="pageNo" />
		
		<html:hidden property="minSeqNo" />
		<html:hidden property="maxSeqNo" />
		
		<html:hidden property="navigationValue" />
		<html:hidden property="navigationControlFirst" />
		<html:hidden property="navigationControlLast" />
		<html:hidden property="searchfilterValues" />
        
    <SCRIPT>
function onClickFirst()
{
	document.modalselectionPopupForm.navigationValue.value = "F";
	viewDetails();
}

function onClickPrevious()
{
	document.modalselectionPopupForm.navigationValue.value = "P";
	viewDetails();
}

function onClickNext()
{
	document.modalselectionPopupForm.navigationValue.value = "N";
	viewDetails();	
}

function onClickLast()
{
	document.modalselectionPopupForm.navigationValue.value = "L";
	viewDetails();	
}

function viewDetails()
{
	with(document.modalselectionPopupForm)
	{
		var ColumnCaption;          //Column Captions
	    var frmName;                //Calling screen form name
	    var popupCaption;           //Used to Display the caption in the Popup screen
	    var PopulateData;           //Specify the fieldname(s) which is needs to be selected from the popup
	    var CntrlName;              //Specify the controls which needs to be hold the selected information 
	    var IsHyperLinkReqd;        //(for future use) set true if hyperlink is required false otherwise
	    var CallParentFunct;        //set true if popup needs to call the function available in the calling place , set false otherwise
	    var CallParentFunctWithArg; //bCallParentFunct is set to true then set the value whatever you want to this variable, which value will be passed from the popup screen to the function available in the calling place
	    var QueryIdxVal;            //Specify the Index of Query to retrieve the Corresponding Query from the QueryStringClass
	    var FltrValsDatatype;       //Set the values,Datatype for the corresponding fields you defined in your Query
	    var surl="";
	    var sparamv;
	    var swinname;
	    var ClearControl;
	    var Pagination;				//Specify "Y" to Implement Pagination else specify "N";
	    var PaginationKeyField;		//Specify Unique key field from appropriate table
	    
	    var searchCols;
	    var searchColsCaptions;
	    var refreshOnLoad;
	    var searchFltrValsDataType;   
		    
		url = contextPath+"/admin/modalselectionPopup.do?method=viewAll&queryIndex=" + queryIndex.value 
				+ "&fltrValsDataType=" + fltrValsDataType.value 
				+  "&columnCaption=" + columnCaption.value 
				+  "&popData=" + popData.value + "&formName=" 
			    + formName.value + "&cntrlName=" 
			    + cntrlName.value + "&linkReqd=" 
			    + linkReqd.value + "&formcaption=" 
			    + formcaption.value + "&callParentfunct=" 
			    + callParentfunct.value + "&callPopupArg=" 
			    + callPopupArg.value + "&displayCols=" 
			    + displayCols.value + "&clearControl=" 
			    + clearControl.value
			    + "&searchCols=" + searchCols.value + "&searchColsCaptions=" + searchColsCaptions.value 
			    + "&refreshOnLoad=" + refreshOnLoad.value 
			    + "&searchFltrValsDataType=" + searchFltrValsDataType.value
			    + "&pagination=" + pagination.value
			    + "&paginationKeyField=" + paginationKeyField.value;
			    
		//action= contextPath + "/admin/abaInfo.do?method=viewAll";
		action = url;
		submit();		
	}
}

function setValues(argumentStr,frmName,CntrlName,CallParentfunct,CallPopupArg,PopData,clearControls,delimiter)
{
    var cntrlArray;
    var controlName;
    var valueArray;
    var populateData;
    
    cntrlArray = CntrlName.split(delimiter);
    //on reset click argumentStr value is empty, hence we don't reset the value.
    if(argumentStr != "")
        valueArray = argumentStr.split(delimiter);
    else
        valueArray = '';
        
    populateData = PopData.split(delimiter);

    if (cntrlArray != null && cntrlArray.length > 0)
    {
        if (populateData != null && populateData.length > 0)
        {
            for (var i=0; i < cntrlArray.length; i++)
            {
                for (var j=0; j < populateData.length; j++)
                {
                    if (i == j)
                    {
                    	//alert("frmName1="+frmName);
                    	//alert("controlName="+cntrlArray[i]);
                    	//alert("valueArray="+valueArray[i]);
                       /*
                        controlName = eval("document." + frmName + "." + cntrlArray[i]);
                        if (clearControls != YES)
                        {
                            if (valueArray != null && valueArray.length > 0)
                            {
                                controlName.value = trim(valueArray[i]);                                
                            }
                        }
                        else
                            controlName.value = "";
                            */
                    }
                }
            }
        }
    }

	//alert("CallPopupArg="+CallPopupArg);
  /*  
    if (CallParentfunct == 'true')
        self.parent.opener.PopupCall(CallPopupArg);
    */   
  	  var retObject = new Array();
  	  retObject[0]  = cntrlArray;
  	  retObject[1]  = valueArray;
  	  retObject[2]  = CallPopupArg;
  	  window_returnValue(retObject) ;

	  //alert("retVal ="+window.returnValue)
	  window_close();
    //self.parent.close();
}

function window_returnValue(retObject){
	ie4 = document.all ? true : false
	ns4 = document.layers ? true : false
	ns4 = ie4 ? false : true;

	if(ie4)
		window.returnValue = retObject ;

	if(ns4){
		window.parent.setWindowReturnValue(retObject);
		self.parent.close();
	}
	
}
/*
function close()
{
	alert("inside close")
	window.returnValue = null;
	window_close();  
}
*/
function ListAll_Click(searchCntlNames,delimiter)
{
    var searchFilterValuesArray;
    var searchFilterData;
    with(document.modalselectionPopupForm)
    {
        searchFilterValuesArray = searchCntlNames.split(delimiter);
        if (searchFilterValuesArray != null && searchFilterValuesArray.length > 0)
        {
            for (var i=0; i < searchFilterValuesArray.length; i++)
            {
                eval(searchFilterValuesArray[i]).value = "";
            }
        }
        
        if(trim(pagination.value) != "" && trim(pagination.value) == "Y") 
        {
        	searchFilterData = searchFltrValsDataType.value.split(delimiter);
	        if (searchFilterData != null && searchFilterData.length > 0)
	        {
	            var searchData = "";
	            for (var i=0; i < searchFilterData.length; i++)
	            {
	               if(i == searchFilterData.length-1)
	               {
	               		searchFilterData[i] = "|S";	
	               }
	               else
	               {	
	               		searchFilterData[i] = "|S^";
	               }
	               searchData = searchData+searchFilterData[i];
	            }
	        }
        	startSeqNo.value 		= "";
			endSeqNo.value 			= "";
			rowCount.value 			= "";
			navigationPage.value 	= "";
			navigationValue.value 	= "";
	        startSeqNo.value	 	= "";
			endSeqNo.value 			= "";
			rowCount.value 			= "";
			pageNo.value 			= "";
			minSeqNo.value 			= "";
			maxSeqNo.value 			= "";
			searchFltrValsDataType.value = searchData;
			viewDetails();
        }
        else
        {
        	action = "<%=request.getContextPath()%>"+"/admin/modalselectionPopup.do?method=viewAll&refreshOnLoad="+YES+"&sourceMode="+SEARCH_MODE+"&listAll="+YES;//+"&searchFltrValsDataType="+searchFilterValues;
	        submit();	
        }
        
    }
}

function List_Click(searchCntlNames,formName,delimiter)
{
    var searchFilterValues = "";
    var searchFilterValuesArray;
    var searchFilterDataTypeArray;
    
    with(document.modalselectionPopupForm)
    {
        if (trim(searchCntlNames) != "")
        {
            searchFilterValuesArray = searchCntlNames.split(delimiter);
            searchFilterDataTypeArray = (searchFilterDataType.value).split("|");
            if (searchFilterValuesArray != null && searchFilterValuesArray.length > 0)
            {
                // Pagination
                //pagination.value = "";
                for (var i=0; i < searchFilterValuesArray.length && i < searchFilterDataTypeArray.length; i++)
                {
                    if (trim(searchFilterValues) == "")
                        searchFilterValues = eval(searchFilterValuesArray[i] + ".value");
                    else
                        searchFilterValues += delimiter + eval(searchFilterValuesArray[i] + ".value");
                    searchFilterValues += "|" + searchFilterDataTypeArray[i];
                }
            }
            
            if(trim(pagination.value) != "" && trim(pagination.value) == "Y")
            {
	            for (var i=0; i < searchFilterDataTypeArray.length; i++)
	            {
	                var isDataExists = "";
	                if(searchFilterDataTypeArray[i] == "S")
	                {
	                	isDataExists = true;
	                }
	                else
	                {
	                	isDataExists = false;
	                	break;
	                }
	            }
		         if(isDataExists)
		         {
		         	startSeqNo.value 		= "";
					endSeqNo.value 			= "";
					rowCount.value 			= "";
					navigationPage.value 	= "";
					navigationValue.value 	= "";
			        startSeqNo.value	 	= "";
					endSeqNo.value 			= "";
					rowCount.value 			= "";
					pageNo.value 			= "";
					minSeqNo.value 			= "";
					maxSeqNo.value 			= "";
					//searchFltrValsDataType.value = searchfilterValues.value;
					
					//viewDetails();
			     }
	         }
	         
	         
        }

        action = "<%=request.getContextPath()%>"+"/admin/modalselectionPopup.do?method=viewAll&refreshOnLoad="+YES+"&sourceMode="+SEARCH_MODE+"&listAll="+NO+"&searchFltrValsDataType="+escape(searchFilterValues);
        submit();
    }
}
</SCRIPT>
    <CENTER>
    <TABLE width=100% border=0 cellspacing=0 cellpadding=0>
        <TR>
            <TD class="PopupCaptionTitle" align=center><bean:write name="modalselectionPopupForm" property="formcaption" /></TD>
        </TR>
    </TABLE>
    </CENTER>

    <DIV id="outer">
    <%=htmlStr%>
    </DIV>
</html:form>
<SCRIPT type="text/javascript">
if(trim(modalselectionPopupForm.pagination.value) == "Y")
{
	valiadteNavigation();	
}
function valiadteNavigation()
{
	with(document.modalselectionPopupForm)
	{
		if(navigationControlFirst.value == "F")
		{
			disableButton(btnFirst);		
			disableButton(btnPrevious);						
		}
		else
		{
			enableButton(btnFirst);
			enableButton(btnPrevious);			
		}
		
		if(navigationControlLast.value == "L")
		{
			disableButton(btnNext);		
			disableButton(btnLast);				
		}
		else
		{
			enableButton(btnNext);
			enableButton(btnLast);			
		}
	}
}
</SCRIPT>
</BODY>
</html:html>
