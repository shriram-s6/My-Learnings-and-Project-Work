# CPSC 8700: Mid-term II

## 1. smart pointers

In modern C++ programming, the Standard Library includes smart pointers, which are used to help ensure that programs are free of memory and resource leaks and are exception-safe.
Smart pointers are used to make sure that an object is deleted if it is no longer used (referenced).

Always create smart pointers on a separate line of code, never in a parameter list, so that a subtle resource leak won't occur due to certain parameter list allocation rules.
    
Example of memory leak:
```
    void my_func()
    {
        int* valuePtr = new int(15);
        int x = 45;
        // ...
        if (x == 45)
            return;   // here we have a memory leak, valuePtr is not deleted
        // ...
        delete valuePtr;
    }
     
    int main()
    {
    }
```

Types of unique pointers:

* unique_ptr
```
A unique_ptr does not share its pointer. It cannot be copied to another unique_ptr, passed by value to a function, or used in 
any C++ Standard Library algorithm that requires copies to be made. A unique_ptr can only be moved. This means that the ownership 
of the memory resource is transferred to another unique_ptr and the original unique_ptr no longer owns it. 
```

* shared_ptr 
```
The shared_ptr type is a smart pointer in the C++ standard library that is designed for scenarios in which more than one owner 
might have to manage the lifetime of the object in memory. After you initialize a shared_ptr you can copy it, pass it by value 
in function arguments, and assign it to other shared_ptr instances. All the instances point to the same object, and share 
access to one "control block" that increments and decrements the reference count whenever a new shared_ptr is added, goes out 
of scope, or is reset. When the reference count reaches zero, the control block deletes the memory resource and itself.
```

* weak_ptr
```
Sometimes an object must store a way to access the underlying object of a shared_ptr without causing the reference count 
to be incremented. Typically, this situation occurs when you have cyclic references between shared_ptr instances.

The best design is to avoid shared ownership of pointers whenever you can. However, if you must have shared ownership of 
shared_ptr instances, avoid cyclic references between them. When cyclic references are unavoidable, or even preferable for 
some reason, use weak_ptr to give one or more of the owners a weak reference to another shared_ptr. By using a weak_ptr, 
you can create a shared_ptr that joins to an existing set of related instances, but only if the underlying memory resource 
is still valid. A weak_ptr itself does not participate in the reference counting, and therefore, it cannot prevent the 
reference count from going to zero. However, you can use a weak_ptr to try to obtain a new copy of the shared_ptr with 
which it was initialized. If the memory has already been deleted, the weak_ptr's bool operator returns false. If the memory 
is still valid, the new shared pointer increments the reference count and guarantees that the memory will be valid as long 
as the shared_ptr variable stays in scope.
```
