---
layout: post
title: Networking, JSON
date: 2018-03-21 10:30:20 +0530
description:  HTTP Networking, JSON Format, Usage. 
image:  jsonbanner.jpg # Add image post (optional)
tags: [Android, JSON, HTTP Networking]
---

In your ever day tasks, on web, you request (Query) for something either 
by clicking some links or entering some link. You get a response.

Basically, You Query from some Client (Mobile/Laptop]. You Get response 
from Server (Google/Twitter/Facebook), which is serving your Query 
(called Web Services).

When you get a Response, data is in a particular format chosen by the Server. 
Like, JSON, XML.  This data is then extracted/converted and is 
updated to UI of your application (Like Twitter/fb)

3 sub topics to cover these include:

* Whats in http link.
* HTTP Networking (Send & Receive) 
* JSON (Parsing)

## Whats in http link.

Entering/Quering "google io" in youtube search bar, is same as forming 
the below link with Query information.

https://www.youtube.com/results?search_query=google+io

`https:` is Protocol/Scheme.

`www.youtube.com` is Host/Domain/Authority.

`results` is Resource Path.

`?search_query=Google+io` is Query.

Query starts with `?` and if you notice its a key/value pair. 

Here in this link, Key is search_query; Value is Google+io.

Similary, Read this link. It has 2 Key-Value pairs.

https://api.github.com/search/repositories?q=android&sort=stars

Enter the link and see the response you get. Use JSON Formatter online, 
to see the response in a cleaner way. Like JSON Pretty print, 
jsonformatter curiousconcept 


## HTTP Networking

Its exchange of Communication that happends between Computers.

Steps involved are:

1. Form HTTP Request

2. Send the Request
> Most commonly used Methods are
> * GET    -> to Get/Receive data
> * POST   -> to Create New Information.
> * PUT    -> to Update existing Information.
> * DELETE -> to Delete data.

3. Receive the Response & Parse it.
> For every response you will receive Status Code which describes 
  Success/Error/Information.
> * 1xx - Information Response Code
> * 2xx - Success 
> * 3xx - Redirection
> * 4xx - Client Errors
> * 5xx - Server Errors

4. Update UI.

  **Form URL**

{% highlight JAVA %}
final static String GITHUB_BASE_URL = "https://api.github.com/search/"
final static String PARAM_QUERY = "q";
final static String PARAM_SORT = "sort";
final static String sortBy = "stars";
    
Uri buildUri = Uri.parse(GITHUB_BASE_URL).buildUpon()
              .appendQueryParameter(PARAM_QUERY, githubSearchQuery)
              .appendQueryParameter(PARAM_SORT, sortBy)
              .build();          
    
// To convert this Android URI, to Java URL, pass URI string to URL 
//Constructor.

URL url = null;
try {
    url = new URL(builtUri.toString());
 } catch (MalformedURLException e) {
 	e.printStackTrace();
 }

{% endhighlight %}

  **Open Connection**

{% highlight JAVA %}

HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();
urlConnection.setRequestMethod("GET"); 
urlConnection.setReadTimeout(10000/*milli seconds*/);
urlConnection.setConnectTimeout(15000/*milli seconds*/);
urlConnection.connect();

//Not always required to call .connect(). Connection will be performed 
//implicity, when getInputStrem(), getOutputStream() are called.
//HttpURLConnection uses GET by default.
//Uses POST if setDoOutput(true)
//setRequestMethod(String) for OPTIONS, HEAD, PUT, DELETE and TRACE 

{% endhighlight %}

  **Get Response**

{% highlight JAVA %}

InputStream inputStream = urlConnection.getInputStream();

//inputStream is stream of bytes, binary data.

{% endhighlight %}

  **Stream Parsing**

{% highlight JAVA %}

  public static String getResponseFromHttpUrl(URL url) 
  throws IOException 
 {
   HttpURLConnection urlConnection = 
   (HttpURLConnection) url.openConnection();

   try {
   InputStream in = urlConnection.getInputStream();

   Scanner scanner = new Scanner(in);

   scanner.useDelimiter("\\A"); 
   //Regex, \A – The beginning of the input. With this you will get 2 parts,
   //if there is data. One, till the beginning, Another, From beginning.
   //This seems to force the scanner to read the entire contents from
   //beginning to end.

   boolean hasInput = scanner.hasNext(); //checking for 2nd part.

   if (hasInput) {
   return scanner.next(); 
   //For this url api, we get JSON Format, JsonResponse.
   } else {
   return null;
   }
  } finally {
   urlConnection.disconnect();
  }
}
{% endhighlight %}

Other Way: 
{% highlight JAVA %}

 private String readFromStream(InputStream inputStream) 
 throws IOException {

  StringBuilder output = new StringBuilder();

  if (inputStream != null) {
   InputStreamReader inputStreamReader = 
   			new InputStreamReader(inputStream, 
   			Charset.forName("UTF-8"));

   BufferedReader reader = new BufferedReader(inputStreamReader);

   String line = reader.readLine();

   while (line != null) {

   output.append(line);

   line = reader.readLine();

   }
  }
  return output.toString(); 
  //For this url api, we get JSON Format, JsonResponse.
}

{% endhighlight %}

{% highlight JAVA %}

//InputStreamReader< allows to read only Single byte at a time. 

//BufferedReader accept request for Char, Reads from Input Stream and builds
//up a String with entire contents. Is Synchronus. Should be used if working
//with multiple threads.

//Scanner pulls the data, buffers as needed, converts UTF-8(Java Encoding) 
//to UTF-16(Android encoding) all automatially.

//StringBuilder is Mutable (Can change once created).

{% endhighlight %}

Check <a href="https://stackoverflow.com/questions/8798403/string-is-immutable-what-exactly-is-the-meaning" target="_blank">this</a> link for  comparision between String/StringBuilder.

Check <a href="https://www.davismol.net/2015/05/07/bufferedreader-and-fileinputstream-vs-scanner-performance-comparison-in-reading-and-parsing-a-200k-lines-text-file/" target="_blank">this</a> link for performance comparision between Scanner/BufferedReader.

## JSON - Java Script Object Notation.

It is a very common language-independent data format used for asynchronous browser–server communication, including as a replacement for XML 

It was derived from JavaScript, and JSON filenames use the extension .json.

**JSON Format is as below**

![JSONFormat]({{site.baseurl}}/images/jsonformat.jpg)

Android provides 4 JSON Classes to extract information.

* JSONArray
* JSONObject
* JSONStringer
* JSONTokener

**JSON Parsing**

{% highlight JAVA %}
JSONObject        root = new JSONObject(JsonResponse);
String       firstName = root.getString("firstName");
int                age = root.getInt("age");

JSONObject     address = root.getJSONObject("address");
JSONArray   phoneArray = root.getJSONArray("phoneNumbers");
JSONObject phoneObject = phoneArray.getJSONObject(0); //Array index
{% endhighlight %}

References: Udacity, Android Developer Document, Code Path, StackOverflow.