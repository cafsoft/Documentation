# URLSessionDownloadTask
URLSessionDownloadTask is a class that manages downloading files directly from the server to disk. It is ideal for downloading large content such as multimedia files, documents or large data packets. Upon completion of the download, it provides a path to the temporary file where the content has been stored, allowing developers to move the file to a desired location or process the content as required.

## Java Language

### Example 1
```java
// Get Default URLSession
var session = URLSession.getShared();

// Create a URLSessionTask (network task) for the request
var task = session.downloadTask(request, (location, response, error) -> {
  // Manage downloaded file here
});

// Start the asynchronous task
task.resume();
```
### Example 2
```java
// Get default URLSession
var session = URLSession.getShared();

// Create a URLSessionTask (network task) for the request
var task = session.dataTask(request, (localURL, response, error) -> {

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
            // Manage downloaded file
            
        } else {
            // Status code is not equals to 200
        }
    }
});

// Start the asynchronous task
task.resume();
```
