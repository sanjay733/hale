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
