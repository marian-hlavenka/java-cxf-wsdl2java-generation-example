# Java CXF wsdl2java generation
## Description
I found a problem when generating Java objects from a wsdl file and the name of the element used in message and the name of the operation was identical. In this case it causes the generated interface service to have void methods with combined parameters of the request and response.<br/>
In this example i have two Operations **MyRequest1** and **MyRequest2**. The name of the element in the input message is different for the operation **MyRequest1** and the same for operation **MyRequest2**.

## Generated Service interface
In this Service interface the generated code looks like this:
```Java
@WebService(targetNamespace = "http://myWsdl/1.0", name = "IMyService")
@XmlSeeAlso({myschema._1.ObjectFactory.class})
public interface IMyService {

    @WebMethod(operationName = "MyRequest1")
    @SOAPBinding(parameterStyle = SOAPBinding.ParameterStyle.BARE)
    @WebResult(name = "MyResponse", targetNamespace = "http://mySchema/1.0", partName = "parameters")
    public myschema._1.MyResponse myRequest1(

        @WebParam(partName = "parameters", name = "MyRequest", targetNamespace = "http://mySchema/1.0")
        myschema._1.MyRequest parameters
    );

    @WebMethod(operationName = "MyRequest2")
    @RequestWrapper(localName = "MyRequest2", targetNamespace = "http://mySchema/1.0", className = "myschema._1.MyRequest2")
    @ResponseWrapper(localName = "MyResponse2", targetNamespace = "http://mySchema/1.0", className = "myschema._1.MyResponse2")
    public void myRequest2(

        @WebParam(name = "field1req", targetNamespace = "")
        java.lang.String field1Req,
        @WebParam(mode = WebParam.Mode.INOUT, name = "field2shared", targetNamespace = "")
        javax.xml.ws.Holder<java.lang.String> field2Shared,
        @WebParam(mode = WebParam.Mode.OUT, name = "field3out", targetNamespace = "")
        javax.xml.ws.Holder<java.lang.String> field3Out
    );
}
```