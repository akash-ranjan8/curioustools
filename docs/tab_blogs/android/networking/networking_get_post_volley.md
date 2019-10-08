# Networking Basics : Making a Get Post Connection(Volley)

In the 1st article of Networking series we, discussed the different terminologies related to networking in Android, along with some basic steps. As per 1st article,
- We need Internet and access network Permissions for creating a connection
- We might need to check for Internet availability  before  making a connection and during the data transfer(usually handled automatically)
(refer [1st article](https://www.google.co.in/search?q=todo%3A+change+this+link) )

In the [2nd article](https://www.google.co.in/search?q=todo%3A+change+this+link) , we saw how data can be downloaded using network fragment as a response string and how it could be set on the ui straightly.But production level apps doesn’t do that.They rather have a pretty ui where the data is set with full animations and loading screens.  
Even when we say "reponse string", we meant something like this:  
```json
{
  "items": [
    {
      "owner": {
        "reputation": 36476,
        "user_id": 7207392,
        "user_type": "registered",
        "accept_rate": 73,
        "profile_image": "https://www.gravatar.com/avatar/dadc9567dde5f02b09a29695eca1ce40?s=128&d=identicon&r=PG&f=1",
        "display_name": "Paul Panzer",
        "link": "https://stackoverflow.com/users/7207392/paul-panzer"
      },
      "is_accepted": false,
      "score": 0,
      "last_activity_date": 1570568059,
      "last_edit_date": 1570568059,
      "creation_date": 1570566735,
      "answer_id": 58293716,
      "question_id": 58292546
    },
    {
      "owner": {
        "reputation": 21,
        "user_id": 12165300,
        "user_type": "registered",
        "profile_image": "https://www.gravatar.com/avatar/05ad0547978cace7429e947736861196?s=128&d=identicon&r=PG&f=1",
        "display_name": "rsomji",
        "link": "https://stackoverflow.com/users/12165300/rsomji"
      },
      "is_accepted": false,
      "score": 0,
      "last_activity_date": 1570568055,
      "creation_date": 1570568055,
      "answer_id": 58293951,
      "question_id": 58289314
    },
    {
      "owner": {
        "reputation": 1982,
        "user_id": 2042457,
        "user_type": "registered",
        "accept_rate": 56,
        "profile_image": "https://i.stack.imgur.com/xQVEN.jpg?s=128&g=1",
        "display_name": "aksappy",
        "link": "https://stackoverflow.com/users/2042457/aksappy"
      },
      "is_accepted": false,
      "score": 0,
      "last_activity_date": 1570568054,
      "creation_date": 1570568054,
      "answer_id": 58293950,
      "question_id": 58290873
    },
    {
      "owner": {
        "reputation": 472124,
        "user_id": 1491895,
        "user_type": "registered",
        "accept_rate": 69,
        "profile_image": "https://www.gravatar.com/avatar/82f9e178a16364bf561d0ed4da09a35d?s=128&d=identicon&r=PG",
        "display_name": "Barmar",
        "link": "https://stackoverflow.com/users/1491895/barmar"
      },
      "is_accepted": true,
      "score": 1,
      "last_activity_date": 1570568051,
      "last_edit_date": 1570568051,
      "creation_date": 1570514725,
      "answer_id": 58280936,
      "question_id": 58280903
    },
    {
      "owner": {
        "reputation": 220,
        "user_id": 11601996,
        "user_type": "registered",
        "profile_image": "https://www.gravatar.com/avatar/51c452c987f2541f3c4d661a80359878?s=128&d=identicon&r=PG&f=1",
        "display_name": "Peter Li",
        "link": "https://stackoverflow.com/users/11601996/peter-li"
      },
      "is_accepted": false,
      "score": 0,
      "last_activity_date": 1570568046,
      "creation_date": 1570568046,
      "answer_id": 58293947,
      "question_id": 58264206
    }
  ],
  "has_more": true,
  "backoff": 10,
  "quota_max": 10000,
  "quota_remaining": 9995
}
```  
This is a reponse from the stack overflow Answers [API](https://api.stackexchange.com/2.2/answers?page=1&pagesize=5&order=desc&sort=activity&site=stackoverflow) .It has various fileds like  questionid, answereId, creationdata, etc but we didn’t even converted this  json string to some list of objects or strings, we just dumped it in our textview as it is.  

In a prettier version of our app :
1. Our data string would have been downloaded in a background thread, just like this
2. there would have been a recyclerview showing each question , with picture in left , title in top and subtitle below that.
3. We would then define a class ‘EachRowData’ with fields like id, question title, answertitle , creation date,etc ., with all those constructers and getters and setters
4. We would then convert out downloaded jsonString to the List<EachRowdata> by first converting  this string to JSONObject class’s object and then iterating through each object.This process is called converting from JSON to POJO (POJO:plain old java object)  

These are a lot of steps and their code, as we know, ends up being a massive monster. So there are many libraries build to perform above tasks in a simpler manner.   
- Volley/Okhttp/Retrofit are libraries which simplifies the process of downloading jsonString from api ( and some other tasks)  

- GSON is a popuar google library used for converting the json to pojo.  
- Picasso/glide/fresco/volley image are libraries build to simplify downloading and displaying of images. This is because  a downloaded bitstream would not be converted to a string and would require some extra functions like scaling ,transformation,etc

## volley
Volley is an HTTP library that makes networking for Android apps easier and, most importantly, faster. For more information about Volley and how to use it, visit the [Android developer training
page](https://developer.android.com/training/volley/index.html).  

### Adding to project:
```
   Implementation  'com.android.volley:volley:1.1.1'
```

### Usage(theory):  

1. create a response listener
2. create an error listener
3. create a response request
4. create a request queue
5. add request to queue

### Usage (Code):  

```java
public class Activity2 extends AppCompatActivity {
    TextView tv2; Button bt2;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_2);
        
        tv2=findViewById(R.id.tv2);
        bt2=findViewById(R.id.bt2);

        String url = "https://api.stackexchange.com/2.2/questions?pagesize=5&order=desc&sort=activity&site=stackoverflow";

        Response.Listener<String> responseListener = getResponseListener();
        Response.ErrorListener errorListener = getErrorListener();
        StringRequest stringRequest = new StringRequest(Request.Method.GET, url,responseListener,errorListener );
        RequestQueue queue = Volley.newRequestQueue(this);

        bt2.setOnClickListener(v-> queue.add(stringRequest));
    }
    private Response.ErrorListener getErrorListener() {
        return new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                tv2.setText("That didn't work!");
            }};
    }
    private Response.Listener<String> getResponseListener() {
        return new Response.Listener<String>() {
            @Override
            public void onResponse(String response) {
                // Display the first 500 characters of the response string.
                tv2.setText("Response is:\n" + response);
            }
        };
    }
```

Advantages of volley:  

- Automatic scheduling of network requests.  
- Multiple concurrent network connections.  
- Transparent disk and memory response caching with standard HTTP cache coherence.  
- Support for request prioritization.  
- Cancellation request API. You can cancel a single request, or you can set blocks or scopes of requests to cancel.  
- Ease of customization, for example, for retry and backoff.  
- Strong ordering that makes it easy to correctly populate your UI with data fetched asynchronously from the network.  
- Debugging and tracing tools.  

### Advance Operations:  

- **Post Request**  

[https://stackoverflow.com/a/33578202](https://stackoverflow.com/a/33578202)  

- **Handling  request cancelling**  

```
//before adding string request in queue:
Object TAG = "Hi";
stringRequest.setTag(TAG);	


// cancelling the request on click:
btCancelLoading.setOnClickListener(v->{
                   queue.cancelAll(TAG);
});

```

- **Making a custom Network queue with custom caching and thread handling**:   
[https://developer.android.com/training/volley/requestqueue](https://developer.android.com/training/volley/requestqueue)

- **Making a custom Network request** : like a direct json object request or direct pojo request :  
 [https://developer.android.com/training/volley/request-custom#example:-gsonrequest ](https://developer.android.com/training/volley/request-custom#example:-gsonrequest ). the example there shows ho we can use gson library alongside volley  to directly receive a list of pojos by extending Network request class
