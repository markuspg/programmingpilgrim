---
title: "Wrapping a C++ SDK with C++/CLI"
date: 2019-07-16T21:54:45+02:00
draft: false
toc: false
images:
tags: 
  - c++
  - windows
---
Recently at work I had to wrap a C++ SDK for usage in C#.
My first try was using _P/Invoke_ which seems to be the more portable way but soon this approach prove to be far too limited (through having to rely only on _C_ linkage).
Therefore my coworkers and me decided to utilize _C++/CLI_ for which an introduction can be found [here](https://docs.microsoft.com/en-us/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp?view=vs-2019).

One basic problem is how to embed the _unmanaged C++_ classes from the SDK in _managed C++/CLI_ ones.
Fortunately managed classes can have pointers to unmanaged objects as private data members, so I created the base class of the inheritance hierarchy mirroring the SDK's inheritance hierarchy like this:

    public ref class WrapperObject {
    public:
        WrapperObject(SdkObject *const argObj) :
            obj(argObj)
        {}
        # The destructor should do nothing more than calling the finalizer
        virtual ~WrapperObject() { this->!WrapperObject(); }
        # The finalizer frees the internally held unmanaged object's instance
        !WrapperObject() { delete obj; }

        void DoSomething() { obj->DoSomething(); }

    private:
        SdkObject *obj = nullptr;
    };

The `void DoSomething()` member function gives an example how the managed object can forward calls to the unmanaged object.
