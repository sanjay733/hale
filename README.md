var focusCtrl;
var focusSet = false;
var error="";
var customTextValue="";

function onFormLoad()
{
	with(document.templateCodesForm)
	{
        Disable(tempDesc,customText,clientId,clientDesc,planType,planTypeDesc);
        disableButton(btnDelete);        
        if(trim(mode.value)==uMode)
        {
        	Disable(tempDesc,tempCD,tempID);
            tempCDDesc.focus();
        }
        else
        {
            Enable(tempCD);
        }
        
        if(tblDataGrid.rows.length > 1 && tblDataGrid.rows[1].cells.length > 1)
	    {
	        if((mode.value == uMode) || (mode.value == iMode))
	        {
	            for(i = 1; i < tblDataGrid.rows.length; i++)  
	            {        
	                if((tempID.value == trimInnerText(tblDataGrid.rows[i].cells[0])&& tempCD.value == trimInnerText(tblDataGrid.rows[i].cells[1])))   
	                {
	                    highlightGridRow(tblDataGrid.rows[i]);  // The row is highlighted
	                    //onGridDataClick(tblDataGrid.rows[i]);
	                    tblDataGrid.rows[i].scrollIntoView(false);  // This will bring the highlighted row into view if it is not in the first page itself.	                                           
	                }
	            }
	        }
	    }
        if(customTextCheck.checked)
        {
        	Enable(customText);
        }
        tempID.onkeypress=AcceptChars;
        tempCD.onkeypress=AcceptChars;
        clientId.onkeypress=AcceptChars;
	}
}
function MessageBoxDetailCall(btnId,msgId)
{
    with(document.templateCodesForm)
    {
         if(trim(msgId) == BTN_INSERT_SUBMIT) 
         {
            if(trim(btnId) == MSG_OK)
            {
                if(focusCtrl != null && focusCtrl)  
                        focusCtrl.focus();          
            }
         }
         else if(trim(msgId) == BTN_FIND) 
         {
            if(trim(btnId) == MSG_OK)
            {
                if(focusCtrl != null && focusCtrl)  
                        focusCtrl.focus();          
            }
         }         
         else if(trim(msgId) == BTN_DELETE) 
         {
            if(trim(btnId) == MSG_YES)
            {
                action = contextPath+"/admin/templateCodes.do?method=deleteRecord";
                submit();
            }
         }
    } 
}

function onClick_Submit()
{
    var dlgType=OKONLY; 
    var srcbtn = BTN_INSERT_SUBMIT;
    error = "";
	with(document.templateCodesForm)
	{
        if(customTextCheck.checked)
            customTextCheck.value=YES;
        else
            customTextCheck.value=NO;
            
		if (trim(tempID.value)=="")
        {
            focusCtrl = tempID;
            focusSet = true;
            error+=","+TEMPLATE_ID_MANDATORY;
        }
        else if(!specialcheck(trim(tempID.value)))
        {
            focusCtrl = tempID;
            focusSet = true;
            error+=","+TEMPLATE_ID_SPECIAL;
        }
        if (trim(tempCD.value)=="")
        {
            if(!focusSet)
            {
                focusCtrl = tempID;
                focusSet = true;
            }
            error+=","+TEMPLATE_CD_MANDATORY;
        }
        else if (!specialcheck(trim(tempCD.value)))
        {
            if(!focusSet)
            {
                focusCtrl = tempID;
                focusSet = true;
            }
            error+=","+TEMPLATE_CD_SPECIAL;;
        }
        if(customTextCheck.checked)
        {
            if(trim(customText.value)=="")
            {
	            if(!focusSet)
	            {
	                focusCtrl = customText;
	                focusSet = true;
	            }
	            error+=","+CUSTOM_TEXT_MANDATORY;
                isDataChanged=false;
            }
        }
        
        if(error.length >0)
        {   
            invokeMessagePopup(error,dlgType,srcbtn);       
        }
        else if(mode.value == uMode)
        {
            if(isDataChanged)
            {
            	if(showSysCodes.checked)
        		{
        			showSysCodes.value="Y";
        		}
        		else
        		{
        			showSysCodes.value="N";
        		}
            	//customText.value = escape(customText.value);
                action = contextPath+"/admin/templateCodes.do?method=updateRecord";
                submit();
            }
            else
            {
                if(!focusSet)
                {
                    focusCtrl=tempID;
                    focusSet = true;
                }
                error += DATA_NTCHANGE;       
                invokeMessagePopup(error,dlgType,srcbtn);    
            }
        }
        else if(mode.value == iMode)
        {
            if(trim(tempDesc.value)=="")
            {
                error+= TEMPLATE_ID_INVALID;
                invokeMessagePopup(error,dlgType,srcbtn);    
            }
            else
            {   
            	if(showSysCodes.checked)
        		{
        			showSysCodes.value="Y";
        		}
        		else
        		{
        			showSysCodes.value="N";
        		}
            	//customText.value = escape(customText.value);
                action = contextPath+"/admin/templateCodes.do?method=createRecord";
                submit();
            }
        }
	}
}
function onClick_Find()
{   
    var dlgType=OKONLY; 
    var srcbtn = BTN_FIND;
    error = "";
    with(document.templateCodesForm)
    {
        if (trim(tempID.value)=="")
        {
            focusCtrl = tempID;
            focusSet = true;
            error+=","+TEMPLATE_ID_MANDATORY;
        }
        else if(!specialcheck(trim(tempID.value)))
        {
            focusCtrl = tempID;
            focusSet = true;
            error+=","+TEMPLATE_ID_SPECIAL;
        }
        if(error.length >0)
        {   
            invokeMessagePopup(error,dlgType,srcbtn);       
        }
        else
        {
            if(trim(tempDesc.value)=="")
            {
                focusCtrl = tempID;
                error+=TEMPLATE_ID_MANDATORY;
                invokeMessagePopup(error,dlgType,srcbtn);
            }
            else
            {
            	if(showSysCodes.checked)
        		{
        			showSysCodes.value="Y";
        		}
        		else
        		{
        			showSysCodes.value="N";
        		}
                action = contextPath+"/admin/templateCodes.do?method=findRecord";
                submit();
            }    
        }
    }
}
function onClick_Delete()
{
    
    var dlgType=YESNO; 
    var srcbtn =BTN_DELETE;
    error ="";
    error+=DELETE_CONFIRM;
    invokeMessagePopup(error,dlgType,srcbtn);    
    
}
function onClick_Refresh()
{
	with(document.templateCodesForm)
	{
		if(showSysCodes.checked)
		{
			showSysCodes.value="Y";
		}
		else
		{
			showSysCodes.value="N";
		}
		tempID.value = "";
		tempDesc.value = "";
		tempCD.value = "";
		tempCDDesc.value = "";
		customTextCheck.checked = false;
        customText.value="";
        clientId.value = "";
        clientDesc.value = "";
        planType.value = ""; 
        planTypeDesc.value = "";
		action = contextPath+"/admin/templateCodes.do?method=viewAll";
		submit();
	}
}

function onClick_Reset()
{
	with(document.templateCodesForm)
	{
		tempID.value = "";
		tempDesc.value = "";
		tempCD.value = "";
		tempCDDesc.value = "";
		clientId.value = "";
        clientDesc.value = "";
        planType.value = ""; 
        planTypeDesc.value = "";
		customTextCheck.checked = false;
		customText.value="";
        Enable(tempID,tempCD); 
        mode.value = iMode;
        Disable(clientId,clientDesc,customText,planType);
        tempID.focus();
        showSysCodes.checked=false;
       	showSysCodes.value="N";
	}
}

function onClick_TempID()
{
	with(document.templateCodesForm)
	{
	 	var dlgType= OKONLY;	
		var srcbtn = BTN_INSERT_SUBMIT;
		var error="";
		
		
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
	    
	    ClearControl            = YES;
	    ColumnCaption           = "Template ID^Description";  
	    frmName                 = templateCodesForm.name; 
	    popupCaption            = "Template Selection Popup";
	    PopulateData            = "TEMPLATE_ID^DOC_DESC";
	    displayCols             = "TEMPLATE_ID^DOC_DESC";
	    CntrlName               = tempID.name+ "^"+ tempDesc.name;
	    IsHyperLinkReqd         = true; 
	    CallParentFunct         = true;  
	    CallParentFunctWithArg  = TEMPLATE_ID_QRY_INDEX;
	    QueryIdxVal             = TEMPLATE_ID_QRY_INDEX;
	    FltrValsDatatype        = "";
		var searchCols;
	    var searchColsCaptions;
	    var refreshOnLoad;
	    var searchFltrValsDataType;   
	    
	    searchCols              = "TEMP_ID^DOC_DESC";
		searchColsCaptions      = "Template ID^Description";
	    refreshOnLoad           = YES;
	    searchFltrValsDataType  = trim(tempID.value) + "|S^" +trim(tempDesc.value)+"|S";
	    
	    url= contextPath+"/admin/modalselectionPopup.do?method=viewAll&queryIndex=" 
		    + QueryIdxVal + "&fltrValsDataType=" + FltrValsDatatype +  "&columnCaption=" 
		    + ColumnCaption +  "&popData=" + PopulateData + "&formName=" 
		    + frmName + "&cntrlName=" + CntrlName + "&linkReqd=" 
		    + IsHyperLinkReqd + "&formcaption=" + popupCaption + "&callParentfunct=" 
		    + CallParentFunct + "&callPopupArg=" + CallParentFunctWithArg + "&displayCols=" 
		    + displayCols + "&clearControl=" + ClearControl
		    + "&searchCols=" + searchCols + "&searchColsCaptions=" + searchColsCaptions 
		    + "&refreshOnLoad=" + refreshOnLoad 
		    + "&searchFltrValsDataType=" + searchFltrValsDataType;
		    
		var width = 500;
	    var height = 400;
	    var winName = "Template Info";
	    var parentObj = new Array();
	    parentObj[0] = url;
	    parentObj[1] = "Template Selection Popup";
		var urlHTML= contextPath+"/jsp/ModalSelectionPopup.html";
		var returnPromiseValue  = _showModalDialog(urlHTML,parentObj,"status:no;dialogTop:100px;dialogLeft:100px;dialogWidth:"+width+"px;dialogHeight:"+height+"px;help:no;center:no;scrollbars:yes");				
		returnPromiseValue.then(function(returnValue){
			if(returnValue !=null )	
			{	
				if(returnValue[0] != null && returnValue[0].length > 0)
				{
					// After Session Expiry redirect to Home page
					if(trim(returnValue[0])==IS_SESSION_EXPIRED_STR)	
					{	
						self.parent.window.location = contextPath+"/admin/login.do?method=home";
					}
					else
					{
						var cntrlArray = new Array();
						var valueArray = new Array();
					  	cntrlArray = returnValue[0];
				  	  	valueArray = returnValue[1];
				  		for(i=0;i<cntrlArray.length ;i++)
		  	  			{		  	  
							eval("document.templateCodesForm."+cntrlArray[i]).value = valueArray[i];					
						}
						PopupCall(returnValue[2]);	
					}
				}
	  		}
		});
	}
}



function onClick_PlanType()
{
	 
	with(document.templateCodesForm)
	{
		if(!customTextCheck.checked)
        {
			return;
        }
		if(planType.readOnly)
		{
			return;
		}
		if(mode.value == uMode || tempID.value!='AGREEMENT_TEMPL')
		{
			return;
		}
			
		
	 	var dlgType= OKONLY;	
		var srcbtn = BTN_INSERT_SUBMIT;
		var error="";
		
		
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
	    
	    ClearControl            = YES;
	    ColumnCaption           = "Plan Type^Name";  
	    frmName                 = templateCodesForm.name; 
	    popupCaption            = "Template Selection Popup";
	    PopulateData            = "CD_ID^CD_DESC";
	    displayCols             = "CD_ID^CD_DESC";
	    CntrlName               = planType.name+ "^"+ planTypeDesc.name;
	    IsHyperLinkReqd         = true; 
	    CallParentFunct         = true;  
	    CallParentFunctWithArg  = 1;
	    QueryIdxVal             = PLAN_TYPE_LIST;
	    FltrValsDatatype        = "";
		var searchCols;
	    var searchColsCaptions;
	    var refreshOnLoad;
	    var searchFltrValsDataType;   
	    
	    searchCols              = "";
		searchColsCaptions      = "";
	    refreshOnLoad           = YES;
	    searchFltrValsDataType  = "";
	    
	    url= contextPath+"/admin/modalselectionPopup.do?method=viewAll&queryIndex=" 
		    + QueryIdxVal + "&fltrValsDataType=" + FltrValsDatatype +  "&columnCaption=" 
		    + ColumnCaption +  "&popData=" + PopulateData + "&formName=" 
		    + frmName + "&cntrlName=" + CntrlName + "&linkReqd=" 
		    + IsHyperLinkReqd + "&formcaption=" + popupCaption + "&callParentfunct=" 
		    + CallParentFunct + "&callPopupArg=" + CallParentFunctWithArg + "&displayCols=" 
		    + displayCols + "&clearControl=" + ClearControl
		    + "&searchCols=" + searchCols + "&searchColsCaptions=" + searchColsCaptions 
		    + "&refreshOnLoad=" + refreshOnLoad 
		    + "&searchFltrValsDataType=" + searchFltrValsDataType;
		    
		var width = 500;
	    var height = 400;
	    var winName = "Template Info";
	    var parentObj = new Array();
	    parentObj[0] = url;
	    parentObj[1] = "Template Selection Popup";
		var urlHTML= contextPath+"/jsp/ModalSelectionPopup.html";
		var returnPromiseValue  = _showModalDialog(urlHTML,parentObj,"status:no;dialogTop:100px;dialogLeft:100px;dialogWidth:"+width+"px;dialogHeight:"+height+"px;help:no;center:no;scrollbars:yes");				
		returnPromiseValue.then(function(returnValue){
			if(returnValue !=null )	
			{	
				if(returnValue[0] != null && returnValue[0].length > 0)
				{
					// After Session Expiry redirect to Home page
					if(trim(returnValue[0])==IS_SESSION_EXPIRED_STR)	
					{	
						self.parent.window.location = contextPath+"/admin/login.do?method=home";
					}
					else
					{
						var cntrlArray = new Array();
						var valueArray = new Array();
					  	cntrlArray = returnValue[0];
				  	  	valueArray = returnValue[1];
				  		for(i=0;i<cntrlArray.length ;i++)
		  	  			{		  	  
							eval("document.templateCodesForm."+cntrlArray[i]).value = valueArray[i];					
						}
						PopupCall(returnValue[2]);	
					}
				}
	  		}
		});
	}
}


function onChangePlanType()
{
	with(document.templateCodesForm)
	{
		if(isDataChanged)
		{
			getDescColArg = PLAN_TYPE_LIST;
		    GetDescription(contextPath,"","CD_DESC","","CD_ID ='" + trim(planType.value) + "' AND CD_TYPE = 'PROD_CATG_CD_TWO'" ,"CODES_DTL",
		                    trim(planType.value),planTypeDesc.name,templateCodesForm.name);
		}
	}	
}


function onClick_TemplateCodes()
{
	
	with(document.templateCodesForm)
	{
		if(tempID.value=="")
		{
	        invokeMessagePopup(TEMPLATE_ID_MANDATORY,OKONLY,BTN_FIND);
	        return;
		}
	 	var dlgType= OKONLY;	
		var srcbtn = BTN_INSERT_SUBMIT;
		var error="";
		
		
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
	    
	    ClearControl            = YES;
	    ColumnCaption           = "Template ID^Template Code^Description";  
	    frmName                 = templateCodesForm.name; 
	    popupCaption            = "Template Codes Selection Popup";
	    PopulateData            = "TEMPLATE_ID^TEMP_CD^TEMP_CD_DESC";
	    displayCols             = "TEMPLATE_ID^TEMP_CD^TEMP_CD_DESC";
	    CntrlName               = tempID.name+ "^"+tempCD.name+"^"+tempCDDesc.name;
	    IsHyperLinkReqd         = true; 
	    CallParentFunct         = true;  
	    CallParentFunctWithArg  = 1;
	    QueryIdxVal             = LETTER_CODES_LIST;
	    FltrValsDatatype        = trim(tempID.value) + "|S";
		var searchCols;
	    var searchColsCaptions;
	    var refreshOnLoad;
	    var searchFltrValsDataType;   
	    
	    searchCols              = "";
		searchColsCaptions      = "";
	    refreshOnLoad           = YES;
	    searchFltrValsDataType  = "";
	    
	    url= contextPath+"/admin/modalselectionPopup.do?method=viewAll&queryIndex=" 
		    + QueryIdxVal + "&fltrValsDataType=" + FltrValsDatatype +  "&columnCaption=" 
		    + ColumnCaption +  "&popData=" + PopulateData + "&formName=" 
		    + frmName + "&cntrlName=" + CntrlName + "&linkReqd=" 
		    + IsHyperLinkReqd + "&formcaption=" + popupCaption + "&callParentfunct=" 
		    + CallParentFunct + "&callPopupArg=" + CallParentFunctWithArg + "&displayCols=" 
		    + displayCols + "&clearControl=" + ClearControl
		    + "&searchCols=" + searchCols + "&searchColsCaptions=" + searchColsCaptions 
		    + "&refreshOnLoad=" + refreshOnLoad 
		    + "&searchFltrValsDataType=" + searchFltrValsDataType;
		    
		var width = 500;
	    var height = 400;
	    var winName = "Template Info";
	    var parentObj = new Array();
	    parentObj[0] = url;
	    parentObj[1] = "Template Selection Popup";
		var urlHTML= contextPath+"/jsp/ModalSelectionPopup.html";
		var returnPromiseValue  = _showModalDialog(urlHTML,parentObj,"status:no;dialogTop:100px;dialogLeft:100px;dialogWidth:"+width+"px;dialogHeight:"+height+"px;help:no;center:no;scrollbars:yes");				
		returnPromiseValue.then(function(returnValue){
			if(returnValue !=null )	
			{	
				if(returnValue[0] != null && returnValue[0].length > 0)
				{
					// After Session Expiry redirect to Home page
					if(trim(returnValue[0])==IS_SESSION_EXPIRED_STR)	
					{	
						self.parent.window.location = contextPath+"/admin/login.do?method=home";
					}
					else
					{
						var cntrlArray = new Array();
						var valueArray = new Array();
					  	cntrlArray = returnValue[0];
				  	  	valueArray = returnValue[1];
				  		for(i=0;i<cntrlArray.length ;i++)
		  	  			{		  	  
							eval("document.templateCodesForm."+cntrlArray[i]).value = valueArray[i];					
						}
						PopupCall(returnValue[2]);	
					}
				}
	  		}
		});
	}
}



function PopupCall(arguments)
{    
	with(document.templateCodesForm)
	{
		if(arguments == TEMPLATE_ID_QRY_INDEX)
		{
			dataChanged();
			if(tempID.value=='AGREEMENT_TEMPL' && customTextCheck.checked)
			{
				Enable(planType);
			}
		}
		if(arguments == CLIENT_ID_QRY_INDEX)
		{
			dataChanged();
			
		}
  	}
}	

function onChange_TempID()
{
	with(document.templateCodesForm)
	{
		if(isDataChanged)
		{
			getDescColArg = TEMPLATE_ID_QRY_INDEX;
		    GetDescription(contextPath,"","CD_DESC","","CD_ID ='" + trim(tempID.value) + "' AND CD_TYPE = 'LETTER_GEN_TEMPLATE_ID'" ,"CODES_DTL",
		                    trim(tempID.value),tempDesc.name,templateCodesForm.name);
		}
	}
}
function DescriptionCall()
{
	var dlgType=OKONLY;		
	var srcbtn = BTN_TABOUT;		
	var error = "";
	with(document.templateCodesForm)
	{
		if(getDescColArg == TEMPLATE_ID_QRY_INDEX)
		{
			if(tempDesc.value == "")
			{
                tempID.value="";
				tempID.focus();
				error=TEMPLATE_ID_INVALID;
			}
			else 
			{
				dataChanged();
				//action=contextPath+"/admin/documentGeneration.do?method=viewAll";
				//submit();
				
				if(tempID.value=='AGREEMENT_TEMPL' && customTextCheck.checked)
				{
					Enable(planType);
				}
			}	
		}
		if(getDescColArg == PLAN_TYPE_LIST)
		{
			if(trim(planType.value) != "" && trim(planTypeDesc.value) == "")
    		{	
    			isDataChanged = false;
	    		error += "31568";	
    		}
		}
		if(error.length > 0)
			invokeMessagePopup(error,dlgType,srcbtn);
	}	
}

function onClick_CustomTextCheck()
{
	with(document.templateCodesForm)
	{
        if(customTextCheck.checked)
        {
        	Enable(customText);
        	
            customText.value=customTextValue;
            if(mode.value == iMode)
            {
            	if(tempID.value=='AGREEMENT_TEMPL')
            	Enable(planType);
            	clientId.value = CLIENT_ID;
            	clientDesc.value = CLIENT_NAME;
            }
        }
        else
        {
            Disable(customText);
            customText.value="";
            Disable(planType);
            if(mode.value == iMode)
            {
            	clientId.value = "";
            	clientDesc.value = "";
            	planType.value = "";
            	planTypeDesc.value = "";
            }
        }
	}
}

function onGridDataClick(rowObj)
{
    with(document.templateCodesForm)
    {
        highlightGridRow(rowObj);
        tempID.value        = trimInnerText(rowObj.cells[10]);
        tempCD.value        = trimInnerText(rowObj.cells[1]);
        tempCDDesc.value    = trimInnerText(rowObj.cells[2]);
        clientId.value 		= trimInnerText(rowObj.cells[4]);
        clientDesc.value    = trimInnerText(rowObj.cells[5]);
        planType.value 		= trimInnerText(rowObj.cells[6]);
        planTypeDesc.value    = trimInnerText(rowObj.cells[7]);
        customText.value    = trimInnerText(rowObj.cells[8]);
        customTextCheck.value = trimInnerText(rowObj.cells[9]);
        tempDesc.value      = trimInnerText(rowObj.cells[0]);
        mode.value          = uMode;
        if(customTextCheck.value==YES)
        {
            customTextCheck.checked=true;
            Enable(customText);
        }
        else
        {
            customTextCheck.checked=false;
            Disable(customText);
        }
        Disable(clientId,clientDesc);
        enableButton(btnDelete);
        Disable(tempID,tempCD);
        tempCDDesc.focus();
        //alert(customTextValue);
    }
}


function checkMaxLength(Control,maxLength)
{
    if(Control.value.length>=maxLength)
        return false;
    else
        return true;
}

function setLength(obj)
{
    if(obj.value.length >3000)
    {
        obj.value=obj.value.substring(0,3000);
    }
}

function  onClick_ClientID()
{   
	with(document.templateCodesForm)
	{
		if(clientId.readOnly)
		{
			return;
		}
	 	var dlgType= OKONLY;	
		var srcbtn = BTN_INSERT_SUBMIT;
		var error="";
		
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
	    
	    ClearControl            = "Y";
	    ColumnCaption           = "Client ID^Description";  
	    frmName                 = templateCodesForm.name; 
	    popupCaption            = "Client Selection Popup";
	    PopulateData            = "CLIENT_ID^CLIENT_NAME";
	    displayCols             = "CLIENT_ID^CLIENT_NAME";
	    CntrlName               = clientId.name+ "^"+ clientDesc.name;
	    IsHyperLinkReqd         = true; 
	    CallParentFunct         = true;  
	    CallParentFunctWithArg  = 1;
	    QueryIdxVal             = CLIENT_ID_QRY_INDEX;
	    FltrValsDatatype        = "";
		var searchCols;
	    var searchColsCaptions;
	    var refreshOnLoad;
	    var searchFltrValsDataType;   
	    
	    searchCols              = "CLIENT_ID^CLIENT_NAME";
		searchColsCaptions      = "Client ID^Name";
	    refreshOnLoad           = "Y";
	    searchFltrValsDataType  = trim(clientId.value) + "|S^" +trim(clientDesc.value)+"|S";
	    
	    url= contextPath+"/admin/modalselectionPopup.do?method=viewAll&queryIndex=" 
		    + QueryIdxVal + "&fltrValsDataType=" + FltrValsDatatype +  "&columnCaption=" 
		    + ColumnCaption +  "&popData=" + PopulateData + "&formName=" 
		    + frmName + "&cntrlName=" + CntrlName + "&linkReqd=" 
		    + IsHyperLinkReqd + "&formcaption=" + popupCaption + "&callParentfunct=" 
		    + CallParentFunct + "&callPopupArg=" + CallParentFunctWithArg + "&displayCols=" 
		    + displayCols + "&clearControl=" + ClearControl
		    + "&searchCols=" + searchCols + "&searchColsCaptions=" + searchColsCaptions 
		    + "&refreshOnLoad=" + refreshOnLoad 
		    + "&searchFltrValsDataType=" + escape(searchFltrValsDataType);
		    
		var width = 500;
	    var height = 400;
	    var winName = "Client Info";
	    var parentObj = new Array();
	    parentObj[0] = url;
	    parentObj[1] = "Client Id Selection Popup";
		var urlHTML= contextPath+"/jsp/ModalSelectionPopup.html";
		var returnPromiseValue  = _showModalDialog(urlHTML,parentObj,"status:no;dialogTop:100px;dialogLeft:100px;dialogWidth:"+width+"px;dialogHeight:"+height+"px;help:no;center:no;scrollbars:yes");				
		returnPromiseValue.then(function(returnValue){
			if(returnValue !=null )	
			{	
				if(returnValue[0] != null && returnValue[0].length > 0)
				{
					// After Session Expiry redirect to Home page
					if(trim(returnValue[0])==IS_SESSION_EXPIRED_STR)	
					{	
						self.parent.window.location = contextPath+"/admin/login.do?method=home";
					}
					else
					{
						var cntrlArray = new Array();
						var valueArray = new Array();
					  	cntrlArray = returnValue[0];
				  	  	valueArray = returnValue[1];
				  		for(i=0;i<cntrlArray.length ;i++)
		  	  			{		  	  
							eval("document.templateCodesForm."+cntrlArray[i]).value = valueArray[i];					
						}
						PopupCall(returnValue[2]);	
					}
				}
	  		}
		});
	}
}
function onChange_ClientID()
{
	with(document.templateCodesForm)
	{
		if(isDataChanged)
		{
			getDescColArg = CLIENT_ID_QRY_INDEX;
		    GetDescription(contextPath,"","CLIENT_NAME","","CLIENT_ID ='" + trim(clientId.value) + "'" ,"CLIENT",
		                    trim(clientId.value),clientDesc.name,templateCodesForm.name);
		}
	}
}
