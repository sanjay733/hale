<action input="/participantsetup.do?method=viewAllPartsetup" name="participantDetailsForm" parameter="method" path="/participantsetup" scope="request" type="com.softeon.eso.adminshared.participant.actions.ASTParticipantDetailsAction" validate="true">
			<forward contextRelative="true" name="participantSuccess" path="/jsp/shared/participant/participantsetupmain.jsp">
			</forward>
			<forward name="recordExistException" path="/participantsetup.do?method=viewAllPartsetup"/>
			<forward name="shareRecordExistException" path="/participantsetup.do?method=viewAllShareHolder"/>
			<forward name="daoException" path="/participantsetup.do?method=viewAllPartsetup"/>
			<forward name="participantSetupFailure" path="/participantsetup.do?method=viewAllPartsetup"/>
		</action>

   public ActionForward createPartsetup(ActionMapping mapping, ActionForm form, HttpServletRequest request, HttpServletResponse response) {
        String target = "participantSuccess";
        ParticipantDetailsForm participantDetailsForm = null;
        ParticipantDetailsValueObject participantDetailsVO = null;
        ASTParticipantSetupDAO participantSetupDAO = null;
        String messageKey = null;
        HttpSession session = null;
        String clientId = null;
        Hashtable ruleValues = null;
        try {
            logger.debug(" Inside the Participant Setup Create method ");
            isSessionExpired(request, response);
            session = request.getSession(false);
            clientId = (String) session.getAttribute(SessionConstants.CLIENT_ID);
            participantDetailsForm = (ParticipantDetailsForm) form;
            participantDetailsVO = participantDetailsForm.createParticipantValueObject();
            participantDetailsVO.setClientId(clientId);
            participantDetailsVO.setModifyUser(session.getAttribute(SessionConstants.USER_ID).toString());
            participantDetailsForm.setDispGlobalId((String) session.getAttribute(SessionConstants.DISPALY_GLOBAL_ID));
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
            if (participantDetailsVO.getParticipantStatus() != null && participantDetailsVO.getParticipantStatus().trim().length() > 0) {
                // List box Validation for Participant Status Code
                messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.PART_STATUS_CD, participantDetailsVO.getParticipantStatus(), MsgConstant.INVALID_PARTICIPANT_STATUS, MsgConstant.INACTIVE_PARTICIPANT_STATUS);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            if (participantDetailsVO.getMailAddress() != null && participantDetailsVO.getMailAddress().trim().length() > 0) {
                // List box Validation for Mail Address
                messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), "ADDRESS_CD", participantDetailsVO.getMailAddress(), MsgConstant.INVALID_MAIL_ADDRESS, MsgConstant.INACTIVE_MAIL_ADDRESS);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            if (participantDetailsVO.getTitle() != null && participantDetailsVO.getTitle().trim().length() > 0) {
                // List box Validation for Title Code
                messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.CD_PERSON_TITLE, participantDetailsVO.getTitle(), MsgConstant.INVALID_TITLE_CODE, MsgConstant.INACTIVE_TITLE_CODE);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            if (participantDetailsVO.getParticipantSuffix() != null && participantDetailsVO.getParticipantSuffix().trim().length() > 0) {
                // List box Validation for Suffix
                messageKey = checkCodeIdExist(request, CodesConstant.PART_SUFFIX_CD, participantDetailsVO.getParticipantSuffix(), MsgConstant.INVALID_SUFFIX, MsgConstant.INACTIVE_SUFFIX);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            if (participantDetailsVO.getJobTitle() != null && !participantDetailsVO.getJobTitle().equals("")) {
                // List box Validation for Job Title Code from Client_codes_Dtl
                messageKey = checkCodeIdExist(request, clientId, CodesConstant.PART_JOB_TITLE, participantDetailsVO.getJobTitle(), MsgConstant.INVALID_JOB_TITLE_CODE, MsgConstant.INACTIVE_JOB_TITLE_CODE);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            if (participantDetailsVO.getIsoCode() != null && !participantDetailsVO.getIsoCode().equals("")) {
                // List box Validation for ISO Code
                messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.ISO_CD, participantDetailsVO.getIsoCode(), MsgConstant.INVALID_ISO_CODE, MsgConstant.INACTIVE_ISO_CODE);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            // List box Validation for Participant Type
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.PARTICIPANT_TYPE, participantDetailsVO.getParticipantType(), MsgConstant.INVALID_PARTICIPANT_TYPE, MsgConstant.INACTIVE_PARTICIPANT_TYPE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Employement Type
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.EMP_TYPE, participantDetailsVO.getEmploymentType(), MsgConstant.INVALID_EMPLOYMENT_TYPE_CODE, MsgConstant.INACTIVE_EMPLOYMENT_TYPE_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Payroll Code
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.PAY_ROLL_CODE, participantDetailsVO.getPayrollCycle(), MsgConstant.INVALID_PAYROLL_CODE, MsgConstant.INACTIVE_PAYROLL_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Officer
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.CNTL_FLG_DEF, participantDetailsVO.getOfficer(), MsgConstant.INVALID_OFFICER, MsgConstant.INACTIVE_OFFICER);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Officer Code
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.OFFICER_CD, participantDetailsVO.getOfficerCode(), MsgConstant.INVALID_OFFICER_CODE, MsgConstant.INACTIVE_OFFICER_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Form114 Code
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.CNTL_FLG_DEF, participantDetailsVO.getForm114(), MsgConstant.INVALID_FORM114_CODE, MsgConstant.INACTIVE_FORM114_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Section16 Code
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.CNTL_FLG_DEF, participantDetailsVO.getSection16(), MsgConstant.INVALID_SECTION16_CODE, MsgConstant.INACTIVE_SECTION16_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Reinvestment Code
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.DIV_ELEC_CD, participantDetailsVO.getReinvestmentCode(), MsgConstant.INVALID_DIV_ELECT_CODE, MsgConstant.INACTIVE_DIV_ELECT_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            // List box Validation for Tax Code
            messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.TAX_CODE, participantDetailsVO.getTaxCode(), MsgConstant.INVALID_TAX_CODE, MsgConstant.INACTIVE_TAX_CODE);
            if (messageKey != null && messageKey.trim().length() > 0) {
                throw new RecordUnAvailableException(messageKey);
            }
            if (participantDetailsVO.getTerminationDate() != null && participantDetailsVO.getTerminationDate().trim().length() > 0) {
                // List box Validation for Termination Reason
                messageKey = checkCodeIdExist(request, participantDetailsVO.getClientId(), CodesConstant.TERM_RSN_CD, participantDetailsVO.getTerminationReason(), MsgConstant.INVALID_TERMINATION_REASON, MsgConstant.INACTIVE_TERMINATION_REASON);
                if (messageKey != null && messageKey.trim().length() > 0) {
                    throw new RecordUnAvailableException(messageKey);
                }
            }
            participantDetailsVO.setAppId((String) session.getAttribute(SessionConstants.APP_ID));
            /*Code added to handle cash disbursement rule */
            if (participantDetailsForm.getReinvestmentCode().trim().equals(CodesConstant.DIV_CD_CASH_PAYOUT)) {
                if (STKGeneral.nullCheck((String) ruleValues.get(CASH_PAYOUT_DISBURSEMENT_METHOD)).trim().equals(Constant.DISBURSEMENT_TO_CLIENT))
                    participantDetailsVO.setReinvestmentCode(Constant.DISBURSEMENT_TO_CLIENT_VALUE);
                else
                    participantDetailsVO.setReinvestmentCode(Constant.DISBURSEMENT_TO_PARTICIPANT_VALUE);
            }
            participantSetupDAO = new ASTParticipantSetupDAO();
            int returnValue = participantSetupDAO.insertToDB(participantDetailsVO, participantDetailsForm.getDispGlobalId());
            if (returnValue <= 0) {
                loadParticipantComboValues(request, participantDetailsForm, clientId);
                super.raiseException('I', returnValue);
            }
            logger.debug(" The Active Flag   Status " + participantDetailsForm.getActive());
            this.populateParticipantFormInRequest(participantDetailsForm, participantDetailsVO);
            logger.debug(" The Active Flag   Status " + participantDetailsForm.getActive());
            participantDetailsForm.setMode(Constant.UPDATE_MODE);
            participantDetailsForm.setShareMode(Constant.INSERT_MODE);
            if (participantDetailsForm.getParticipantStatus() != null && participantDetailsForm.getParticipantStatus().equals(CodesConstant.PARTICIAPNT_STATUS_ACTIVE) && participantDetailsForm.getMode().equals(Constant.UPDATE_MODE))
                loadParticipantComboValuesUpdate(request, clientId, participantDetailsForm, participantSetupDAO);
            else
                loadParticipantComboValues(request, participantDetailsForm, clientId);
            participantDetailsForm.setPageType(Constant.PARTICIPANT_DETAILS);
            participantDetailsForm.setIsSuperUser((String) session.getAttribute(SessionConstants.SUPER_USER));
        } catch (SessionExpiredException se) {
            target = "sessionExpired";
            super.handleUserException(request, se, "Error while executing Participant Setup create method : ");
        } catch (RuleSetupException rsE) {
            target = "participantSetupFailure";
            logger.error("RuleSetupException is Occured:" + rsE);
            super.handleRuleSetupException(request, rsE);
        } catch (RecordUnAvailableException re) {
            target = "recordExistException";
            super.handleUserException(request, re, "Error while executing Participant Setup create method : ");
        } catch (UniqueIndexKeyViolationException sqlE) {
            target = "recordExistException";
            super.handleUserException(request, sqlE, "Error while executing Participant Setup create method : ");
        } catch (DAOException daoE) {
            target = "daoException";
            super.handleUserException(request, daoE, "Error while executing  Participant Setup create method : ");
        } catch (SQLException sqlE) {
            target = "participantSetupFailure";
            target = super.handleSQLException(request, sqlE, target, "Error while executing Participant Setup create method : ");
        } catch (Exception e) {
            target = super.handleUnhandeldException(request, e);
        } finally {
            participantDetailsVO = null;
            participantSetupDAO = null;
            messageKey = null;
            session = null;
            clientId = null;
            ruleValues = null;
        }
        return mapping.findForward(target);
    }
