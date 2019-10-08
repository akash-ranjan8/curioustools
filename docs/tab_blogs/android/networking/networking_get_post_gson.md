# Networking Basics : Making a Get Post Connection(gson)

This article is not actually about downloading a response, but rather how to convert it into meaningful form 
In the previous articles, we saw how data can be downloaded as a response string . When we say "reponse string", we mean something like this:  

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

To make it useful, we must convert it into some form of object or object list via a parser. Here is when 
GSON comes useful. Now before diving straight into gson, let's see how we would have done in native code:  

```java  

public class  RvAdapterData{
	String questionText,url;
	public RvAdapterData(String questionText, String url) {  /*..*/} /*..*/    
}
private ArrayList<RvAdapterData> convertJSONToAdapterDataList(String responseString) {
	ArrayList<RvAdapterData> result= new ArrayList();
	try {
		JSONObject root = new JSONObject(responseString);
		JSONArray itemsArr= root.getJSONArray("Items");

		for (int i=0;i<itemsArr.length();i++){
			JSONObject item= itemsArr.getJSONObject(i);
			String questionText= item.getString("title");
			String url= item.getString("link");
			
			RvAdapterData data = new RvAdapterData(questionText, url);
			result.add(data);
		}
		String serverResponse = root.getString("val");
	}
	catch (JSONException e) {
		e.printStackTrace();
	}
	return  result;
}
```  

There could be a chance that you might mistake in passing the correct keys in `getJsonObject()` or `getString()`. I personally use [http://www.jsonschema2pojo.org](http://www.jsonschema2pojo.org) to first convert my json to a set of java files and then write the above code. I do not like GSON library, it so easy to use that site, we could get a pojo in just a few checkboxes clicked(although, be very careful with that, the pojo is not generated if some checks are wrong)  

You can check more about  How GSON simplifies the process of json parsing here: [https://guides.codepath.com/android/leveraging-the-gson-library ](https://guides.codepath.com/android/leveraging-the-gson-library )
