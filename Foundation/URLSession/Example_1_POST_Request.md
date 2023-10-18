# Example: POST request using default URLSession

## Swift language
## Requirements

- Foundation (Apple)
- Swift 5.0 or later
- iOS 13.0 or later

## Code
```swift
import Foundation

// Initialize URLComponents
var components = URLComponents()
components.scheme = "https"
components.host = "api.example.com"
components.path = "/data"

// Generate the URL from the components
guard let url = components.url else {
    fatalError("Could not create URL from components")
}

// Create a URLRequest (POST)
var request = URLRequest(url: url)
request.httpMethod = "POST"

// Add HTTP header fields
request.setValue("application/json", forHTTPHeaderField: "Content-Type") 

// Add HTTP body from JSON string
let jsonString = "{\"key1\": \"value1\", \"key2\": \"value2\"}"
let bodyData = jsonString.data(using: .utf8)
request.httpBody = bodyData

// Get default URLSession
let session = URLSession.shared

// Create a URLSessionTask (network task) for the request
let task = session.dataTask(with: request) { (data, response, error) in
    // Handle general errors
    if let error = error {
        print("Error: \(error)")
        return
    }
    
    // Verify that the response is an HTTPURLResponse object
    if let httpResponse = response as? HTTPURLResponse {
        // Check the status code
        if httpResponse.statusCode == 200 {
            // Process the received data
            if let data = data {
                print("Data received: \(data)")
            }
        } else {
            print("Received HTTP \(httpResponse.statusCode), expected 200")
        }
    } else {
        print("Response was not HTTPURLResponse")
    }
}

// Start the task
task.resume()
```

## Java language
## Requirements
- Foundation (CAFSoft)
- Java 11 or later

## Code
```java
// Initialize URLComponents
var components = new URLComponents();
components.setScheme("https");
components.setHost("api.example.com");
components.setPath("/data");

// Generate the URL from the components
var url = components.getURL();

// Create a URLRequest (POST)
var request = new URLRequest(url)
request.setHttpMethod("POST")

// Add HTTP header fields
request.setValue("application/json", "Content-Type");

// Add HTTP body from JSON string
var jsonString = "{\"key1\": \"value1\", \"key2\": \"value2\"}";
var bodyData = new Data(jsonString, StandardCharsets.UTF_8);
request.setHttpBody(bodyData);

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
                System.out.println(data.toText());
            }
        } else {
            // Status code is not equals to 200
        }
    }
});

// Start the task
task.resume();
```
