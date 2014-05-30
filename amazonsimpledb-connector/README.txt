Product: Integration tests for WSO2 ESB Amazon Simple DB connector

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
	Please make sure that the below mentioned Axis configurations are enabled (\repository\conf\axis2\axis2.xml).
   
   <messageFormatter contentType="application/octet-stream" class="org.apache.axis2.format.BinaryFormatter"/>

   
 3. Make sure "integration-base" project is placed at "Integration_Test/products/esb/4.8.1/modules/integration/"

 
 4. Navigate to "Integration_Test/products/esb/4.8.1/modules/integration/integration-base" and run the following command.
      $ mvn clean install

	  
 5. Make sure the "amazonsimpledb" test suite is enabled (as given below) and all other test suites are commented in the following file - "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/testng.xml"
	<test name="AmazonSimpleDB-Connector-Test" preserve-order="true" verbose="2">
        <packages>
            <package name="org.wso2.carbon.connector.integration.test.amazonsimpledb"/>
        </packages>
    </test>
	
	
 6. Copy the main and test folders from the provided "amazonsimpledb" connector source bundle to the location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/". Copy pom.xml from the source bundle to "Integration_Test/products/esb/4.8.1/modules/integration/connectors/". When running integration tests, uncomment fhe following two blocks from pom.xml:

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

	
 7. Update the "Amazon Simple DB" properties file at location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/artifacts/ESB/connector/config" as below.
   
	i) accessKeyId 				- Your AWS account is identified by your Access Key ID. Sample account's Access Key ID is mentioned below under special notes. 
	ii) secretAccessKey         - Secret access key given in the account. Access key of the sample account is mentioned below under special notes.
	iii) version 				- Version of the API. The current version of the API is 2009-04-15. 
	iv) signatureVersion 		- The AWS signature version, current version value is "2".
	v) signatureMethod 			- Explicitly provide the signature method, which is "HmacSHA1".
	vi) amazonSimpleDBApiUrl 	- Current API url is 'https://sdb.amazonaws.com'.
	vii) domainName 			- The name of the domain to create. The name can range between 3 and 255 characters and can only contain the following characters: a-z, A-Z, 0-9, '_', '-', and '.'. 
								  Please make sure that a domain with the given name doen't exist in the current account. 
	viii) negativeDomainName 	- An invalid value for domain name. Eg: '@'
	ix) sleepTime 				- An integer value in milliseconds, to wait between API calls to avoid conflicts at API end. preferred value is 10000
	
	Allowed characters for the below mentioned parameters are all UTF-8 characters valid in XML documents.
	
	x) itemName 				- Unique identifier of an item. Eg: person 
	xi) attributeName 			- Name of an attribute associated with an item. Eg: age   
	xii) attributeValue 			- Value of attribute associated with an item. Eg: 25
	xiii) attributeName2 		- Name of an attribute associated with an item. Eg: name 
	xiv) attributeValue2 		- Value of attribute associated with an item. Eg: andrew 
	
	
 8. Navigate to "Integration_Test/products/esb/4.8.1/modules/integration/connectors/" and run the following command.
      $ mvn clean install

	  
	SPECIAL NOTE : Following Amazon Simple DB account details can be used to run the integration tests.
		Username : virtusa.wso2.connector@gmail.com
		Password : 1qaz2wsx@
		Access Key ID: AKIAIYMUNCUHQJEFMURA
		Secret Access Key: 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