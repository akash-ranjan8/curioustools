# Android Fundamentals : Parts of an app

In part 1 we got atleast 1 thing figured out: Android OS is a distinctive, layered, linux based OS in which multiple layers are involved to perform an action.

As we may remember, in the topmost layer resides softwares called Apps, System Apps and UserApps. These are super busy chunks of code, as they need to provide an interface of communication among  user and internal components. They are executable with an extension as `.apk` and each Android app lives in its own security sandbox, protected by  Android security features like:
	- The Android operating system is a multi-user Linux system in which each app is a different user.
	- By default, the system assigns each app a unique Linux user ID (the ID is used only by the system and is unknown to the app). The system sets permissions for all the files in an app so that only the user ID assigned to that app can access them.
	- Each process has its own virtual machine (VM), so an app's code runs in isolation from other apps.
	- By default, every app runs in its own Linux process. The Android system starts the process when any of the app's components need to be executed, and then shuts down the process when it's no longer needed or when the system must recover memory for other apps.

The Android system implements the principle of least privilege. That is, each app, by default, has access only to the components that it requires to do its work and no more. This creates a very secure environment in which an app cannot access parts of the system for which it is not given permission. However, there are ways for an app to share data with other apps and for an app to access system services:

	- It's possible to arrange for two apps to share the same Linux user ID, in which case they are able to access each other's files. To conserve system resources, apps with the same user ID can also arrange to run in the same Linux process and share the same VM. The apps must also be signed with the same certificate.
	- An app can request permission to access device data such as the device's location, camera, and Bluetooth connection. The user has to explicitly grant these permissions