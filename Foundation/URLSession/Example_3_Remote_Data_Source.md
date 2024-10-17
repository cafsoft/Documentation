# Example: Create a Remote Data Source Class

## Java language
## Requirements
- Foundation (CAFSoft)
- Java 11 or later 

## Code
```java
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;

import java.net.URL;

import cafsoft.foundation.Data;
import cafsoft.foundation.HTTPURLResponse;
import cafsoft.foundation.URLRequest;
import cafsoft.foundation.URLSession;
import cafsoft.foundation.URLSessionDataTask;
import cafsoft.foundation.URLSessionDownloadTask;

public class RemoteDataSource {

    public URLSession getSession(){
        return URLSession.getShared();
    }

    private URLSessionDataTask createDataTask(URLRequest request,
                                              URLSession session,
                                              DataCompletionHandler dataCompletionHandler,
                                              ErrorCodeCompletionHandler errorCodeCompletionHandler) {

        var task = session.dataTask(request, (data, response, error) -> {
            // Handle general errors
            if (error != null) {
                // Network error
                errorCodeCompletionHandler.run(-1);
                return;
            }

            if (response instanceof HTTPURLResponse) {
                var httpResponse = (HTTPURLResponse) response;
                var statusCode = httpResponse.getStatusCode();

                if (statusCode == 200)
                    dataCompletionHandler.run(data);
                else
                    errorCodeCompletionHandler.run(statusCode);
            }
        });

        return task;
    }

    private void fetchData(URLRequest request,
                           URLSession session,
                           DataCompletionHandler dataCompletionHandler,
                           ErrorCodeCompletionHandler errorCodeCompletionHandler) {

        // Create a network task for the request
        var task = createDataTask(request, session, (data) -> {
            dataCompletionHandler.run(data);
        }, (errorCode) -> {
            errorCodeCompletionHandler.run(errorCode);
        });

        // Start the task
        task.resume();
    }

    public void fetchText(URLRequest request,
                          URLSession session,
                          TextCompletionHandler textCompletionHandler,
                          ErrorCodeCompletionHandler errorCodeCompletionHandler) {

        fetchData(request, session, (data) -> {
            var text = (String) null;
            if (data != null)
                text = data.toText();
            textCompletionHandler.run(text);
        }, (errorCode) -> {
            errorCodeCompletionHandler.run(errorCode);
        });
    }

    public void fetchImage(URLRequest request,
                           URLSession session,
                           ImageCompletionHandler imageCompletionHandler,
                           ErrorCodeCompletionHandler errorCodeCompletionHandler) {

        fetchData(request, session, (data) -> {
            var bitmap = (Bitmap) null;
            if (data != null)
                bitmap = BitmapFactory.decodeByteArray(data.toBytes(), 0, data.length());
            imageCompletionHandler.run(bitmap);
        }, (errorCode) -> {
            errorCodeCompletionHandler.run(errorCode);
        });
    }

    public interface DataCompletionHandler {
        void run(Data data);
    }

    public interface URLCompletionHandler {
        void run(URL url);
    }

    public interface TextCompletionHandler {
        void run(String text);
    }

    public interface ImageCompletionHandler {
        void run(Bitmap image);
    }

    public interface ErrorCodeCompletionHandler {
        void run(int errorCode);
    }
}
```
