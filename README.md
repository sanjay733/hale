package com.softeon.eso.shared.utils;

import java.sql.SQLException;

import javax.servlet.http.HttpServletRequest;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import com.ast.eps.businessline.esoshared.BusinessLineImplementation;

/*
 * This is the global exception handler
 * If you have any code that results in a target =
 * call this from your final catch(Exception e)
 * If you have global exception handling that results in a custom action add it here
 */
public abstract class GlobalExceptionHandler {

    protected static final Logger logger = LogManager.getLogger(GlobalExceptionHandler.class);

    protected static void logUnhandled(HttpServletRequest request, Exception e) {
        logger.error(request.getPathTranslated() + " : " + e + " : " + e.getMessage(), e);
    }

    protected static void log(HttpServletRequest request, SQLException e) {
        logger.error("SQL Exception from : " + request.getPathTranslated() + " : " + e.getMessage() + ": code = " + e.getErrorCode(), e);
    }

    protected static void log(HttpServletRequest request, Exception e) {
        logger.error(request.getContextPath() + " : " + e.getMessage(), e);
    }

    public static String exceptionHandler(HttpServletRequest request, Exception e, String defaultTarget) {
    	return BusinessLineImplementation.getInstance().exceptionHandler(request, e, defaultTarget);
    }

    public static GlobalExceptionHandler getInstance() {
        return BusinessLineImplementation.getInstance().getGlobalExceptionHandler();
    }
}
