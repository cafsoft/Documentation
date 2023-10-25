# URLSessionDataTask
The URLSessionDataTask class is used to perform tasks related to data transfer using the HTTP/HTTPS protocol, sending and receiving data as text, JSON, or any other data format. URLSessionDataTask instances return data directly to memory, making them ideal for short-lived network operations, such as getting JSON from an API.

## Java Language

### Example 1
```java
// Get Default URLSession
var session = URLSession.getShared();

// Create a URLSessionTask (network task) for the request
var task = session.dataTask(request, (data, response, error) -> {
  // Handle response here
});

// Start the task
task.resume();
```
### Example 2
```java
// Get default URLSession
var session = URLSession.getShared();

// Create a URLSessionTask (network task) for the request
var task = session.dataTask(request, (data, response, error) -> {

    // Handle general errors
    if (error != null){
        // Network error
        return;
    }

    // Verify that the response is an HTTPURLResponse object
    if (response instanceof HTTPURLResponse) {
        var httpResponse = (HTTPURLResponse) response;

        // Check the status code
        if (httpResponse.getStatusCode() == 200) {
            // Process the received data
            if (data != null) {
                var text = data.toText();
            }
        } else {
            // Status code is not equals to 200
        }
    }
});

// Start the task
task.resume();
```
