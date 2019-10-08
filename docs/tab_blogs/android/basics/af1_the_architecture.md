# Android Fundamentals : Part 1 ,The architecture


`# Android Fundamentals : Part 2 ,The application components`


So Before getting into advance stuff, i need to get my mind off the basics . These were done by me so long ago, that i have forgotten most of the theory part of it and have got the practical part of it as unconsious reflexes reflex.

We write apps in java,kotlin,dart,..etc in many languages.  
But before even writing, when we create a new project, a lot of stuff gets automatically generated.  
 
```
So what are these things ? Why are they created? which of those files are necessary? 
What is their use? why I get an Appcompat- extended Activity class when beginning an app?
 Why so many xml folders ? what is gradle? How are they all combining to make an apk that 
 file? how is this apk being run by my system? how are these apk apps able to interact 
 with my hardware, like camera and stuff? How do android maintains contacts, sms , media
 files and other stuff that can be viewed by so many apps? where is the security in it? 
 
 Whe-
```

phew! Take a break mr curious! They are a hell lot of questions which we will be covering in detail. Let's try covering them part by part.  
 
--- 

## Architecture. 

If you ask me How a mobile works, then the most crude defination will simply be "Power is connected to hardware, firmwares and switches. when pressed a hardware button, the battery starts giving power to firmware having a software bootloader. The bootloader loads the software and then the software controls everything" (*That's pretty much defination of every OS based device system*).  

So, aside from a little stuff about hardware, bootloader, firmware and circuits, the Software is pretty much the main thing. And that's what Android is: the OS for mobiles.  
To know more about How this single thing , "Android OS" is able to handle everything so gracefully, We have to first learn about its architecture.  

A simple analogy will be that of a human: "How Newton is able to think about gravity?". Well, his heart pumbed blood to his brain and eyes, his eyes saw apple falling,his nerves send that stimulus to brain, his brain proceeded ..."and that's how he thought of gravity" . Even though this is not the right answer, it is technically correct: this was the actual procedure on how any human would think. Te correct answer would be "He was a skilled man with a great ackquired knowledge, he got tinto a correct interesting environment and automatically got an idea" .  

This is so much like an app! You are not required to understand a deep architecture of OS to make an app, infact you could just have a comfortable environment setup completely by android studio, build everything by modules , generators and libraries and you just pressing the build button. But of you do, you might make a better more optimised app.    

So let’s start with this only,
**What is an architecture?** 
(Note: This entire article(after the tldr) below is copied from  [deepam goel's amazing article](https://medium.com/@deepamgoel/understanding-android-architecture-1f0fb4b52f90) , which i cannot explain any better.  
) 

So any software or system is based on some set of components that work together to accomplish some certain task or to perform some specific function. This is a vast topic and without discussing much about it lets see what components Android is made up of and how they all work together to make things happen.  

**What are Android OS Components?**  

Android OS is based on the stack of 5 main components, viz:  
1. System Applications
2. Application Framework
3. Libraries
4. Android Runtime
5. Hardware Abstraction Layer (HAL)
6. Linux Kernel  

![Imgur](https://imgur.com/8aKYZ9d.png)

### Application  
([more info](https://developer.android.com/guide/platform/#system-apps) )      
These are the application that we all make and use. They reside on the topmost layer of Android Architecture.

### Application Framework  
([more info](https://developer.android.com/guide/platform/#api-framework) )      

The Application Framework layer provides many higher-level services to applications in the form of Java classes and also contains other APIs. Application developers are allowed to make use of these services in their applications.

### Libraries  
([more info](https://developer.android.com/guide/platform/#native-libs) )    

Above Linux kernel, there is a set of libraries including open-source Web browser engine WebKit, well-known library libc, SQLite database, libraries to play and record audio and video, SSL libraries responsible for Internet security etc. All Java based libraries that are needed for building Android app also resides here.

### Android Runtime  
([more info](https://source.android.com/devices/tech/dalvik/) )  
This portion contains one of the most important components Dalvik Virtual Machine, DVM. This is a kind of JVM that is specially optimized and made for Android. It enables every android app to run its own process which enables us to run many applications at same time. It basically converts .java file into .Dex format.

### Hardware Abstraction Layer (HAL)  
( [more info](https://source.android.com/devices/architecture/hal) ) 

A HAL defines a standard interface for hardware vendors to implement, which enables Android to be agnostic about lower-level driver implementations. Using a HAL allows you to implement functionality without affecting or modifying the higher level system. HAL implementations are packaged into modules and loaded by the Android system at the appropriate time.

### Linux Kernel  
([more info](https://source.android.com/devices/architecture/kernel/) )  

Kernel is core of any OS. The Linux Kernel sits at the bottom of Android architecture. The Android platform is built on top of the Linux 2.6 Kernel. The Linux Kernel provides support for many features like memory management, security management, process management, and device management etc. It also contains all the driver that helps Android device to communicate with other hardware devices.  

**Note**  

DVM was officially replaced by Android Run Time, ART in Lollipop version. It has many advantages over DVM. To know more about ART, Check this [post](
https://stackoverflow.com/questions/31957568/what-is-difference-between-dvm-and-art-why-dvm-has-been-officially-replaced-wi) .

**Other references**
- [Ref1](http://www.linux-india.org/characteristics-and-architecture-of-linux-oprating-system/ ) : good look at linux architecture on which android architecture is based upon.  

- [Ref2](https://androidclarified.com/android-architecture/) : good link with similar content but in more detail(i also downloaded a pdf of it with name `[Android] android_architecture.pdf`)  

- [Ref3](https://www.youtube.com/watch?v=QBGfUs9mQYY) : same content in a video.  


- [Ref4](https://source.android.com/devices/architecture/hal-types ): HAL, [Ref5](http://source.android.com/devices/tech/dalvik/index.html) : ART  

- [Ref6](https://developer.android.com/guide/platform) General info about android(added this link because it has got specific more info on other aspects of android like apk, bundles, system updates, etc)  
 
- [Ref7]( https://www.youtube.com/watch?v=3Lelk1ZyyQM ): video on android Security  

- [Ref8]( https://www.youtube.com/watch?v=o8NPllzkFhE ): Linus torvalds on linux




