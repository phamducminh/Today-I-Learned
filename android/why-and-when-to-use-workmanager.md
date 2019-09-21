# When and Why to use WorkManager

A good way to organize the background solutions is to split them based on the time and importance:

* the vertical axis is the timing of the work: work should be done when it is specified or it could wait for a bit.

* the horizontal axis represents how important is the work. It should be done when the app is in foreground or it needs to be done at a certain point.

![](https://github.com/phamducminh/Today-I-Learned/blob/master/android/resources/split-background-solutions-based-on-the-time-and-importance.png)

There comes a rescue

> WorkManager is a library for managing **deferrable** and **guaranteed** background work.

**Deferrable work:** the task can run later and it is still useful (uploading logs vs send a message)

**Guaranteed:** the task will run even the app will be closed or the device restarts (backup images to a server)

## Why to use WorkManager?

* **Backward compatibility** with different OS versions (API level 14+)
* Follows system health **best practices**
* Supports **one-off** and **periodic tasks**
* Supports **chained tasks with input/output**
* Defines **constraints** on when the task runs
* **Guarantees task execution**, even if the app or device **restarts**

## When to use WorkManager?

* Fetching data periodically
* Uploading images/files to a server
* Syncing data between app and server
* Sending logs to a server
* Executing expensive operations on data

## Inspiration

Inspired by [Magda Miu](https://proandroiddev.com/@magdamiu) and her series about WorkManager. You can read this [here](https://proandroiddev.com/workout-your-tasks-with-workmanager-intro-db5aefe14d66)