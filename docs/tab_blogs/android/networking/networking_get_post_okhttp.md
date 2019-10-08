# Networking Basics : Making a Get Post Connection(Okhttp)

In the 1st article of Networking series we, discussed the different terminologies related to networking in Android, along with some basic steps. As per 1st article,
- We need Internet and access network Permissions for creating a connection
- We might need to check for Internet availability  before  making a connection and during the data transfer(usually handled automatically)
(refer [1st article](https://www.google.co.in/search?q=todo%3A+change+this+link) )

## okhttp
This is not as advanced as volley library, but still reduces our code by a lot. It was originally made to be an internal part of retrofit library, which is the actual massive library for multiple usages, but on popular demand, it was derived as a separate library for simpler use cases . thus we have to handle the asynchronous threading of netwrk by ourselves.

### Adding to project:
```
   implementation 'com.squareup.okhttp3:okhttp:4.0.0-RC1'

```

### Usage(theory):  

meh. see code. very small stuff

### Usage (Code):  

```java
public class Activity3 extends AppCompatActivity {
    private static final String TAG = "Activity3>>";TextView tv3;Button bt3;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_3);
        tv3 = findViewById(R.id.tv3);
        bt3 = findViewById(R.id.bt3);
        bt3.setOnClickListener(v -> startNetworkRequest());
    }

    private void startNetworkRequest() {
        String pagesize = "5";
        String order = "desc";
        String urlPost = "http://api.stackexchange.com/2.2/questions?";
        String urlGet = "https://api.stackexchange.com/2.2/questions?pagesize=5&order=desc";

        new Thread(() -> {
            String responseGet = apiRequestGet(urlGet);  // String responsePost=apiRequestPOST(urlPost,pagesize,order);// can't check,not sure about it
            runOnUiThread(()->tv3.setText(responseGet));
        }).start();
    }

    private String apiRequestGet(String urlToUploadto) {
        String responseJsonString = "";

        OkHttpClient client = new OkHttpClient();

        Request request = new Request.Builder().url(urlToUploadto).build();
        try {
            Response response = client.newCall(request).execute();
            responseJsonString = response.body().string();
        } catch (IOException e) {
            e.printStackTrace();//this might be caused due response is not being recieved

        }
        return responseJsonString;
    }

    private String apiRequestPOST(String urlToUploadto, String pagesize, String order) {
        String responseJsonString = "";

        OkHttpClient client = new OkHttpClient();
        RequestBody jsonFormBody = new FormBody.Builder()
                .add("pagesize", pagesize)
                .add("order", order).build();
        Request request = new Request.Builder().url(urlToUploadto).post(jsonFormBody).build();
        try {
            Response response = client.newCall(request).execute();
            responseJsonString = response.body().string();
        } catch (IOException e) {
            e.printStackTrace();//this might be caused due response is not being recieved

        }
        return responseJsonString;
    }

```  

### Advance Operations:  

there is an excellent guide at [https://github.com/chaostools/okhttp/blob/master/RECIPES.md](https://github.com/chaostools/okhttp/blob/master/RECIPES.md) (my fork of ok http, just in case ok http removes them later) covering some advanced usecases like video streaming post,  response caching, etc

