public ActionForward viewAllPartsetup(ActionMapping mapping, ActionForm form, HttpServletRequest request, HttpServletResponse response) {
        String target = "participantSuccess";
        ParticipantDetailsForm participantDetailsForm = null;
        Hashtable ruleValues = null;
        HttpSession session = null;
        String clientId = null;
        try {
            logger.debug(" Inside the Participant Setup ViewAll method ");
            isSessionExpired(request, response);
            session = request.getSession(false);
            clientId = (String) session.getAttribute(SessionConstants.CLIENT_ID);
            logger.debug("The Selected Client Id  " + clientId);
            participantDetailsForm = (ParticipantDetailsForm) form;
            loadParticipantComboValues(request, participantDetailsForm, clientId);
            ruleValues = ruleValidation((String) session.getAttribute(SessionConstants.APP_ID), clientId, null);
            if (ruleValues != null && ruleValues.get(Constant.MIN_AGE_ENROLLMENT) != null) {
                participantDetailsForm.setEnrollmentAge(STKGeneral.nullCheck((String) ruleValues.get(Constant.MIN_AGE_ENROLLMENT)));
            } else
                participantDetailsForm.setEnrollmentAge(String.valueOf(0));
            if (ruleValues != null && ruleValues.get(Constant.ENROLLMENT_WAITING_PERIOD) != null) {
                participantDetailsForm.setDateOfAdmisssion(STKGeneral.nullCheck((String) ruleValues.get(Constant.ENROLLMENT_WAITING_PERIOD)));
            } else
                participantDetailsForm.setDateOfAdmisssion(String.valueOf(0));
            if (ruleValues != null && ruleValues.get(Constant.PART_GRP_MAP_RULE_ID) != null) {
                participantDetailsForm.setPartGrpMapRule(STKGeneral.nullCheck((String) ruleValues.get(Constant.PART_GRP_MAP_RULE_ID)));
            }
            String REGISTRATION_VALIDATION="8107";
            if (ruleValues != null && ruleValues.get(REGISTRATION_VALIDATION) != null) {
            	if("HDATE".equalsIgnoreCase(STKGeneral.nullCheck((String) ruleValues.get(REGISTRATION_VALIDATION)))) {
            		participantDetailsForm.setRegistrationValidationRule(STKGeneral.nullCheck((String) ruleValues.get(REGISTRATION_VALIDATION)));	
            	}
            }
            if (participantDetailsForm.getMode() != null && participantDetailsForm.getMode().equals(Constant.UPDATE_MODE))
                participantDetailsForm.setMode(Constant.UPDATE_MODE);
            else
                participantDetailsForm.setMode(Constant.INSERT_MODE);
            participantDetailsForm.setShareMode(Constant.INSERT_MODE);
            if (participantDetailsForm.getPageType() == null || participantDetailsForm.getPageType().trim().equals(""))
                participantDetailsForm.setPageType(Constant.PARTICIPANT_DETAILS);
            participantDetailsForm.setIsSuperUser((String) session.getAttribute(SessionConstants.SUPER_USER));
            participantDetailsForm.setDispGlobalId((String) session.getAttribute(SessionConstants.DISPALY_GLOBAL_ID));
        } catch (SessionExpiredException se) {
            target = "sessionExpired";
            super.handleUserException(request, se, "Error while executing Participant Setup ViewAll method : ");
        } catch (RuleSetupException rsE) {
            target = "participantSetupFailure";
            logger.error("RuleSetupException is Occured:" + rsE);
            super.handleRuleSetupException(request, rsE);
        } catch (SQLException sqlE) {
            target = "participantSetupFailure";
            target = super.handleSQLException(request, sqlE, target, "Error while executing Participant Setup View All method : ");
        } catch (Exception e) {
            target = super.handleUnhandeldException(request, e);
        } finally {
            session = null;
            clientId = null;
        }
        logger.debug("Target :" + target);
        return mapping.findForward(target);
    }
