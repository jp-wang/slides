## How to build navigation HMI?



## What's the characteristics?

- Single activity
- Map takes the center
- Event driven
- Multiple screen sizes
- High quality and robust
- Limited system resource



## How did we solve them in Denali?


Flow Manager to support in-app UI navigation.


Map Fragment everywhere.


Singleton pattern plus observer callback.


Multiple layouts, dimensions, drawables, configurations and if...else... code.


Heavily rely on QA manual testing.



## What's the tradeoff?


Flow manager is purely built by Telenav which is not easy to adapt with Android app lifecycle. And It's not able to integrate with existing native animation system.


Map fragment is flowing through everywhere which cause our business layer not having clean separation between business logic and UI code.


Multiple layouts and dimensions have a lot of boilerplate code which is hard to maintain and modify, and very hard to scale it.


Heavily depending on QA manual testing make our code too fragility, rigidity and immobility.



## How to solve it?


Android Jetpack!

<img src="img/jetpack.png" alt="jetpack" width="400"/>


> Jetpack is a suite of libraries, tools, and guidance to help developers write high-quality apps easier. These components help you follow best practices, free you from writing boilerplate code, and simplify complex tasks, so you can focus on the code you care about.

Published by Google. Android's first citizen library. Huge Android development community.


Current|Jetpack
-|-
Flow manager|Navigation
MVC(actually not)|MVVM-unidirectional dependency
Multiple layouts|ConstraintLayout
Singleton+Observer|LiveData
File I/O|Room
Thread|Coroutine

>Note: Three senior resources should be enough. Engineering culture is actually about how you solve problems with tools.