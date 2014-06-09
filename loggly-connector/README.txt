Product: Integration tests for WSO2 ESB Loggly connector

Pre-requisites:

 - Maven 3.x
 - Java 1.6 or above

Tested Platform:

 - Microsoft WINDOWS V-7
 - UBUNTU 13.04
 - WSO2 ESB 4.8.1

STEPS:

 1. Make sure the ESB 4.8.1 zip file with latest patches are available at "Integration_Test/products/esb/4.8.1/modules/integration/connectors/repository"

 2. The ESB should be configured as below;
	Please make sure that the below mentioned Axis configurations are enabled (/repository/conf/axis2/axis2.xml).
   
    <messageFormatter contentType="text/html" class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>
    <messageBuilder contentType="text/html"	class="org.wso2.carbon.relay.BinaryRelayBuilder"/>

	Note: Add the aforementioned message formatter and the message builder to the axis file, if they are not available by default.
	
 3. Make sure "integration-base" project is placed at "Integration_Test/products/esb/4.8.1/modules/integration/"

 4. Navigate to "Integration_Test/products/esb/4.8.1/modules/integration/integration-base" and run the following command.
      $ mvn clean install
  
 5. Make sure the "loggly" test suite is enabled (as given below) and all other test suites are either commented or removed in the following file - "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/testng.xml"
    <test name="loggly-Connector-Test" preserve-order="true" verbose="2">
        <packages>
            <package name="org.wso2.carbon.connector.integration.test.loggly"/>
        </packages>
    </test>
	
 6. Copy the main and test folders from the provided "loggly" connector source bundle to the location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/". 
	Copy pom.xml from the source bundle to "Integration_Test/products/esb/4.8.1/modules/integration/connectors/" and uncomment fhe following two blocks:

	<parent>
	<groupId>org.wso2.esb</groupId>
	<artifactId>esb-integration-tests</artifactId>
	<version>4.8.1</version>
	<relativePath>../pom.xml</relativePath>
	</parent>

	<dependency>
	<groupId>org.wso2.esb</groupId>
	<artifactId>org.wso2.connector.integration.test.base</artifactId>
	<version>4.8.1</version>
	<scope>system</scope>
	<systemPath>${basedir}/../integration-base/target/org.wso2.connector.integration.test.base-4.8.1.jar</systemPath>
	</dependency>

 7. Follow the below steps to create a Loggly account.

	i) Navigate to "https://www.loggly.com/" and click "Try Loggly for FREE" link.
   ii) Enter the required details and complete the account creation. 
	
 8. Required properties for Loggly Connector Integration Testing
   
	i) logglyAccountApiUrl 		- The URL of the account subdomain. This parameter takes the format of "http://<account>.loggly.com". Substitute your account name for "<account>".
	ii) logglyApiUrl            - The API URL. Current API URL is "http://logs-01.loggly.com". 
	iii) token 				    - The customer token. Follow the steps in 9) to derive the customer token. 
	iv) txtFileName 		    - Name of the file which is used in the uploadLogFile method. File is located at /test/resources/artifacts/ESB/config/resources/loggly.
	v) username 			    - Username of the loggly account. This will be used to generate the Basic HTTP Access Authentication Token mentioned in Step 9).
	vi) password            	- Password of the loggly account. This will be used to generate the Basic HTTP Access Authentication Token mentioned in Step 9).
	vii) query 			        - A random string which will be used to query the logs. Eg: "abc".
	viii) firstLine 	        - A random string value which will be logged Eg: "aaa".
	ix) secondLine 				- A random string value which will be logged Eg: "bbb".	
	
 9. This API uses two types of authentication schemes. Those two types are authentication via Customer Token and Basic HTTP Accesss Authentication.  

	i) Deriving the customer token:
		
		- Navigate to loggly account home page.
		- Go to "Source Setup" > "Customer Tokens" and get the customer token which is required to execute the uploadLogFile and sendBulkLogs methods.   
	
   ii) Basic HTTP Access Authentication:
   
		- The below steps will be done through the code. It is sufficient to provide the username and the password in the properties file. 
		- Concat the username and password according to the below format.
			username:password
		- Encode (Base64) the entire string and get the token.
		
 10. Navigate to "Integration_Test/products/esb/4.8.1/modules/integration/connectors/" and run the following command.
      $ mvn clean install