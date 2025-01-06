<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<%--
 * $Workfile:   error.jsp $
 * $Revision:   1.11  $ 
 * $Date:   21 Apr 2005 11:38:24  $
 * $Modtime:   28 Apr 2005 12:05:06  $
 * $Author:   E1071  $
 *
 * Copyright 2006 Softeon Inc.All rights reserved.
 * ============================================================================
--%>
<%@ taglib uri="/WEB-INF/struts-html.tld" prefix="html" %>
<%@ taglib uri="/WEB-INF/struts-bean.tld" prefix="bean" %>
<%@ taglib uri="/WEB-INF/struts-logic.tld" prefix="logic" %>
<html:html>
<head>
<meta http-equiv="X-UA-Compatible" content="IE=5,chrome=1" />
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>

<HEAD>
<META name="GENERATOR" content="IBM WebSphere Studio">
<LINK href="<%=request.getContextPath()%>/theme/Master.css"
	rel="stylesheet" type="text/css">
<TITLE><bean:message key="title.common.error" /></TITLE>
</HEAD>
<BODY id="bodySilver">
<jsp:include page="/jsp/standardheader.jsp" flush="true" />
<logic:present name="esoErrors" scope="request">
	<bean:define id="esoErrors" name="esoErrors" scope="request"
		type="java.util.Iterator" />
	<table border="0" cellpadding="2" cellspacing="1" align="center">
		<logic:iterate id="esoError" name="esoErrors">
			<tr>
				<td><img src="<%=request.getContextPath()%>/images/btncritical.gif" />
				</td>
			</tr>
			<tr id="errorHeading" align="center">
				<td><bean:write name="esoError" property="msgDesc" /></td>
			</tr>
		</logic:iterate>
	</table>
</logic:present>
</BODY>
</html:html>
