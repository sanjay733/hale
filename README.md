	<action input="/modalselectionPopup.do?method=viewAll" name="modalselectionPopupForm" parameter="method" path="/modalselectionPopup" scope="request" type="com.softeon.eso.shared.actions.ModalSelectionPopupAction" validate="false">
			<forward contextRelative="true" name="loadModalPopupSuccess" path="/jsp/modalselectionpopup.jsp"/>
			<forward contextRelative="true" name="loadModalPopupFailure" path="/jsp/modalselectionpopup.jsp"/>
		</action>
