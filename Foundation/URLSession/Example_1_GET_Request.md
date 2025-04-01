# Example: GET request using default URLSession

## Swift language
## Requirements

- Foundation (Apple)
- Swift 5.0 or later

## Code
```swift
import Foundation

// Initialize URLComponents
var components = URLComponents()
components.scheme = "https"
components.host = "api.example.com"
components.path = "/data"
components.queryItems = [
    URLQueryItem(name: "parameter1", value: "value1"),
    URLQueryItem(name: "parameter2", value: "value2")
]

// Generate the URL from the components
guard let url = components.url else {
    fatalError("Could not create URL from components")
}

// Create a URLRequest (GET)
var request = URLRequest(url: url)

// Get Default URLSession
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
components.setQueryItems(new URLQueryItem[]{
  new URLQueryItem("parameter1", "value1"),
  new URLQueryItem("parameter2", "value2")
});

// Generate the URL from the components
var url = components.getURL();

// Create URLRequest (GET)
var request = new URLRequest(url);

// Get Default URLSession
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
