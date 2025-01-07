<%
    String errorMessage = (String) request.getAttribute("errorMessage");
%>

<%-- If errorMessage is not null, display it --%>
<% if (errorMessage != null) { %>
    <div class="error-message" style="color: red; font-weight: bold; padding: 10px; border: 1px solid red; background-color: #f8d7da;">
        <%= errorMessage %>
    </div>
<% } %>

request.setAttribute("errorMessage", "There was an error processing the participant setup. Please try again.");

catch (SQLException sqlE) {
    target = "participantSetupFailure";
    request.setAttribute("errorMessage", "Database error occurred. Please contact support.");
    logger.error("SQLException occurred: " + sqlE);
}
catch (Exception e) {
    target = "participantSetupFailure";
    request.setAttribute("errorMessage", "An unexpected error occurred. Please try again.");
    logger.error("Unhandled exception occurred: " + e);
}
