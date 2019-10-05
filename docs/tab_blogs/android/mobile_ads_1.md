# The World of mobile ads (Part1)

*Disclaimer: The below article is very crude and mostly a collection of ideas and* 
			 *intutions. I might formalise and verify my guesses someday, but until then,*
			 *its just a collection of thoughts i felt during working with IMA Ads SDK and*
			 *some non verified internet comments*.

After working as an intern for a month, i can't tell you how much am excited to write 
about this stuff, its so cool! I can't write the whole article right now, but here are 
some parts of it.

- data is a trillion doller indusry and ads are the no. 1 source of data, and therefore 
  ads are the source of revenue for 80% of the businesses.

- But mobile industry has seen a particular downfall of Ads. people don't like clicking on
  ads now. the banner ads in android have been found out to be least clickable areas of an
  app in numerous studies . 

- Native ads are the future. Native ads are..`todo`.................

### How IMA works with a video player?


First we should talk about videos and video view in android . We know that in Android, 
a `VideoView` is a component that  plays video. how it plays videos? well there are 4 
types of video rendering, as i see them :  

- **Static Offline Video Rendering** : There is a video on user's device,the player simply 
  opens an `input stream` and starts rendering bitstream on a view

- **Static Online Video Rendering** : there is a video present on some server, the player
  downloads data in small chunks, stores them in cache, play them accordingly and then 
  clears cache.  
  It includes many type of fetching and caching techniques and these techniques are 
  applied to offline static video rendering too, for better results.(Although none of 
  those optimisation settings are provided by the VideoView in default)

- **Dynamic Offline Video Rendering(or video recording)** : video is being rendered as 
  soon  as input is received from a camera sensor.  
  This would not be technically called a video "rendering", because it is not controllable
  to start/pause/forward/reverse the playback. we just press the button to start 
  recording the rendered bits, i.e at the press of a button, the rendered bits will also
  start getting saved to a file.

- **Dynamic online video rendering(or video streaming)** : A camera app or software is  
  present somewhere in a  user's mobile that is uploading image bits at runtime to some 
  server, which passes it as response to millions of user devices in a fraction of second,
  and which these device render immediately . Most video streamer apps provide a rewind 
  option like youtube, but some do not, like instagram. 
 
There are a lot of components involved in there too : infact, the whole  "renderer" 
called `Media Player`  is a different thing which sends bitmaps to a view called 
`surface view` . other things are buffers and buffer sizes, adaptive downloading,
network loss handling, cache cleaning,etc.  i will cover these ina seperate article 
on video_view and exoplayer respectively.  

---

### Back to ads

So where were we? yeah, video ads .video Ads are rendered in a unique mechanism : the 
most famous video ad sdk is the ima sdk , which will work **alongside** your video player
to play ads like those you see on youtube:  

- startroll ads: small ads that plays before the content plays
- midroll ads  : small ads that play in between the content playback
- endroll ads  : small ads that play at the end of content playback.
- ad-list : group of small ads in which consequtive ads starts as soon as the previous ad 
  ended. can be start/mid/endroll 
- skippable/non skippable ads : long ads, but with/without a skip button, can be 
  startroll/midroll/enndroll/ad-list. 


The playing of an ad on paper looks simple : there is an advideo/ list of advideos, 
simply when the video reaches a particular time, pause the video, start playing ads & 
then resume the video. But here is when the real stuff begins:

#### Question 1
how can a video player be able to change its playing stream in between? suppose it was
previously playing a video for 60 seconds and then you wanna show an ad. How can you 
change its bitstream that is already loading(as i explained in the point about video 
players above)?  
  
#### Solution 

So that's either not possible or i couldn't find it, but what i found a solution to this 
was that Ads are actually displayed **over a video_view, not inside it** . yes!, the 
parent view of your video player view will becomea kind of video player itself and the 
surface would use to play ad videos, while your content video remains hidden and paused 
into background

#### Question 2

`<some question i had but forgot>`

#### Solution 

`<some answer i might but forgot>`


#### Question 3
How will streaming of ad work? Will we be loading ads just like we load content video? 

#### Solution.
Well, yes, and no. let's look at some ad based terms:

**VAST(Video Ad Serving Template)** TAG is standard response from any server with an ad 
video. Sending a req to this vast based server will send you an xml response with url to 
the advideo. And your video player can then simply start showing ads immediately or at a 
particular time instance of your choice(that is ofcourse if you have a client side code
for creating and rendering an ad above current player and fortunely most of it is done by 
ima sdk, you just need to implement their partially visible code for that)

**VMAP (Video Multiple Ad Playlist)** : are  list of vast ads but with even more 
information like at whatt time each vast ad/ vast ad-group will run. A VMAP based server 
will also return an xml having urls of vast tag along with timestamps about which url to 
load ad from and at what time. This is also handled by ima sdk and its extensions and we 
just need to integrate it


**VPAID** ads are based on a small lie of mine: a VAST ad does not *always necessarily* 
contain video urls but sometimes a js function called vpaid is also present. in web dev, 
this function is suppose to run directly on the clients machine and shows ads but it does
not seem to work in mobile version of the ima sdk.  

```
So let me come to the topic that should be a little above current content: what are 
adevrtisements? what is google ads? why google has so many ad services ? what is the 
category of people that interact with what part of each service? what is IMA? what part 
from the company that wants to show ads to a particular app to the company that actually 
makes those ads to the rendering handling components to the aapps/websites that will be 
showing ads to the users wathing and clicking ads to the trackers to everyone , 
who benefit from this.  
```

So here , IMA sdk for android fails in mysterious ways:  

- <B>"ima sdk <U>*exoplayer-extension*</U> for android"</B> fails because it is very limited 
  and black boxed. It only works with vmap, handles all the stuff on its own and not even
  sends any callbacks.(As per my knowledge) 
  
- <B>"ima sdk <U>(library)</U> for android"</B> is  a little
  flexible, allowing both vast tags and handling of vmap but it still won't allow vpaid 
  ads. these vpaid ads are officially not supported for android sdk, but supported for 
  html 5 sdk. plus we loose all those optimisation features of exoplayer if that's 
  acceptable.