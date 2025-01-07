
<%@ page import="com.softeon.eso.shared.utils.Constant"%>
<%@ page import="com.softeon.eso.shared.utils.SessionConstants"%>
<%@ page import="com.softeon.eso.shared.utils.SQLQuery"%>
<%@ page import="com.softeon.eso.shared.utils.CodesConstant" %>
<%@ page import="com.softeon.eso.shared.utils.STKGeneral" %>

<%@ taglib uri="/WEB-INF/struts-html.tld" prefix="html"%>
<%@ taglib uri="/WEB-INF/struts-bean.tld" prefix="bean"%>
<%@ taglib uri="/WEB-INF/struts-logic.tld" prefix="logic"%>
<html:html>
<head>
<meta http-equiv="X-UA-Compatible" content="IE=5,chrome=1" />
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>

<script language="JavaScript">
var yes = "<%=Constant.YES %>";
var umode = "<%= Constant.UPDATE_MODE %>";
var imode = "<%= Constant.INSERT_MODE %>";
var Active = "<%= Constant.STR_ACTIVE %>";
var contextPath ="<%=request.getContextPath()%>";
var ON = "<%= Constant.ON %>";
var YES = "<%=Constant.YES %>";
var PARTICIPANT_DETAILS = 	"<%=Constant.PARTICIPANT_DETAILS%>";
var SHARE_HOLDER_INFO =		"<%=Constant.SHARE_HOLDER_INFO %>";
var ADDITIONAL_INFO	= "<%=Constant.ADDITIONAL_INFO %>";
var USER_INFO	= "<%=Constant.USER_INFO %>";

var SHARE_HOLDER_TYPE_INDIVIDUAL = "<%=CodesConstant.SHARE_HOLDER_TYPE_INDIVIDUAL%>";
var SHARE_HOLDER_TYPE_ENTITY = "<%=CodesConstant.SHARE_HOLDER_TYPE_ENTITY%>";

var PARTICIPANT_TYPE = "<%=Constant.PARTICIPANT_TYPE %>";
var NON_OFFICER_CODE = "<%= Constant.NON_OFFICER_CODE %>";
var client	=	"<%=session.getAttribute(SessionConstants.CLIENT_ID)%>";
var APP_ID =	"<%=STKGeneral.nullCheck((String)session.getAttribute(SessionConstants.APP_ID))%>";
var LOCATION_ID_QUERY_INDEX = "<%= SQLQuery.LOCATION_ID_QUERY_INDEX %>";
var DIVISION_ID_QUERY_INDEX = "<%=SQLQuery.DIVISION_ID_QUERY_INDEX %>";
var DEPARTMENT_ID_QUERY_INDEX = "<%=SQLQuery.DEPARTMENT_ID_QUERY_INDEX %>";
var SUBSIDIARY_ID_QUERY_INDEX = "<%=SQLQuery.SUBSIDIARY_ID_QUERY_INDEX %>";
var PARTICIPANT_GROUP_QUERY_INDEX = "<%=SQLQuery.PARTICIPANT_GROUP_QUERY_INDEX %>";
var UNDEFINED = "<%=Constant.UNDEFINED %>";
var UNDEFINED_CONSTANT = "<%=Constant.UNDEFINED_CONSTANT %>";
var GLOBAL_ID_QUERY_INDEX = "<%=SQLQuery.GLOBAL_ID_QUERY_INDEX_PARTICIPANT %>";
var GLOBAL_ID_QUERY_INDEX_INACTIVE = "<%=SQLQuery.GLOBAL_ID_QUERY_INDEX_INACTIVE %>";
var NON_EMPLOYEE_PARTTYPE = "<%=Constant.NON_EMPLOYEE_PARTTYPE %>";
var NOT_APPLICABLE_PARTTYPE = "<%=Constant.NOT_APPLICABLE_PARTTYPE %>";
var NOT_APPLICABLE_EMPTYPE = "<%= Constant.NOT_APPLICABLE_EMPTYPE %>";
var refreshMode = "<%=Constant.REFRESH_MODE %>";
var hideInactive =  "<%=session.getAttribute(SessionConstants.IS_HIDE_INACTIVE_REC)%>";

var NON_OFFICER_CODE = "<%=Constant.NON_OFFICER_CODE%>";
var US_TAX_PAYER = "<%=Constant.US_TAX_PAYER %>";
var TAX_STATUS_US_IMPARTRIATE = "<%=Constant.TAX_STATUS_US_IMPARTRIATE %>";
var TAX_STATUS_US_EXPARTRIATE = "<%=Constant.TAX_STATUS_US_EXPARTRIATE %>";
var TAX_STATUS_INTERNATIONAL_EXPARTRIATE = "<%=Constant.TAX_STATUS_INTERNATIONAL_EXPARTRIATE %>";

var CLIENT = "<%=Constant.CLIENT_PART_GRP %>";
var PARTICIAPNT_ACTIVE = "<%=CodesConstant.PARTICIAPNT_STATUS_ACTIVE %>";
var PARTICIAPNT_INACTIVE = "<%=CodesConstant.PARTICIAPNT_STATUS_INACTIVE %>";

<%--Address Details--%>

var MAIL_ADDRESS_HOME = "<%=Constant.MAIL_ADDRESS_HOME%>";
var MAIL_ADDRESS_WORK = "<%=Constant.MAIL_ADDRESS_WORK%>";
var MAIL_ADDRESS_PAY = "<%=Constant.MAIL_ADDRESS_PAY%>";

<%--Address Details--%>

var DISPALY_GLOBAL_ID = "<%=session.getAttribute(SessionConstants.DISPALY_GLOBAL_ID)%>";
//  Regarding Participant Id and  Global Id 
var PARTICIPANT_ID_QUERY_INDEX_PARTICIPANT_INFO = "<%= SQLQuery.PARTICIPANT_ID_QUERY_INDEX_PARTICIPANT_INFO %>";
var COUNTRY_QRY_INDEX	= "<%=SQLQuery.COUNTRY_ID_QRY_INDEX%>";
var ABA_NO_INDEX 		= "<%=SQLQuery.ABA_NO_INDEX %>"; 
var LIEN_GARNISHMENT_ID_QUERY_INDEX = "<%= SQLQuery.LIEN_GARNISHMENT_ID_QUERY_INDEX %>";
</script>
<bean:define id="participantTitleArray" name="participantDetailsForm" 	property="titleList" type="java.util.ArrayList" scope="request" />
<bean:define id="participantStatusArray" name="participantDetailsForm" 	property="participantStatusList" type="java.util.ArrayList" scope="request" />
<bean:define id="participantSuffixArray" name="participantDetailsForm" 	property="participantSuffixList" type="java.util.ArrayList" scope="request" />
<bean:define id="shareHolderFillValues" name="participantDetailsForm" property="shareHolderList" type="java.util.ArrayList" scope="request" />
<bean:define id="systemAccessStatusArray" name="participantDetailsForm" 	property="systemAccessStatusCodeList" type="java.util.ArrayList" scope="request" />


<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/shared/participant/participantsetup.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/messagedialog.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/msgconstants.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/GeneralUIFunctions.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/stkgenfunctions.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/Calender.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/constants.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/StringValidations.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/AddlDateFunctions.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/GenDateFunctions.js"></script>
<script language="JavaScript" 	src="<%=request.getContextPath()%>/js/common.js"></script>

<BODY id="bodySilver" onClick="parent.hideAllVisibleDivs();">

<jsp:include page="/jsp/standardheader.jsp" flush="true" />
<html:form action="/participantsetup.do" method="post"
	onsubmit="return preventMultipleFormSubmission();">
	<html:hidden property="pageType"  />
	<html:hidden property="mode"  />
	<html:hidden property="focusControl"  />
	<html:hidden property="shareMode"  />
	<html:hidden property="shareAddressId" />
	<html:hidden property="enrollmentAge" />
	<html:hidden property="dateOfAdmisssion" />
	<html:hidden property="canModify" />
	<html:hidden property="canDelete" />
	<html:hidden property="termDateModify" />
	<html:hidden property="termDatePrevValues" />
	<html:hidden property="notifyDatePrevValues" />
	<html:hidden property="termReasonPrevValues" />
	<html:hidden property="deathReasonPrevValue"/>
	<html:hidden property="prevPartGroup" />
	<html:hidden property="grpPrefSeqNo" />
	<html:hidden property="partGrpMapRule" />
	<html:hidden property="sloaExist" />
	<html:hidden property="isSuperUser"  />
	<html:hidden property="reinvestmentPreValue"  />				
	<html:hidden property="dispGlobalId" />
	<html:hidden property="participantId" />
	<html:hidden property="countryName" />
	<html:hidden property="memoRefId" />
	<html:hidden property="lienGarnishmentPartId" />
	<%--Address Details--%>
	
	<html:hidden property="addressId" />
	<html:hidden property="workAddressId"  />
	<html:hidden property="payAddressId" />
	
	<html:hidden property="address" />
	<html:hidden property="workAddress" />
	<html:hidden property="payAddress" />
	<html:hidden property="registrationValidationRule" />
	<%--Address Details--%>
	
<div class="DivGridFieldSet" >
	<FIELDSET class="clsTabSet">
	</FIELDSET>	
</div>

<DIV id="DivTabHeader" class="clsDivTab">
	<table border="0" cellPadding="0" cellSpacing="0">
		<TR>
			<TD height="24px" valign="top">
			<TABLE cellpadding="0" cellspacing="0" border="0">
				<TBODY>
					<TR>
						<TD height="24px"><img name="img1" border="0"
							src="<%=request.getContextPath()%>/images/form_tab_start_on.gif">
						</TD>
					</TR>
				</TBODY>
			</TABLE>
			</TD>

			<TD height="24px" valign="top">
			<TABLE cellpadding="0" cellspacing="0" border="0">
				<TBODY>
					<TR>
						<td valign="middle" height="24px" class="form_tab_on"
							id="<%= Constant.PARTICIPANT_DETAILS %>_IMG"
							background="<%=request.getContextPath()%>/images/form_tab_bg_on.gif">
						<a href="Javascript:onClickParticipantDetailTab();" class="A_Tab" ><font
							id="PSFont"  style="text-decoration: none;"  class="LblFontTabCaption">Participant
						Details</font></a></td>
					</TR>
				</TBODY>
			</TABLE>
			</TD>
			<TD height="24px" valign="top">
			<TABLE cellpadding="0" cellspacing="0" border="0">
				<TBODY>
					<TR>
						<TD height="24px"><img name="img2"
							src="<%=request.getContextPath()%>/images/form_tab_btw_on_off.gif">
						</TD>
					</TR>
				</TBODY>
			</TABLE>
			</TD>

			<TD height="24px" valign="top">
			<TABLE cellpadding="0" cellspacing="0" border="0">
				<TBODY>
					<TR>
						<td height="24px" valign="middle" class="form_tab_on"
							id="<%= Constant.ADDITIONAL_INFO %>_IMG"
							background="<%=request.getContextPath()%>/images/form_tab_bg_off.gif">
						<a href="Javascript:onClickParticipantAdditionalTab();"
							class="A_Tab" ><font id="PAFont"  style="text-decoration: none;" class="LblFontTabCaption">Additional
						Info</font></a></TD>
					</TR>
				</TBODY>
			</TABLE>
			</TD>
			<TD height="24px" valign="top" >
			<TABLE cellpadding="0" cellspacing="0" border="0"><TBODY>
				<TR>
				<TD height="24px">
					<img name="img3"  src="<%=request.getContextPath()%>/images/form_tab_btw_off_off.gif">
				</TD>
				</TR>
			</TBODY>
			</TABLE>
			</TD>		
			
			<TD height="24px" valign="top">
			<TABLE cellpadding="0" cellspacing="0" border="0">
				<TBODY>
					<TR>
						<td height="24px" valign="middle" class="form_tab_on"
							id="<%= Constant.USER_INFO %>_IMG"
							background="<%=request.getContextPath()%>/images/form_tab_bg_off.gif">
						<a href="Javascript:onClickParticipantUserTab();"
							class="A_Tab" ><font id="PTIFont"  style="text-decoration: none;" class="LblFontTabCaption">User
						Info</font></a></TD>
					</TR>
				</TBODY>
			</TABLE>
			</TD>
			<TD height="24px" valign="top">
			<TABLE cellpadding="0" cellspacing="0" border="0">
				<TBODY>
					<TR>
						<TD height="24px"><img name="img4" border="0"
							src="<%=request.getContextPath()%>/images/form_tab_end_on.gif">
						</TD>
					</TR>
				</TBODY>
			</TABLE>
			</TD>
		</TR>

	</table>
</DIV>

<DIV STYLE="position: absolute; width:98%; float: center; z-index: 2; top:45px; left: 20px">
	<table align="center" border=0 cellspacing=2 cellpadding=2 width="100%">
		<tr>
			<td width="250px">
				<table align="center" border=0 cellspacing=2 cellpadding=2 >
					<tr>
						<td>
							<a href="javascript:GlobalId_click();"> <FONT class="LblFontMandUnderLine"> 
							<bean:message key="participant.label.GlobalId" /> </FONT></a>
						</td>
						<td valign=middle><FONT class=LblFontMand>Share Holder Type</FONT></td>
					</tr>
					<tr>
						<td ><html:text property="globalId" size="15" maxlength="12" style="font-variant:small-caps" onchange="javascript:dataChanged()" onblur="javascript:find_OnClick(), changeToUpperCase(this);"  tabindex="1" /></td>
						<td ><html:select name="participantDetailsForm" property="shareHolderType" style="height:22px; width:120px" onchange="javascript:dataChanged();shareType();changeLienGranishment();" tabindex="2">
								<logic:present name="shareHolderFillValues">
									<html:options collection="shareHolderFillValues" property="codeId" labelProperty="codeDesc" />
								</logic:present>
							</html:select>
						</td>
					</tr>
				</table>
			</td>
			
			<td>
				<table id="IndividualId" width="100%" align="center" border=0 cellspacing=0 cellpadding=0 style="display: none;">
					<TR>
						<td style="width: 250px;">
							<TABLE cellpadding="2" cellspacing="2" width="100%">
								<tr>
									<td ><FONT class=LblFontMand><bean:message key="participant.label.LastName" /> </FONT></td>
									<td ><FONT class="LblFontMand"><bean:message 	key="participant.label.FirstName" /> </FONT></td>
									<td ><FONT class="LblFont"><bean:message 	key="participant.label.MiddleName" /> </FONT></td>			
								</tr>
								<tr>
									<td ><html:text property="lastName" size="15" 	maxlength="56" onchange="javascript:dataChanged();" tabindex="3"/></td>
									<td ><html:text property="firstName" size="15" 	maxlength="30" onchange="javascript:dataChanged();" tabindex="4"/></td>
									<td ><html:text property="middleName" size="15" maxlength="20" onchange="javascript:dataChanged();" tabindex="5"/></td>
								</tr>
							</TABLE>
						</td>
					</TR>
				</table>
				<table id="EntityId" width="100%" align="center" border=0 cellspacing=0 cellpadding=0 style="display:  none;">
					<TR>
						<td style="width: 250px;">
							<TABLE cellpadding="2" cellspacing="2" width="100%">
								<tr>
									<td title="Click here to enter Long Name"><a href="#" tabindex="3" onClick="javascript:memoPopup(participantDetailsForm.longName,144,400,240);"><FONT class=LblFontMandUnderLine>Long Name</FONT></a></td>
								</tr>
								<tr>
									<td ><html:textarea property="longName" rows="1" cols="50" onchange="javascript:dataChanged();"/></td>
								</tr>
							</TABLE>
						</TD>
					</TR>
				</table>
			</td>
			<script>
				shareType();
			</SCRIPT>
			<td width="200px">
				<table align="center" border=0 cellspacing=2 cellpadding=2 >
					<tr>
						<td ><FONT class="LblFont"> <bean:message 	key="participant.label.TitleName" /> </FONT></td>
						<td ><FONT class="LblFont"> Suffix </FONT></td>
						<td ><FONT class="LblFontMand"> <bean:message key="participant.label.ParticipantStatus" /> </FONT></td>
						<td ><FONT class="LblFontMand"> <bean:message key="participant.label.gender" /> </FONT></td>			
					</tr>
					<tr>
						<td >
							<html:select name="participantDetailsForm" 	property="title" style="height:22px; width:65px" onchange="javascript:dataChanged();" tabindex="6">
								<html:option value=""></html:option>
								<logic:present name="participantTitleArray">
									<html:options collection="participantTitleArray" property="codeId" labelProperty="codeDesc" />
								</logic:present>
							</html:select>
						</td>
						
						<td ><html:select name="participantDetailsForm" property="participantSuffix" style="height:22px; width:65px"
							onchange="javascript:dataChanged();" tabindex="7">
							<html:option value=""></html:option>
							<logic:present name="participantSuffixArray">
								<html:options collection="participantSuffixArray" property="codeId"
									labelProperty="codeDesc" />
							</logic:present>
						</html:select></td>
						
						<td ><html:select name="participantDetailsForm" property="participantStatus" style="height:22px; width:90px"
							onchange="javascript:dataChanged();" tabindex="8">
							<html:option value=""></html:option>
							<logic:present name="participantStatusArray">
								<html:options collection="participantStatusArray" property="codeId"
									labelProperty="codeDesc" />
							</logic:present>
						</html:select></td>
						
						<td ><html:select name="participantDetailsForm" property="gender" style="height:22px; width:70px"
							onchange="javascript:dataChanged();" tabindex="9">
								<html:option value=""></html:option>
								<html:option value="Male"></html:option>
								<html:option value="Female"></html:option>
							</html:select>
						</td>
					</tr>
				</table>
			</td>
		</tr>
		<tr>
		<table align="center" border=0 cellspacing=2 cellpadding=2 >
		<tr>
		<td valign=bottom><FONT class="LblFontMand"> System Status </FONT></td>	
		<td valign=top><html:select name="participantDetailsForm" property="systemAccessStatusCd" style="height:22px; width:100px"
				onchange="javascript:dataChanged();" tabindex="5">
				<logic:present name="systemAccessStatusArray">
					<html:options collection="systemAccessStatusArray" property="codeId"
						labelProperty="codeDesc" />
				</logic:present>
			</html:select></td>
			</tr>
			</table>
		</tr>
	</table>
</DIV>


<DIV STYLE="position: absolute; width:98%; float: center; z-index: 2; top:480px; left: 20px">
	<TABLE width=100% align="center" cellpadding="0" cellspacing="0">
		<TR>
			<TD>
				<HR>
			</TD>
		</TR>
		<TR>
			<TD>
				<TABLE align="center">
					<tr>
						<td><input name="btnSubmit" type="button" class="action"
							onmouseover="javascript:buttonOnMouseOver(this);"
							onmouseout="javascript:buttonOnMouseOut(this);" value="Submit"
							onclick="javascript:submit_OnClick();" title="Submit Participant" tabindex="53">
						</td>
						<td><input name="btnDelete" type="button" class="action"
							onmouseover="javascript:buttonOnMouseOver(this);"
							onmouseout="javascript:buttonOnMouseOut(this);" value="Delete"
							onclick="javascript:onClick_Delete();" title="Delete Participant" tabindex="54">
						</td>
						<td><input name="btnReset" type="button" class="action"
							onmouseover="javascript:buttonOnMouseOver(this);"
							onmouseout="javascript:buttonOnMouseOut(this);" value="Reset"
							onclick="javascript:reset_Click();" title="Reset Participant" tabindex="55"></td>
			
						<td><input name="btnRefresh" type="button" class="action"
							onmouseover="javascript:buttonOnMouseOver(this);"
							onmouseout="javascript:buttonOnMouseOut(this);" value="Refresh"
							onclick="javascript:refresh_Click();" title="Refresh Participant" tabindex="56"/>
						</td>
					</tr>
				</TABLE>
			</TD>
		</TR>
		<TR>
			<TD>
				<HR>
			</TD>
		</TR>
	</TABLE>
</DIV>

<DIV id="<%=Constant.PARTICIPANT_DETAILS%>" style="position:absolute; top:127px; z-index:2; width :98%; left:20px;  display:none;">
<jsp:include page="/jsp/shared/participant/participantdetails.jsp" />
</DIV>

<DIV id="<%=Constant.ADDITIONAL_INFO %>" style="position:absolute; top:150px; z-index:2; width :98%; left:20px;  display:none;">
<jsp:include page="/jsp/shared/participant/participantadditional.jsp" />
</DIV>

<DIV id="<%=Constant.USER_INFO%>" style="position:absolute; top:150px; z-index:2; width :98%; left:20px;  display:none;">
	<jsp:include page="/jsp/shared/participant/participantuserinfo.jsp" />
</DIV>

<DIV id="<%=Constant.SHARE_HOLDER_INFO%>" style="position:absolute; top:115px; z-index:2; width :98%; left:20px;  display:none;">
<jsp:include page="/jsp/shared/participant/shareholderinfo.jsp" />
</DIV>
	
<script language="JavaScript">

if(participantDetailsForm.pageType.value == "<%=Constant.PARTICIPANT_DETAILS%>") 
	ShowHideDiv(ALL_TABS,gcsShowTab1,1);	
else if(participantDetailsForm.pageType.value == "<%=Constant.ADDITIONAL_INFO %>") 
	ShowHideDiv(ALL_TABS,gcsShowTab2,2);		
else if(participantDetailsForm.pageType.value == "<%=Constant.USER_INFO %>") 
	ShowHideDiv(ALL_TABS,gcsShowTab3,3);	

function MessageBoxCall(flag)
{
	if(flag == "MSG_YES")
	{
		participantDetailsForm.action=contextPath + "/admin/participantsetup.do?method=logicalDeletePartsetup";	
		participantDetailsForm.submit();
	}
	else if(flag == "MSG_NO")
		participantDetailsForm.mode.value=umode;
	
}
</script>
<jsp:include page="/jsp/formerrors.jsp"  flush="true"/>
<script language="Javascript">
 		onFormLoad();
 		shareType();
 		changeParicipantType();
 		changeOfficerCode();
 		
 </script>
</html:form>
</body>
</html:html>
