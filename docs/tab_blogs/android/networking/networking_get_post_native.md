# Networking Basics : Making a Get Post Connection(Native Way)

In the 1st article of Networking series we, discussed the different terminologies related to networking in Android, along with some basic steps. As per 1st article,
- We need Internet and access network Permissions for creating a connection
- We might need to check for Internet availability  before  making a 
  connection and during the data transfer(usually handled automatically)
refer [1st article](https://www.google.co.in/search?q=todo%3A+change+this+link)

So let's see how we can use Native HttpConnection libaries to make a connection.  
We would be creating an http connection inside an asynctask(a nice version of thread that handles supplying progress updates on the main thread)whose object would be handled by [headless fragment](https://www.androiddesignpatterns.com/2013/04/retaining-objects-across-config-changes.html) (very nice article) , in order to prevent network from failing on configuration change.  
you can handle that in a service as well, but the main file remains the Downloader task , the callback class , and the Response Class.  

### The Result Class
The Result class is the final encapsulation of reponse received. When data is recieved by the user's device, it is in the form of a stream of bits. The Java's Inputstream class is either able to fetch that input stream or throws some relevent exception .  

The downloader will either Convert that stream to string and put it in this class's Object or take the exception and put it in this class's Object and return this class's Object (Too much repeatitio, I know :/).  
```java

class Result {
    String resultValue;
    Exception exception;
    Result(String resultValue) {
        this.resultValue = resultValue;
    }
    Result(Exception exception) {
        this.exception = exception;
    }
}

```

### The Download Listener and Progress interface. 
The download listener is a listener implemented by Activity and used by the Downloader for communication. The downloader would push updates to this interface's object  regarding downloading started, progress updates, errors ,etc.  

```java

public interface DownloadListener<T> {
    interface Progress {
        int ERROR = -1;
        int CONNECT_SUCCESS = 0;
        int GET_INPUT_STREAM_SUCCESS = 1;
        int PROCESS_INPUT_STREAM_IN_PROGRESS = 2;
        int PROCESS_INPUT_STREAM_SUCCESS = 3;
    }

    void updateFromDownloader(@Nullable Result result);
    NetworkInfo getActiveNetworkInfo();
    void onProgressUpdate(int progressCode, int percentComplete);//todo define a way for showing partial updated
    void onDownloadingFinished(@Nullable Result result);

}
```



### The Downloader Asynctask
The downloader Asynctask is the main Downloading Body here . It downloads the response from an api as input stream, converts it to a `Result` class's object and returns the result using `Download Listener`'s object.

```java
public class DownloadTask extends AsyncTask<String, Integer, Result> {

    private DownloadListener<String> downloadListener;

    DownloadTask(DownloadListener<String> downloadListener) {
        setDownloadListener(downloadListener);
    }
    private void setDownloadListener(DownloadListener<String> downloadListener) {
        this.downloadListener = downloadListener;
    }

    @Override
    protected void onPreExecute() {
        if (downloadListener != null) {
            NetworkInfo networkInfo = downloadListener.getActiveNetworkInfo();
            if (networkInfo == null || !networkInfo.isConnected() ||
                    (networkInfo.getType() != ConnectivityManager.TYPE_WIFI &&
                            networkInfo.getType() != ConnectivityManager.TYPE_MOBILE)) {

                downloadListener.updateFromDownloader(null);
                cancel(true);
            }
        }
    }

    @Override
    protected Result doInBackground(String... urls) {
        Result result = null;
        if (!isCancelled() && urls != null && urls.length > 0) {
            String urlString = urls[0];
            try {
                URL url = new URL(urlString);
                String resultString = downloadUrl(url);
                if (resultString != null) {
                    result = new Result(resultString);
                } else {
                    throw new IOException("No response received.");
                }
            } catch(Exception e) {
                result = new Result(e);
            }
        }
        downloadListener.updateFromDownloader(result);
        return result;
    }
    private String downloadUrl(URL url) throws IOException {
        InputStream stream = null;
        HttpsURLConnection connection = null;
        String result = null;
        try {
            connection = (HttpsURLConnection) url.openConnection();
            connection.setReadTimeout(30000);
            connection.setConnectTimeout(30000);
            connection.setRequestMethod("GET");
            connection.setDoInput(true);
            connection.connect();
            publishProgress(DownloadListener.Progress.CONNECT_SUCCESS);
            int responseCode = connection.getResponseCode();
            if (responseCode != HttpsURLConnection.HTTP_OK) {
                throw new IOException("HTTP error code: " + responseCode);
            }
            stream = connection.getInputStream();
            publishProgress(DownloadListener.Progress.GET_INPUT_STREAM_SUCCESS, 0);
            if (stream != null) {
                result = readStream(stream);
            }
        } finally {
            if (stream != null) {
                stream.close();
            }
            if (connection != null) {
                connection.disconnect();
            }
        }
        return result;
    }
    private String readStream(InputStream stream) throws IOException {
        int maxReadSize=500;
        Reader reader;
        reader = new InputStreamReader(stream, StandardCharsets.UTF_8);
        char[] rawBuffer = new char[maxReadSize];
        int readSize;
        StringBuilder buffer = new StringBuilder();
        while (((readSize = reader.read(rawBuffer)) != -1) && maxReadSize > 0) {
            if (readSize > maxReadSize) {
                readSize = maxReadSize;
            }
            buffer.append(rawBuffer, 0, readSize);
            maxReadSize -= readSize;
        }
        return buffer.toString();

        //for bitmap
        //InputStream inputStream = null;
        //...
        //Bitmap bitmap = BitmapFactory.decodeStream(inputStream);
        //ImageView imageView = (ImageView) findViewById(R.id.image_view);
        //imageView.setImageBitmap(bitmap);

    }

    @Override
    protected void onPostExecute(Result result) {
        if (result != null && downloadListener != null) {
            downloadListener.onDownloadingFinished(result);
        }
    }


    @Override
    protected void onProgressUpdate(Integer... values) {
        super.onProgressUpdate(values);
        //downloadListener.onProgressUpdate(int progressCode, int percentComplete);
        // ^ todo define a way for showing partial updated
    }

    @Override
    protected void onCancelled() {
        super.onCancelled();
    }
}


```
  
### The Network Fragment
This is the main Fragment that will be handling Downloader's Instance and preventing it from getting killed on changes like activity pausing or Screen getting rotated. It is not as permanent solution as that of a forgeround service(forground, because background service is now deprecated), which could run even if activity is destroyed, but it is still a hard core solution for downloading without worrying about configuration changes or screen lock or pause/play stuff . 
More about Headless fragment is discussed [here](https://medium.com/@ghbhatt/my-experiments-with-headless-fragments-20606c5180ab) . 

```java
public class NetworkFragment2 extends Fragment {
    private static final String TAG = "NetworkFragment";
    private static final String URL_KEY = "UrlKey";

    private @Nullable DownloadListener<String> downloadListener;
    private DownloadTask downloadTask;
    private String urlString;

    static NetworkFragment2 getInstance(FragmentManager fragmentManager, String url) {
        NetworkFragment2 networkFragment = new NetworkFragment2();
        Bundle args = new Bundle();
        args.putString(URL_KEY, url);
        networkFragment.setArguments(args);
        fragmentManager.beginTransaction().add(networkFragment, TAG).commit();
        return networkFragment;
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        if (getArguments() != null) {
            urlString = getArguments().getString(URL_KEY);
        }
    }

    @Override
    public void onAttach(@NonNull Context context) {
        super.onAttach(context);
        downloadListener = (DownloadListener<String>) context;
    }

    @Override
    public void onDetach() {
        super.onDetach();
        downloadListener = null;
    }

    @Override
    public void onDestroy() {
        cancelDownload();
        super.onDestroy();
    }
    void startDownload() {
        cancelDownload();
        downloadTask = new DownloadTask(downloadListener);
        downloadTask.execute(urlString);
    }
    void cancelDownload() {
        if (downloadTask != null) {
            downloadTask.cancel(true);
        }
    }

}

```

### The Activity class
finally the activity class. We simply create an instance of network fragment(which automatically attaches itself to our activity) and implement the downloadlistener callback.  

```java

public class MainActivity extends AppCompatActivity implements DownloadListener<String> {

    private NetworkFragment2 networkFragment;
    private boolean isDownloading = false;

    TextView tvText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //tvText = findViewById(R.id.tv_text);

        String api = "http://a.vdo.ai/core/ir24/ads_vmap.php";
        networkFragment = NetworkFragment2.getInstance(getSupportFragmentManager(), api);
        //NetworkFragment2.DownloadListener<String> listener = getNetWorkListener();
                //can't do above  thing. in this method, we are creating a fragment that
                // gets attached to our view, so our view must have those methods defined already,
                // and it will call those methods when needed. We will rather just press 
				//start here or in onStart and let fragment call them on its own

        startConnection();
    }
    private void startConnection() {
        if (!isDownloading && networkFragment != null) {
            // Execute the async download.
            networkFragment.startDownload();
            isDownloading = true;
        }
    }

    //-----------------------funcs called by fragment's callbacks-----------------------------------

    @Override
    public void updateFromDownloader(Result result) {
        updateUiFromResult(result);
    }
    private void updateUiFromResult(Result result) {
        if (result.exception != null) {
            tvText.setText("ErrorOccurred!" + result.exception.getMessage());
        } else if (result.resultValue != null) {
            tvText.setText(result.resultValue);

        }
    }

    @Override
    public NetworkInfo getActiveNetworkInfo() {
        ConnectivityManager connectivityManager =
                (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
        if (connectivityManager != null) {
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            return networkInfo;
        }
        return null;
    }

    @Override
    public void onProgressUpdate(int progressCode, int percentComplete) {
        updateUiWithDownloaderMetaData(progressCode, percentComplete);

    }
    private void updateUiWithDownloaderMetaData(int progressCode, int percentComplete) {
        switch (progressCode) {
            // You can add UI behavior for progress updates here.
            // its not being handled in fragment. its like we assumed there is a pipe with flowing
            // water and we attached our  nozel to it, but there is no one sending water to this
            // pipe. no code is sending updates , so these will never be called.
            case Progress.ERROR:
                tvText.append("\n==================================\nERROR_HAPPENED");
                break;
            case Progress.CONNECT_SUCCESS:
                tvText.append("\n==================================\nCONNECT_SUCCESS");

                break;
            case Progress.GET_INPUT_STREAM_SUCCESS:
                tvText.append("\n==================================\nGET_INPUT_STREAM_SUCCESS");
                break;
            case Progress.PROCESS_INPUT_STREAM_IN_PROGRESS:
                tvText.append("\n==================================\nPROCESS_INPUT_STREAM_IN_PROGRESS");
                break;
            case Progress.PROCESS_INPUT_STREAM_SUCCESS:
                tvText.append("\n==================================\nPROCESS_INPUT_STREAM_SUCCESS");
                break;
        }
    }

    @Override
    public void onDownloadingFinished(Result result) {
        closeConnection(result);
    }
    private void closeConnection(Result result) {
        isDownloading = false;
        if (networkFragment != null) {
            networkFragment.cancelDownload();
        }
    }

    //--------------------------------------------------------------------------------------------
    // not useful anymore function:
    private DownloadListener<String> getNetWorkListener() {
        return new DownloadListener<String>() {
            @Override
            public void updateFromDownloader(Result result) {
                updateUiFromResult(result);
            }

            @Override
            public NetworkInfo getActiveNetworkInfo() {
                ConnectivityManager connectivityManager =
                        (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
                if (connectivityManager != null) {
                    NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
                    return networkInfo;
                }
                return null;
            }

            @Override
            public void onProgressUpdate(int progressCode, int percentComplete) {
                updateUiWithDownloaderMetaData(progressCode, percentComplete);

            }

            @Override
            public void onDownloadingFinished(Result result) {
                closeConnection(result);

            }
        };
    }
}



``` 


---   
Well that's it.I usually have these all as just 2 classes instead of 4:  
```
- MainActivity implements...{...}
- NetWorkFragment{
	- Result{...}
	- DownloadListener{...}
	- DownloaderTask{...}
}
```

Next, We would be seeing how Simple it is to download a response via okhttp or  volley. its so small , that it would be covered in first quarter of article and later we would rather be covering more advance topics like conveting this reponse string to a list of objects, downloading images, etc.  
