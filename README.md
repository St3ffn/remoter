# Remoter

Remoter  - An alternative to Android AIDL for Android Remote IPC services using plain java interfaces

Remoter makes developing android **remote services** intuitive **without messing with AIDL**.

## Android IPC through AIDL

Android supports remote IPC using [AIDL](https://developer.android.com/guide/components/aidl.html). This process of using "aidl" is painful and limited.

Some of the **problems and limitations of AIDL** are : 

* Unlike the intuitive way of defining an inteface defintions as an **interface**, AIDL forces you to define the interface in an "**.aidl**" file
* The .aidl file is usually in a different folder than your normal source
* You lose most of the IDE capability for the "**.aidl**" 
* You can't use an existing interface class and convert that to a remote interface -- it has to be defined seperately as ".aidl"
* Only limited predefined data types are supported in aidl
* Any custom Parcelable class that you want to pass through the interface has to be defined again as another ".aidl" file!
* No overloaded methods!- Methods with same name fail
* Can't extend an aidl with another
* Can't throw custom exceptions

## Remoter - An intuitive way for Android IPC

Remoter solves the above problems in AIDL by allowing you to define the remote interface using plain java **interface**, and implement it using plain java implementation of the interface.

All you have to do is annotate the interface using **@Remoter**


```java
@Remoter
public interface ISampleService {
    ...
}

```

* No messy **.aidl**, just plain simple **interface**
* Implement the interface directly using intuitive normal java way, instead of extending Stub
* **Fully interoperable with AIDL**. Remoter creates the same serialized data as created by AIDL, so it is fully interoperable with AIDL
* Supports more data types than AIDL, everything supported by Parcel
* Make an interface that extends other interfaces as @Remoter
* Interface methods can throw any exceptions. Clients will get the same exception that is thrown.
* Remoter interface can be templated
* Remoter is an **annotation processor** that generates two helper classes during build time -- a client side Proxy and a service side Stub that allows you to wrap your interface and implementation


**At the client side**

* Simply wrap the binder that you got from the ServiceConnection with the autogenerated **Proxy** for your interface

```java
ISampleService sampleService = new ISampleService_Proxy( binder );

```

**At the service side**

* Wrap the implementation with the autogenerated **Stub** to covert it as a remote Binder and return that from your service

```java
Binder binder = new ISampleService_Stub( sampleServiceImpl );

```

That's it! 


**Annotations**

* **@Remoter** Annotate on an interface to make it a remote interface
* **@ParamIn** Mark an array or Parcelable parameter as an **input only** parameter(**in** of aidl). By **default** they are **input and output** (inout of aidl)
* **@ParamOut** Mark an array or Parcelable parameter as an **output only** parameter(**out** of aidl).
* **@Oneway** Annotate on a method (in the @Remoter interface) with void return to make it an asynchronous method. 



Getting Remoter
--------

Gradle dependency

```groovy
dependencies {
    implementation 'com.josesamuel:remoter-annotations:1.2.3'
    annotationProcessor 'com.josesamuel:remoter:1.2.3'
}
```


License
-------

    Copyright 2017 Joseph Samuel

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


