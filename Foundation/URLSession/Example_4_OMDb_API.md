# Guide: Consuming the OMDb API using CAFSoft + Gson + Java Record in Android Studio with Empty Views Activities

## Prerequisites

1. **Android Studio 2024 or later.**  
   Use a modern version of Android Studio that supports Java 17 and Empty Views Activity templates.

2. **Java 17 or later (recommended).**  
   To use Java records and modern language features, configure your project as follows:

   **a. Groovy DSL (`build.gradle`)**

   ```groovy
   java {
       sourceCompatibility = JavaVersion.VERSION_17
       targetCompatibility = JavaVersion.VERSION_17
   }
   ```

   **b. Kotlin DSL (`build.gradle.kts`)**

   ```kotlin
   java {
       sourceCompatibility = JavaVersion.VERSION_17
       targetCompatibility = JavaVersion.VERSION_17
   }
   ```

3. **Include CAFSoft Foundation in your project.**  
   Follow this guide to configure CAFSoft Foundation:
   [CAFSoft - Setting up Android Studio Gradle Project](https://github.com/cafsoft/Documentation/blob/main/Foundation/SettingASGP.md)

4. **Add the Gson library** for JSON deserialization:

   **a. Groovy DSL (`build.gradle`)**

   ```groovy
   dependencies {
       implementation 'com.google.code.gson:gson:2.10.1'
   }
   ```

   **b. Kotlin DSL (`build.gradle.kts`)**

   ```kotlin
   dependencies {
       implementation("com.google.code.gson:gson:2.10.1")
   }
   ```

5. **Get an OMDb API key**  
   [https://www.omdbapi.com/apikey.aspx](https://www.omdbapi.com/apikey.aspx)

---

## Step 1: `RemoteDataSource.java` class

Use the `RemoteDataSource` base class. Official implementation:
[CAFSoft Example 3: Remote Data Source](https://github.com/cafsoft/Documentation/blob/main/Foundation/URLSession/Example_3_Remote_Data_Source.md)

This class provides:
- `getSession()` to get the shared session
- `fetchData(...)` for core HTTP operations
- `fetchText(...)`, `fetchImage(...)`
- Interfaces: `CompletionHandler<T>`, `TextCompletionHandler`, `ImageCompletionHandler`, etc.

---

## Step 2: Movie and Rating model using Java `record`

### Movie.java

```java
import com.google.gson.annotations.SerializedName;
import java.util.List;

public record Movie(
    @SerializedName("Title") String title,
    @SerializedName("Year") String year,
    @SerializedName("Released") String released,
    @SerializedName("Runtime") String runtime,
    @SerializedName("Genre") String genre,
    @SerializedName("Director") String director,
    @SerializedName("Writer") String writer,
    @SerializedName("Actors") String actors,
    @SerializedName("Plot") String plot,
    @SerializedName("Language") String language,
    @SerializedName("Country") String country,
    @SerializedName("Poster") String poster,
    @SerializedName("Ratings") List<Rating> ratings,
    @SerializedName("imdbRating") String imdbRating,
    @SerializedName("imdbID") String imdbID
) {}
```

### Rating.java

```java
import com.google.gson.annotations.SerializedName;

public record Rating(
    @SerializedName("Source") String source,
    @SerializedName("Value") String value
) {}
```

---

## Step 3: `OMDbAPI.java` class based on `RemoteDataSource`

```java
import cafsoft.foundation.*;
import com.google.gson.Gson;

public class OMDbAPI extends RemoteDataSource {
    private final String apiKey;
    private final Gson gson = new Gson();

    public OMDbAPI(String apiKey) {
        this.apiKey = apiKey;
    }

    public void fetchText(String name,
                          TextCompletionHandler textHandler,
                          ErrorCodeCompletionHandler errorHandler) {

        var components = new URLComponents();
        components.setScheme("https");
        components.setHost("www.omdbapi.com");
        components.setPath("/");
        components.setQueryItems(new URLQueryItem[]{
            new URLQueryItem("t", name),
            new URLQueryItem("type", "movie"),
            new URLQueryItem("apikey", apiKey)
        });

        var request = new URLRequest(components.getURL());
        fetchText(request, getSession(), textHandler, errorHandler);
    }

    public void fetchMovie(String name,
                           CompletionHandler<Movie> movieHandler,
                           ErrorCodeCompletionHandler errorHandler) {

        var components = new URLComponents();
        components.setScheme("https");
        components.setHost("www.omdbapi.com");
        components.setPath("/");
        components.setQueryItems(new URLQueryItem[]{
            new URLQueryItem("t", name),
            new URLQueryItem("type", "movie"),
            new URLQueryItem("apikey", apiKey)
        });

        var request = new URLRequest(components.getURL());

        fetchText(request, getSession(), text -> {
            try {
                Movie movie = gson.fromJson(text, Movie.class);
                movieHandler.run(movie);
            } catch (Exception e) {
                errorHandler.run(-1);
            }
        }, errorHandler);
    }
}
```

---

## Step 4: Usage example in Android

```java
public class MovieActivity extends AppCompatActivity {

    private final OMDbAPI omdbAPI = new OMDbAPI("your_api_key_here");

    private EditText txtTitle;
    private Button btnSearch;
    private TextView labTitle;
    private TextView labDirector;
    private TextView labYear;
    private TextView labGenre;
    private TextView labPlot;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_movie);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        initViews();
        initActions();
    }

    private void initViews() {
        txtTitle = findViewById(R.id.txtTitle);
        btnSearch = findViewById(R.id.btnSearch);
        labTitle = findViewById(R.id.labTitle);
        labDirector = findViewById(R.id.labDirector);
        labYear = findViewById(R.id.labYear);
        labGenre = findViewById(R.id.labGenre);
        labPlot = findViewById(R.id.labPlot);
    }

    private void initActions() {
        btnSearch.setOnClickListener(view -> {
            String title = txtTitle.getText().toString().trim();
            if (!title.isEmpty()) {
                searchMovieInfo(title);
            } else {
                Toast.makeText(this, "Please enter a movie title", Toast.LENGTH_SHORT).show();
            }
        });
    }

    private void searchMovieInfo(String name) {
        omdbAPI.fetchMovie(name, movie -> runOnUiThread(() -> {
            labTitle.setText(movie.title());
            labDirector.setText("Director: " + movie.director());
            labYear.setText("Year: " + movie.year());
            labGenre.setText("Genre: " + movie.genre());
            labPlot.setText(movie.plot());
        }), errorCode -> runOnUiThread(() -> {
            Toast.makeText(this, "API Error. Code: " + errorCode, Toast.LENGTH_LONG).show();
        }));
    }
}
```
