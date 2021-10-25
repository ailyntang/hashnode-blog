## Random troubleshooting for Firebase

`FirebasePerformanceTraceId`

The issue is that in startTraceWithId you are accessing the traces dictionary from a background thread. 

A dictionary is not thread safe so if another thread is also trying to use it (to start or end a trace) you'll have problems and the app will crash