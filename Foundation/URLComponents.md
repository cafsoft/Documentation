# URLComponents class
The URLComponents class provides a structured way to construct and decompose URLs (Uniform Resource Locators). When working with URLs, it is often useful to break them down into their individual components, such as the scheme, host, path, and query parameters.

## Java language

### Example 1
```java
// Create a URLRequest instance with an URL
var components = new URLComponents("https://api.example.com/data");

// Set query component
components.setQuery("parameter1=value1&parameter2=value2");

// Generate the URL from the components
var url = components.getURL();
```

### Example 2
```java
// Create a URLRequest instance with an URL
var components = new URLComponents("https://api.example.com/data");

// Add query params
components.setQueryItems(new URLQueryItem[]{
  new URLQueryItem("parameter1", "value1"),
  new URLQueryItem("parameter2", "value2")
});

// Generate the URL from the components
var url = components.getURL();
```

### Example 3
```java
// Create a empty URLRequest instance
var components = new URLComponents();

// Add components
components.setScheme("https");
components.setHost("api.example.com");
components.setPath("/data");

// Add query params
components.setQueryItems(new URLQueryItem[]{
  new URLQueryItem("parameter1", "value1"),
  new URLQueryItem("parameter2", "value2")
});

// Generate the URL from the components
var url = components.getURL();
```
