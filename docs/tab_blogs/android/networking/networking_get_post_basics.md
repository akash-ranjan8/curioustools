# Networking Basics : Making a Get Post Connection(Theory)
Networking would refer to the use of internet inside an app. There could be a million and 1 way to be using internet in an app: your app could be a chat app for sending/receiving messages , reddit like social media app, instagram like pictures app, or even a feed app like google news app.
During network transmissions,  there are many things that need to be kept in mind : paging, caching, asynchronous loading and transmission,  realtime ui updates,etc  

So let’s start from the very basics.


## 1.	Adding permissions.
We have to add the following permission to allow our app to receive and send data via a network connection:

```html
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
The second permission is to check weather the user’s device is connected to a network or not. Since both permissions are safe and not dangerous, we do not need to provide a runtime check

## 2. Checking the connection.
Here comes the code part. We have to first check weather our system is connected to the internet a Network. For this, we use the following piece of code:
```java
private boolean isConnectedToMobileDataOrWify(){ 
     ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
     NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();

     return
              networkInfo != null &&
              networkInfo.isConnected() &&
                        (networkInfo.getType() == ConnectivityManager.TYPE_WIFI || networkInfo.getType() == ConnectivityManager.TYPE_MOBILE);
    }
```
( We would be later using A decoupled asynctask, therefore it would be eventually broken and used in a different manner.)

Note that this specifically refers to a network type of Wify or Mobile. There DOES exist other types of network, specifically:  

| 			Type,value  			| 				type,value				|
| --------------------------------- | ------------------------------------- |
|	(psfi) TYPE_NONE = -1;			|	(psfi) TYPE_MOBILE = 0;				|
|	(psfi) TYPE_WIFI = 1;			|	(psfi) TYPE_MOBILE_MMS = 2;			|
|	(psfi) TYPE_MOBILE_SUPL = 3;	|	(psfi) TYPE_MOBILE_DUN = 4;			|
|	(psfi) TYPE_MOBILE_HIPRI = 5;	|	(psfi) TYPE_WIMAX = 6;				|
|	(psfi) TYPE_BLUETOOTH = 7;		|	(psfi) TYPE_DUMMY = 8;				|
|	(psfi) TYPE_MOBILE_FOTA = 10;	|	(psfi) TYPE_MOBILE_IMS = 11;		|
|	(psfi) TYPE_MOBILE_CBS = 12;	|	(psfi) TYPE_WIFI_P2P = 13;			|
|	(psfi) TYPE_MOBILE_IA = 14;		|	(psfi) TYPE_MOBILE_EMERGENCY = 15;	|
|	(psfi) TYPE_PROXY = 16;			|	(psfi) TYPE_VPN = 17;				|
|	(psfi) TYPE_ETHERNET = 9;		|										|  

*Update: usage of these integers is deprecated. Now, `networkCapabilitiesObj.hasTransport(NetworkCapabilities.TRANSPORT_WIFI)`;Or `connectivityManager.requestNetwork(networkRequestObj,networkCallbackObj)` is used.*

##  3.	Making a connection(Theory)  

Next we would be establishing a connection. Before we establish a connection, one thing should be pretty clear:  

!!! danger
    **All the network fetching / uploading / other network related tasks should be performed on a separate thread.**  


The ui operations would obviously be performed on the ui thread , therefore we use a **Threadsafe Asynctask**( <u> An Asynctask created in a headless fragment along with callbacks to prevent leaking</u> ) which could perform a network task and safely update ui for task progress and later, task completion.  


There are some other concepts in regard to a network task:  

**Http Connection**: HttpURLConnection is the official Android Api
  for creating an http connection In android.According to
  geeksforgeeks, The “Hypertext Transfer Protocol (HTTP) is an
  application-level protocol that uses TCP as an underlying
  transport and typically runs on port 80. HTTP is a stateless 
  protocol i.e. server maintains no information about past client 
  requests.”   
  
  This [hidden link]( https://developer.android.com/reference/java/net/HttpURLConnection) has 
  a lot of info regarding different functions and features of
  HttpConnection class.  
  
  
**Get and Post()**: something about security, blah blah blah.
Would look into networking someday. Not important in android 
development.  

From what I know from my app dev experience in claro, both `get()`
and `post()` request is like calling a server and demanding a
response. Our calling method would contain a custom query(like 
google.com?q=animal where “animal” is a search result we want). Both 
methods  will give us a json or XML response against the query .  
- In `get()` ,the request to server itself contains queries like  

`https://api.stackexchange.com/2.2/questions?pagesize=5&order=desc&sort=activity&site=stackoverflow"`  

- In `post()`, we have to pass queries alongside the request to 
server in a json string. This json string is hidden inside a
`requestBody` obj. libraries like **okhttp** help create and 
pass  these json string alongside request in a very simple way,
like `request.putString(“pagesize”,”5”)` or 
`putString(“email”,“ansh@example.com”)`. in native http way, we have 
to perform a lot of steps for such thing , like adding it to 
outputstream’s buffer, creating aurl encoded string, etc.  
	
This reason for this distinguishment is most probably the security 
issues. I guess the code behind checking the `get` request is either 
very small or none at all. But in case of `post` request there might 
be some encoding decoding algorithms involved. If you ask a web dev, 
they would simply say "get me to url me hi sbkuch pass hota hai, to 
isse passwords leak hone ka khatra hai but post chhipa rehega to no 
khatra".  


## 4. Making a Connection (Code)(to be continued)

The above was the the set of general and commone things that are needed in making a get/post connection.  Next we will be seeing the code for it. Now before heading to code, there is a thing to handle threads. Now, threads cause a lot of memory and context leaks which needs to be handled. So we will be seeing the native code for doing this(which handles problem of thread leakage using Threadsafe asynctasks enclosed in Fragments ) and then we will be seeing Libraries made for performing get/post requests which i don't know how or weather they are handling these or not.
