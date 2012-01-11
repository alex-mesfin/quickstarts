Introduction
============
This quickstart demonstrates the usage of the XML validator. The service is almost same as the one
in the transform-xslt quickstart. The service contained in this quickstart is a dummy service
(OrderService) which simply returns the order object so that it can be processed by the transformer.
The object is then transformed from an Order object to an OrderAck object using XSLT.  
The only one difference is that XML validator of message contents is enabled in this service. So SOAP
Fault is caught when you submit a invalid SOAP request against XML Schema.

This example is invoked through a SOAP gateway binding.  

Running the quickstart
======================

JBoss AS 7
----------
1. Build the quickstart:
    mvn clean install
2. Start JBoss AS 7 in standalone mode:
    ./standalone
3. Deploy the Quickstart : 
    cp target/switchyard-quickstart-validate-xml-${version}.jar ${AS7}/standalone/deployments
4. Submit a webservice request to invoke the SOAP gateway.  There are a number of ways to do this :
      - Submit a request with your preferred SOAP client - src/test/resources/xml contains 
         sample requests and the responses that you should see
      - SOAP-UI : Use the wsdl for this project (src/main/resources/wsdl/OrderService.wsdl) to 
        create a soap-ui project. Use the sample request
        (src/test/resources/xml/soap-request-with-invalid-element.xml) 
        as an example of a sample request. The output below is the expected output : 

Expected Output
===============
```
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <soap:Fault>
         <faultcode>soap:Server</faultcode>
         <faultstring>Error during validation with '/xsd/orders.xsd' as 'XML_SCHEMA'.</faultstring>
      </soap:Fault>
   </soap:Body>
</soap:Envelope>
```
        And you can see the details of this error in the server.log.

        You also can see the normal response when you disable the validator in switchyard.xml.
        The output below is the expected output :

Expected Output
===============
```
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>  
   <SOAP-ENV:Body>  
      <orders:orderAck xmlns:orders="urn:switchyard-quickstart:validate-xml:0.1.0">  
         <orderId>PO-19838-XYZ</orderId>  
         <accepted>true</accepted>   
         <status>Order Accepted</status>  
      </orders:orderAck>  
   </SOAP-ENV:Body>  
</SOAP-ENV:Envelope>
```

