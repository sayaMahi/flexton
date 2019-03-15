---
layout: post
title: Retrofit 2.0 and Network Logging.
date: 2018-07-04 11:00:20 +0530
description:  # This post discusses about Basic setup of Retrofit 2.0 and Network Debugging.
image:  retrofit_banner.jpg 
tags: [Android Tutorials, Retrofit]
categories: Retrofit-2.0 Android
---

## Retrofit 2.0

This is one popular Networking library from <a href="http://square.github.io/retrofit/" target="_blank">Square.</a> It also does support converting your JSON response directly to a Model Object.

In this post we shall be using Gson converter. Other converters can be found <a href="http://square.github.io/retrofit/" target="_blank">here.</a>

If you are new to Gson, you may find details <a href="http://guides.codepath.com/android/leveraging-the-gson-library" target="_blank">here</a> on how to start with.

<form>
<input style="background:#3366cc; cursor: pointer; color: #fff; border-radius: 3px; border: 1px solid #3366cc;" class='c-btn' type="button" value="Download Code" onclick="window.open('http://github.com/sayaMahi/PopularMovies')" target="_blank" />  &  `git checkout 0162f80`
</form> 

Consider below APIs to GET data: 
- http://api.themoviedb.org/3/movie/upcoming?api_key=\<api-key-value\>&page=1
- http://api.themoviedb.org/3/movie/top_rated?api_key=\<api-key-value\>&page=1
- http://api.themoviedb.org/3/movie/{movie_id}/images?api_key=\<api-key-value\>&page=1

If we segregate this to different sections,

 - Base URL: http://api.themoviedb.org/3/
 - Relative path/API End Points: 
    - movie/upcoming, 
    - movie/top_rated, 
    - movie/{movie_id}/images.
 - Query:
    - api_key=\<api-key-value\>
    - page=1

Traditionally, URLBuilder is used to construct the URL. With different relative paths to append and to query, code doesnt look concise. 

Retrofit comes to the rescue in converting this process to be a simple Java Interface Methods using annotations, and does all the URL manipulation, requesting/loading, converting under the hood.

Below are the steps:

#### Gralde

{% highlight XML %}

    // Retrofit
    implementation 'com.squareup.retrofit2:retrofit:2.4.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.4.0'

{% endhighlight %}

### Retrofit Instance: 

{% highlight JAVA %}

public class MovieInterface {

    private final static String MOVIES_BASE_URL = "http://api.themoviedb.org/3/";
    private static Retrofit retrofit;
    private static MovieService movieService;

    public static Retrofit getMovieInterface() {

        if(null == retrofit) {

            retrofit = new Retrofit.Builder()
                     .baseUrl(MOVIES_BASE_URL)
                     .addConverterFactory(GsonConverterFactory.create())
                     .build();
        }
        return retrofit;
    }

    public static MovieService getMovieService(){
        if(null == movieService) {
            movieService = getMovieInterface().create(MovieService.class);
        }
        return movieService;
    }
}
{% endhighlight %}

Creating instance of retrofit and interface are costly, so make sure you do it once once and return the existing instance.

### Relative Paths/End Points:

{% highlight JAVA %}

public interface MovieService {

    @GET("movie/top_rated")
    Call<Movie> getTopRatedMovies(@Query("api_key") String apiKey, @Query("page") String pageNum);

    @GET("movie/popular")
    Call<Movie> getPopularMovies(@Query("api_key") String apiKey, @Query("page") String pageNum);

    @GET("movie/{movie_id}/images")
    Call<Movie> getMovieDetail(@Path("movie_id") int id);
}
{% endhighlight %}

- @GET   annotation is for server request, followed with relative paths.
- @Query annotation is to append keyname and its value (passed while calling function).
- @Path  annotation is to replace variable defined in {...} with value (passed while calling function).
- Call\<T\> is a call-back with T being Model object.

Recommended Way: Base URL ends with "/", End-point does not start with "/"

- Other Ways:

{% highlight JAVA %}
Base URL:  "www.androidcitizen.com/xyz/" ;
End Point: @GET("user/abc") ;
==> "www.androidcitizen.com/xyz/user/abc"

Base URL:  "www.androidcitizen.com/xyz" ; 
End Point: @GET("user/abc")
==> "www.androidcitizen.com/user/abc"

{% endhighlight %}

If End Point doesn't start with "/", it simply appends to BaseURL. If EndPoint starts with "/", it appends to the host/domain/Authority from BaseURL and ignores the rest.

**Instance**

{% highlight JAVA %}
    MovieService movieService = MovieInterface.getMovieInterface().createMovieService.class);
        
    Call<Movie> moviesServiceCall = movieService.getAllMovies(API_KEY, PAGE_NUM);

{% endhighlight %}

**Async Call:**

{% highlight JAVA %}
    moviesSerivceCall.enqueue(new Callback<Movie>() {
            @Override
            public void onResponse(Call<Movie> call, Response<Movie> response) {
               //returns converted JSON in response.body()
            }

            @Override
            public void onFailure(Call<Movie> call, Throwable t) {
                // process error
            }
        });
    }
{% endhighlight %}

**Sync Call**

{% highlight JAVA %}
    Movie movie = moviesSerivceCall.execute();
{% endhighlight %}

Sync call runs on Main thread. If being used in Android, makesure you run in a background thread.

You now have Retrofit Integrated successfully.

## Network Logging:

In order to Debug the Network Requests/Responses, there is no direct way. But there are 3rd party libraries which comes in handy to make the overall process easier.


### HTTPLoggingInterceptor [Library]

Once enabled, all the requests/responses over Network will be Logged in Log Console. You can also control the level of Logging.

#### Gralde

{% highlight XML %}
    implementation 'com.squareup.okhttp3:logging-interceptor:3.9.1'
{% endhighlight %}

#### Instance

{% highlight JAVA %}

HttpLoggingInterceptor loggingInterceptor = new HttpLoggingInterceptor();

/* Set the Logging Level */
loggingInterceptor.setLevel(HttpLoggingInterceptor.Level.BODY);

OkHttpClient.Builder httpClient = new OkHttpClient.Builder();

/*
If condition makes sure, Logs are printed only in the development mode.
*/
if(BuildConfig.DEBUG) {
    httpClient.addInterceptor(loggingInterceptor);
    }

retrofit = new Retrofit.Builder()
        .baseUrl(MOVIES_BASE_URL)
        .addConverterFactory(GsonConverterFactory.create())
        .client(httpClient.build())
        .build();

{% endhighlight %}

Different Logging Levels:

- BASIC: Logs request, response lines.
- BODY : Logs request, response lines, respective headers, bodies.
- HEADERS: Logs request, response lines, their respective headers.
- NONE: No logs.

You may find additional details <a href="http://square.github.io/okhttp/3.x/logging-interceptor/" target="_blank">here</a>.

But the problem with this library is, all the Logged data gets cluttered on Log Console. Stetho is another library, which handles this very well.

### Stetho [Library]

Stetho is a wonderful library from Facebook, where all the Network Logs are segregated and are displayed on Chrome Browser's Developer Tool.

#### Gralde

{% highlight XML %}
    // Stetho Log Interceptor
    implementation 'com.facebook.stetho:stetho:1.5.0'
    // Network Helper
    implementation 'com.facebook.stetho:stetho-okhttp3:1.5.0'
{% endhighlight %}

#### Initialize

MainActivity.java
{% highlight JAVA %}
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Stetho.initializeWithDefaults(this);
        ...
    }
{% endhighlight %}  

#### Manifest

{% highlight XML %}

Make sure MainActivity.java where the setup is done is registered 
in manifest file under <application/> or launch <activity/>.

android:name="MainActivity"

{% endhighlight %}  

#### Instance

{% highlight JAVA %}

OkHttpClient.Builder httpClient = 
      new OkHttpClient.Builder().addNetworkInterceptor(new StethoInterceptor());

retrofit = new Retrofit.Builder()
        .baseUrl(MOVIES_BASE_URL)
        .addConverterFactory(GsonConverterFactory.create())
        .client(httpClient.build())
        .build();    

{% endhighlight %}  

You may find additional details <a href="http://facebook.github.io/stetho/" target="_blank">here</a> and <a href="http://github.com/facebook/stetho" target="_blank">here</a>

#### Run

Enter and run "chrome://inspect" on Chrome Browser.

If there are any emulators/Devices connected over ADB, Your application will show up here. Click on "inspect" label.

You will have this window pop-up here.

![Network-Debug-1]({{site.baseurl}}/images/network_debug.jpg)

This info is very much helpful in fixing any issues. In the above Image, we can see same request was sent twice (successfully) which needs to be fixed.

### Network Profiler

Additionally, Network Debugging can be done on Android Studio by in-built tools.

Step 1:
Select Edit Configurations.
![Network-Debug-2]({{site.baseurl}}/images/network_profiler_1.jpg)

Step 2:
Enable Profiling.

![Network-Debug-3]({{site.baseurl}}/images/network_profiler_2.jpg)

Step 3:
Select Android Profiler on the bottom tab.
![Network-Debug-4]({{site.baseurl}}/images/network_profiler_3.jpg)

References:

[Post 1][link-1]
[Post 2][link-2]
[link-1]: http://inthecheesefactory.com/blog/retrofit-2.0/en
[link-2]: http://guides.codepath.com/android/Consuming-APIs-with-Retrofit