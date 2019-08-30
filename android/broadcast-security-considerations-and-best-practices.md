# BROADCAST: Security considerations and best practices

Here are some security considerations and best practices for sending and receiving broadcasts:

* If you don't need to send broadcasts to components outside of your app, then send and receive local broadcasts with the [LocalBroadcastManager](https://developer.android.com/reference/androidx/localbroadcastmanager/content/LocalBroadcastManager.html) which is available in the `Support Library`. The `LocalBroadcastManager` is much more efficient (no interprocess communication needed) and allows you to avoid thinking about any security issues related to other apps being able to receive or send your broadcasts. Local Broadcasts can be used as a general purpose pub/sub event bus in your app without any overheads of system wide broadcasts.

* If many apps have registered to receive the same broadcast in their manifest, it can cause the system to launch a lot of apps, causing a substantial impact on both device performance and user experience. To avoid this, prefer using context registration over manifest declaration. Sometimes, the Android system itself enforces the use of context-registered receivers. For example, the `CONNECTIVITY_ACTION` broadcast is delivered only to context-registered receivers.

* Do not broadcast sensitive information using an implicit intent. The information can be read by any app that registers to receive the broadcast. There are three ways to control who can receive your broadcasts:

    * You can specify a permission when sending a broadcast.
    
    * In Android 4.0 and higher, you can specify a `package` with [setPackage(String)](https://developer.android.com/reference/android/content/Intent.html#setPackage(java.lang.String)) when sending a broadcast. The system restricts the broadcast to the set of apps that match the package.
    
    * You can send local broadcasts with `LocalBroadcastManager`.

* When you register a receiver, any app can send potentially malicious broadcasts to your app's receiver. There are three ways to limit the broadcasts that your app receives:

    * You can specify a permission when registering a broadcast receiver.
    
    * For manifest-declared receivers, you can set the `android:exported` attribute to "false" in the manifest. The receiver does not receive broadcasts from sources outside of the app.
    
    * You can limit yourself to only local broadcasts with `LocalBroadcastManager`.

* The namespace for broadcast actions is global. Make sure that action names and other strings are written in a namespace you own, or else you may inadvertently conflict with other apps.

* Because a receiver's `onReceive(Context, Intent)` method runs on the main thread, it should execute and return quickly. If you need to perform long running work, be careful about spawning threads or starting background services because the system can kill the entire process after onReceive() returns. For more information, see Effect on process state To perform long running work, we recommend:

    * Calling **`goAsync()`** in your receiver's onReceive() method and passing the `BroadcastReceiver.PendingResult` to a background thread. This keeps the broadcast active after returning from onReceive(). However, even with this approach the system expects you to finish with the broadcast very quickly (under 10 seconds). It does allow you to move work to another thread to avoid glitching the main thread.
    
    * Scheduling a job with the **`JobScheduler`**. For more information, see [Intelligent Job Scheduling](https://developer.android.com/topic/performance/scheduling.html).

* Do not start activities from broadcast receivers because the user experience is jarring; especially if there is more than one receiver. Instead, consider displaying a `notification`.

## Reference

* https://developer.android.com/guide/components/broadcasts.html#security-and-best-practices