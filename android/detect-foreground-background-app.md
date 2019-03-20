# Detecting when an Android app is in foreground or background

### Creating the LifeCycleObserver class

It provides lifecycle for the whole application process.

```java
public class AppLifecycleObserver implements LifecycleObserver {

    public static final String TAG = AppLifecycleObserver.class.getName();

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    public void onEnterForeground() {
        //run the code we need
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    public void onEnterBackground() {
        //run the code we need
    }

}
```

### Integrating the Observer within the App

Register the LifeCycleObserver class within the app in onCreate() method of Application class

```java
public class MyApp extends MultiDexApplication {

    private static final String TAG = MyApp.class.getName();

    @Override
    public void onCreate() {
        super.onCreate();

        AppLifecycleObserver appLifecycleObserver = new AppLifecycleObserver();
        ProcessLifecycleOwner.get().getLifecycle().addObserver(appLifecycleObserver);
    }
}
```

You can check the Github source in [here](https://github.com/phamducminh/detect-app-fore-back-ground).

### References

* [
Handling Lifecycles with Lifecycle-Aware Components](https://developer.android.com/topic/libraries/architecture/lifecycle#java)
* [LifecycleObserver](https://developer.android.com/reference/android/arch/lifecycle/LifecycleObserver)
* [ProcessLifecycleOwner](https://developer.android.com/reference/android/arch/lifecycle/ProcessLifecycleOwner)

## Inspiration

Inspired by [Carlos Daniel](https://medium.com/@cdmunoz) and his post about [Detecting when an Android app is in foreground or background](https://medium.com/mindorks/detecting-when-an-android-app-is-in-foreground-or-background-7a1ff49812d7).
