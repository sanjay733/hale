<forward name="participantSetupFailure" path="/jsp/shared/participant/participantsetupmain.jsp"/>


} catch (RuleSetupException rsE) {
    target = "participantSetupFailure";
    logger.error("RuleSetupException occurred: " + rsE.getMessage());
    request.setAttribute("errorMessage", "An error occurred during participant setup: " + rsE.getMessage());
    super.handleRuleSetupException(request, rsE);
} catch (SQLException sqlE) {
    target = "participantSetupFailure";
    logger.error("SQLException occurred: " + sqlE.getMessage());
    request.setAttribute("errorMessage", "A database error occurred: " + sqlE.getMessage());
    target = super.handleSQLException(request, sqlE, target, "Error in Participant Setup.");
}


<c:if test="${not empty errorMessage}">
    <div style="color: red; margin-top: 10px;">
        <strong>Error:</strong> ${errorMessage}
    </div>
</c:if>


participantDetailsForm.setSomeField(userEnteredValue);
