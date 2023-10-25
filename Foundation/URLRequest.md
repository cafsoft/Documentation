# URLRequest class

The URLRequest class represents information about an HTTP request such as the URL, method (GET, POST, PUT, etc.), headers, and a the body.

## Java example 
```java
// Create a URLRequest instance
var request = new URLRequest(url);

// Set HTTP method
request.setMethod('POST'); //GET, POST, PUT

// Add HTTP header fields
request.setValue("application/json", "Content-Type");

// Add HTTP body from a String
var text = "key1=value&key2=value2";
var bodyData = new Data(text, StandardCharsets.UTF_8);
request.setHttpBody(bodyData);
```
