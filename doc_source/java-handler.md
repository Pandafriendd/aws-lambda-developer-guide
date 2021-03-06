# AWS Lambda Function Handler in Java<a name="java-handler"></a>

At the time you create a Lambda function you specify a handler that AWS Lambda can invoke when the service executes the Lambda function on your behalf\. 

Lambda supports two approaches for creating a handler: 
+ Loading the handler method directly without having to implement an interface\. This section describes this approach\.
+  Implementing standard interfaces provided as part of `aws-lambda-java-core` library \(interface approach\)\. For more information, see [Using Provided Interfaces for Java Function Handlers in AWS Lambda](java-handler-interfaces.md)\. 

The general syntax for the handler is as follows:

```
outputType handler-name(inputType input, Context context) {
   ...
}
```

In order for AWS Lambda to successfully invoke a handler it must be invoked with input data that can be serialized into the data type of the `input` parameter\. 

In the syntax, note the following:
+  *inputType* – The first handler parameter is the input to the handler, which can be event data \(published by an event source\) or custom input that you provide such as a string or any custom data object\. In order for AWS Lambda to successfully invoke this handler, the function must be invoked with input data that can be serialized into the data type of the `input` parameter\.
+ *outputType* – If you plan to invoke the Lambda function synchronously \(using the `RequestResponse` invocation type\), you can return the output of your function using any of the supported data types\. For example, if you use a Lambda function as a mobile application backend, you are invoking it synchronously\. Your output data type will be serialized into JSON\. 

  If you plan to invoke the Lambda function asynchronously \(using the `Event` invocation type\), the `outputType` should be `void`\. For example, if you use AWS Lambda with event sources such as Amazon S3 or Amazon SNS, these event sources invoke the Lambda function using the `Event` invocation type\.
+ The *inputType* and *outputType* can be one of the following:
  + Primitive Java types \(such as String or int\)\.
  + Predefined AWS event types defined in the `aws-lambda-java-events` library\. 

    For example `S3Event` is one of the POJOs predefined in the library that provides methods for you to easily read information from the incoming Amazon S3 event\.
  + You can also write your own POJO class\. AWS Lambda will automatically serialize and deserialize input and output JSON based on the POJO type\. 
+ You can omit the `Context` object from the handler method signature if it isn't needed\. For more information, see [AWS Lambda Context Object in Java](java-context.md)\.

For example, consider the following Java example code\. 

```
package example;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;

public class Hello implements RequestHandler<Integer, String>{
  public String handleRequest(Integer myCount, Context context) {
    return String.valueOf(myCount);
  }
}
```

In this example input is of type Integer and output is of type String\. If you package this code and dependencies, and create your Lambda function, you specify `example.Hello` as the handler\. 

If your Java code contains multiple methods with same name as the handler, then AWS Lambda uses the following rules to pick a method to invoke:

1. Select the method with the largest number of parameters\.

1. If two or more methods have the same number of parameters, AWS Lambda selects the method that has the `Context` as the last parameter\. 

   If none or all of these methods have the `Context` parameter, then the behavior is undefined\.

When you define your handler, you can use built\-in types, POJOs, or streams\.

The following Java class shows a handler called `myHandler` that uses String type for input and output\.

```
package example;

import com.amazonaws.services.lambda.runtime.Context; 

public class Hello {
    public String myHandler(String name, Context context) {
        return String.format("Hello %s.", name);
    }
}
```

AWS Lambda supports the String, Integer, Boolean, Map, and List types\.

**Topics**
+ [Using POJO Input and Output Types for Java Functions in AWS Lambda](java-handler-pojo.md)
+ [Using Stream Input and Output Types for Java Functions in AWS Lambda](java-handler-stream.md)
+ [Using Provided Interfaces for Java Function Handlers in AWS Lambda](java-handler-interfaces.md)