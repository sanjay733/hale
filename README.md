	<!-- Global Forwards -->
	<global-forwards>
		<forward contextRelative="true" name="databaseBusy" path="/jsp/databasebusy.jsp"/>
		<forward contextRelative="true" name="fatalException" path="/jsp/error.jsp"/>
		<forward name="sessionExpired" path="/global.do?method=oamLogout"/>
		<forward name="popupSessionExpired" path="/global.do?method=oamLogout"/>
	</global-forwards>
