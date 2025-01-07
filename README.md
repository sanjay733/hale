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
	<%
    String errorMessage = (String) request.getAttribute("errorMessage");
    %>
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
<%-- If errorMessage is not null, display it --%>
<% if (errorMessage != null) { %>
    <div class="error-message" style="color: red; font-weight: bold; padding: 10px; border: 1px solid red; background-color: #f8d7da;">
        <%= errorMessage %>
    </div>
<% } %>
<DIV id="DivTabHeader" class="clsDivTab">
	<table border="0" cellPadding="0" cellSpacing="0">
		<TR>
			<TD height="24px" vali
