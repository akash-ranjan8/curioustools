# Exoplayer

17/9/19  


A good video player library for implementing A decent video player in your app, coz the other option of video_view from the default android sdk is less useful

## Steps to implement

1. Add Exoplayer library : Which is actually a group of 6-7 individual libraries having a jillion files.
```
    implementation 'com.google.android.exoplayer:exoplayer:2.10.4'

```
adding this would add fucking [800 classes](exoplayer_classes.md) of this massive library. So let us see what it has to offer.  
  
**Some basic Details.**  
The library has the following modules. Adding a dependency to the full ExoPlayer library is equivalent to adding dependencies on all of the library modules individually:  

- exoplayer-core: Core functionality (required).
- exoplayer-dash: Support for DASH content.
- exoplayer-hls: Support for HLS content.
- exoplayer-smoothstreaming: Support for SmoothStreaming content.
- exoplayer-ui: UI components and resources for use with ExoPlayer.    

In addition to library modules, ExoPlayer has multiple extension modules that depend on external libraries to provide additional functionality. Browse the extensions directory(https://github.com/google/ExoPlayer/tree/release-v2/extensions/) and their individual READMEs for details.
Also java 8 support is compulsory for this library. 

## the Bare Minimmums.

The smallest code that we need to 
```java

     protected void onCreate(Bundle savedInstanceState) {
        ...

        Context ctx =this;
        String CONTENT_URL = "https://www.radiantmediaplayer.com/media/bbb-360p.mp4";
        int playerID=R.id.pv_main;
        int appNameStringRes = R.string.app_name;
        startPlayingVideo(this,CONTENT_URL,playerID,appNameStringRes);


    }

    //
    private void startPlayingVideo(Context ctx , String CONTENT_URL, int playerID, String appNameRes) {

        PlayerView pvMain = ctx.findViewById(playerID);
        
        //BandwidthMeter bandwidthMeter = new DefaultBandwidthMeter();
        //TrackSelection.Factory videoTrackSelectionFactory = new AdaptiveTrackSelection.Factory(bandwidthMeter);        
        //TrackSelector trackSelectorDef = new DefaultTrackSelector(videoTrackSelectionFactory);
        TrackSelector trackSelectorDef = new DefaultTrackSelector();

        SimpleExoPlayer absPlayerInternal = ExoPlayerFactory.newSimpleInstance(ctx, trackSelectorDef);
        
        String userAgent = Util.getUserAgent(ctx, ctx.getString(appNameRes));
        
        DefaultDataSourceFactory defdataSourceFactory = new DefaultDataSourceFactory(ctx,userAgent);
        Uri uriOfContentUrl = Uri.parse(CONTENT_URL);
        MediaSource mediaSource = new ProgressiveMediaSource.Factory(defdataSourceFactory).createMediaSource(uriOfContentUrl);
        
        absPlayerInternal.prepare(mediaSource);
        absPlayerInternal.setPlayWhenReady(true);
        
        pvMain.setPlayer(absPlayerInternal);

    }

    private void stopPlayer(PlayerView pv,SimpleExoPlayer absPlayer){
        pv.setPlayer(null);
        absPlayer.release();
        absPlayer = null;
    }
```
How this works?  
- We add a `PlayerView` widget in our xml and refer its instance in java. 
- Then we attach an instance of `SimpleExoPlayer` to it , which will handle all downloading, decoding and rendering of video.  

**In More Details, but with a tiny off topic context:**   

Suppose I am the creator of baby shark video song. I have made this large 48 mb sized HD, bluray video and uploaded it to my fast, responsive,hypothetical aws server.  
I want the an app which is able to fetch this video into user's mobile without downloading it on their mobiles.  

This is what I call **static streaming** . ***The video player is supposed to download some small chunks(Say, 2-12 second of a video at a time) of video one by one, at real time and show it to the users on the spot.***  

**But** The biggest problem with streaming is connectivity. Their are mobile users with super fast internet speeds, users with fluctuating speeds and users with terribly ugly internet speeds. Even though a 6 sec HD video would be just 700-800 KB data, it might take 4-5 seconds for a user with poor connection.  

So exo player has this `TrackSelector` class, that is  responsible for **choosing which file chunks to download based on perceived bandwidth changes.**  
So, for eg, if a high quality chunk would take too long to download, the TrackSelector will choose something of lower quality to show the user. And since each chunk is usually about 2 to 12 seconds long, it’s not  bad if the user gets a lower quality chunk of media once in while for the benefit of a smooth experience.
It Uses `BandwidthMeter` class to get user's current bandwidth speeds  and a bunch of  other classes nd factories to create different tracks of video stream chuncks or packets of different sizes and qualities.
(Later we will expand more on this: what are tracks, how tracks are created, track selection and creators, How it interacts with `absPlayerInstance` etc )

> <I><B><U>So, to summarise, TrackSelector basically makes a setting in our exoplayer environment to download either  high quality HD packets(i.e a complete packet) or low quality 144p or 360p packets(or optimised/compressed/low quality version of a packet) from a stream usually on the basis of user's Internet Connection Bandwidth and/or other factors</B></U></I>  

---  

Using this track selector we can create an almost ready to run ExoPlayer instance. Let's look into **`SimpleExoPlayer`**.  

At the core of the ExoPlayer library is the **`Player`** interface. This is the most powerful interface as it has definations for functions like:  

- `isPlayingAd()`: weather the player is playing an ad or not.  
- `getCurrentAdGroupIndex()` : index of current ad group(if ad is being played)
- `getCurrentAdIndexInAdGroup()` : index of current ad in current ad group(if ad is being played)  
- `getContentDuration()` : returns either the total duration of contents(+ the ads) or `C.TIME_UNSET` (a constant indicating remaining time is unknown) 
- `getContentPosition()` : position of content from where it will be resumed playing after ads are finished. (if ads aren't playing, will simply return same as `getPosition()`)  
-`getContentBufferedPosition()` : returns an estimate of the content position in the current content window up to which data is buffered, in milliseconds.(if ads aren't playing, will simply return same as `getBufferedPosition()`)  
- get/setVolume()
- getAudioComponent(),getVideoComponent(),MetaDataComponent() // not exactly what you are thinking
- add/remove audio/videoListener
- addEventListener(),addErrorListener
- setPlayWhenRead()/ setRepeatMode()/ seekTo()/ stop()/reset()/ isLoading()/release()
- getCurrentTrackGroups/getTrackSelections()/geTCurrentManifest()
- setPlayBackParams() //like speed/pith  

Instances can be created through ExoPlayerFactory class.  SimpleExoPlayer instance is a class which implements Player functions. ExoPlayer exposes traditional high-level media player functionality such as getAudioFormat, getCurrentPositon, getDuration, setVolume, seekTo, setPlayWhenReady, stop and much more. Just like I have mentioned before ExoPlayer uses many components to achieve its modularity and customizability. It delegates work to components which have to be injected when we create and prepare an ExoPlayer instance.  

The next part is specifying a **MediaSource**. We have assumed that their will be a video on some server that will be continuously sending us data some large chunks of data packages and our track selector and other libs will download small chuncks of it based on user bandwidth.  

But there is something missing, something incomplete. How is this continuous stream coming?Where is it coming from? why is it coming automatically and continuously?When and how did it started coming? How is it forever running?Why isn't data getting wasted if packet downloading speed is so fast while streaming speed is slow?Can I listen to the packets being received/decoded/...(whatever are the stages before player starts to render it and user sees it)...?   

These are good questions that will be obviously answered. But let's first work on our assumptions.
1. Our first assumption is very correct: There **IS** some server that has our video loaded in it. So our first task would be to inform our app's video handling environment, i.e the exoplayer about it


 A {@link Factory} that produces {@link DefaultDataSource} instances that delegate to
 * {@link DefaultHttpDataSource}s for non-file/asset/content URIs.

 Your choice of MediaSource is going to depend on the type of media you're trying to play.
In this example, we're going to be playing an MP4, so we're going to use an `ProgressiveMediaSource` which supports formats  like MP4, MP3, Matroska, and so on.  
If you wanted to play Dash or HLS or smooth streaming streams, then you would use the corresponding MediaSources for those different formats.  

---  


## Extras  

- Why exoplayer does not support progress listener: https://github.com/google/ExoPlayer/issues/2090 
>> logically it's the client that knows the points at which it wants a position update  
>  
> What if the client just wants to listen to the video current position updates? Constantly checking getCurrentPosition (e.g. with the help of Timer().scheduleAtFixedRate) is a very inefficient solution. IMHO, it would be great if ExoPlayer had a specialized method for this, somethings like setCurrentPositionListener which the developers could use to get notified about the video position changes.  
> 
> Since the player manages its own getCurrentPosition property it already knows when to emit onPositionChanged events. The client can only use the polling technique for this.  


```java
/**
// TrackSelector class
 * The component of an {@link ExoPlayer} responsible for selecting tracks to be consumed by each of
 * the player's {@link Renderer}s. The {@link DefaultTrackSelector} implementation should be
 * suitable for most use cases.
 *
 * <h3>Interactions with the player</h3>
 *
 * The following interactions occur between the player and its track selector during playback.
 *
 * <p>
 *
 * <ul>
 *   <li>When the player is created it will initialize the track selector by calling {@link
 *       #init(InvalidationListener, BandwidthMeter)}.
 *   <li>When the player needs to make a track selection it will call {@link
 *       #selectTracks(RendererCapabilities[], TrackGroupArray, MediaPeriodId, Timeline)}. This
 *       typically occurs at the start of playback, when the player starts to buffer a new period of
 *       the media being played, and when the track selector invalidates its previous selections.
 *   <li>The player may perform a track selection well in advance of the selected tracks becoming
 *       active, where active is defined to mean that the renderers are actually consuming media
 *       corresponding to the selection that was made. For example when playing media containing
 *       multiple periods, the track selection for a period is made when the player starts to buffer
 *       that period. Hence if the player's buffering policy is to maintain a 30 second buffer, the
 *       selection will occur approximately 30 seconds in advance of it becoming active. In fact the
 *       selection may never become active, for example if the user seeks to some other period of
 *       the media during the 30 second gap. The player indicates to the track selector when a
 *       selection it has previously made becomes active by calling {@link
 *       #onSelectionActivated(Object)}.
 *   <li>If the track selector wishes to indicate to the player that selections it has previously
 *       made are invalid, it can do so by calling {@link
 *       InvalidationListener#onTrackSelectionsInvalidated()} on the {@link InvalidationListener}
 *       that was passed to {@link #init(InvalidationListener, BandwidthMeter)}. A track selector
 *       may wish to do this if its configuration has changed, for example if it now wishes to
 *       prefer audio tracks in a particular language. This will trigger the player to make new
 *       track selections. Note that the player will have to re-buffer in the case that the new
 *       track selection for the currently playing period differs from the one that was invalidated.
 * </ul>
 *
 * <h3>Renderer configuration</h3>
 *
 * The {@link TrackSelectorResult} returned by {@link #selectTracks(RendererCapabilities[],
 * TrackGroupArray, MediaPeriodId, Timeline)} contains not only {@link TrackSelection}s for each
 * renderer, but also {@link RendererConfiguration}s defining configuration parameters that the
 * renderers should apply when consuming the corresponding media. Whilst it may seem counter-
 * intuitive for a track selector to also specify renderer configuration information, in practice
 * the two are tightly bound together. It may only be possible to play a certain combination tracks
 * if the renderers are configured in a particular way. Equally, it may only be possible to
 * configure renderers in a particular way if certain tracks are selected. Hence it makes sense to
 * determined the track selection and corresponding renderer configurations in a single step.
 *
 * <h3>Threading model</h3>
 *
 * All calls made by the player into the track selector are on the player's internal playback
 * thread. The track selector may call {@link InvalidationListener#onTrackSelectionsInvalidated()}
 * from any thread.
 */
public abstract class TrackSelector {
}
```  

**ProgressiveMediaSource(a class)** extends **BaseMediaSource(a class)** extends **Media Source(an interface)**

---  

```java

/**
 * Provides one period that loads data from a {@link Uri} and extracted using an {@link Extractor}.
 *
 * <p>If the possible input stream container formats are known, pass a factory that instantiates
 * extractors for them to the constructor. Otherwise, pass a {@link DefaultExtractorsFactory} to use
 * the default extractors. When reading a new stream, the first {@link Extractor} in the array of
 * extractors created by the factory that returns {@code true} from {@link Extractor#sniff} will be
 * used to extract samples from the input stream.
 *
 * <p>Note that the built-in extractor for FLV streams does not support seeking.
 */
public final class ProgressiveMediaSource extends BaseMediaSource
    implements ProgressiveMediaPeriod.Listener {


  /* Factory for creating instances of ProgressiveMediaSource class, using the extractors provided by {DefaultExtractorsFactory}  (OR something else too, idk). */
  public static final class Factory implements AdsMediaSource.MediaSourceFactory {
      ...
      ...
    
    public Factory(DataSource.Factory dataSourceFactory) {
      this(dataSourceFactory, new DefaultExtractorsFactory());
    }
....
}    

```  

```java

/**
 * Base {@link MediaSource} implementation to handle parallel reuse and to keep a list of {@link
 * MediaSourceEventListener}s.
 *
 * <p>Whenever an implementing subclass needs to provide a new timeline and/or manifest, it must
 * call {@link #refreshSourceInfo(Timeline, Object)} to notify all listeners.
 */
public abstract class BaseMediaSource implements MediaSource {

  private final ArrayList<SourceInfoRefreshListener> sourceInfoListeners;
  private final MediaSourceEventListener.EventDispatcher eventDispatcher;

  @Nullable private Looper looper;
  @Nullable private Timeline timeline;
  @Nullable private Object manifest;

  public BaseMediaSource() {
    sourceInfoListeners = new ArrayList<>(/* initialCapacity= */ 1);
    eventDispatcher = new MediaSourceEventListener.EventDispatcher();
  }


```



```java

/**
 * Defines and provides media to be played by an {@link com.google.android.exoplayer2.ExoPlayer}. A
 * MediaSource has two main responsibilities:
 *
 * <ul>
 *   <li>To provide the player with a {@link Timeline} defining the structure of its media, and to
 *       provide a new timeline whenever the structure of the media changes. The MediaSource
 *       provides these timelines by calling {@link SourceInfoRefreshListener#onSourceInfoRefreshed}
 *       on the {@link SourceInfoRefreshListener}s passed to {@link
 *       #prepareSource(SourceInfoRefreshListener, TransferListener)}.
 *   <li>To provide {@link MediaPeriod} instances for the periods in its timeline. MediaPeriods are
 *       obtained by calling {@link #createPeriod(MediaPeriodId, Allocator, long)}, and provide a
 *       way for the player to load and read the media.
 * </ul>
 *
 * All methods are called on the player's internal playback thread, as described in the {@link
 * com.google.android.exoplayer2.ExoPlayer} Javadoc. They should not be called directly from
 * application code. Instances can be re-used, but only for one {@link
 * com.google.android.exoplayer2.ExoPlayer} instance simultaneously.
 */
public interface MediaSource {

  /** Listener for source events. */
  interface SourceInfoRefreshListener {

    /**
...
} 
```


### exo resources :  

1. https://android.jlelse.eu/streaming-video-on-android-using-exoplayer-3087d604095 This guy implements basic streaming but has some content that i haven't yet read.
2. https://medium.com/@pawankgupta.se/live-video-streaming-using-exoplayer-2-x-41cc7f9301d6 basic video rendering using HLS  
3. http://www.tothenew.com/blog/playing-different-videos-with-exo-player-in-android-tv/  basic exoplayer. might have something new.  
4. https://blog.hotstar.com/when-hotstar-met-exoplayer-5b9ea500bd0 : good theory regarding exo
5. https://github.com/Tubitv/TubiPlayer : tubi player : a player build around exoplayer

### non exo player resources  

Good article on how video straming woks :  
  1. https://www.swipetips.com/android-video-streaming-videoview-tutorial/ video view streaming
  1. https://instagram-engineering.com/improving-video-playback-on-android-2f6c6a0058d
  2. https://medium.com/@onix_systems/rtmp-protocol-enable-instant-video-streaming-for-android-apps-44cd9de6f339  
  3. http://www.tothenew.com/blog/adaptive-video-streaming-hls/ very good theory
  3. https://medium.com/p/cddc0b1bf764/responses/show (bad , promotional content, but good theory) 
  4. https://medium.com/@judeosby/creating-a-video-streaming-app-in-android-using-mvvm-rxjava-pagination-library-and-the-normal-e7b120653d19  (good, practical one with github/open source use cases uses deprecated bvp player : https://github.com/halilozercan/BetterVideoPlayer)  
  
  5. https://www.streamingmedia.com/Default.aspx website on streaming.
  6. https://imagen.io/resources/what-are-svod-tvod-avod/ : VOD business  
  7. jw player : they have implemented everything we want.
  