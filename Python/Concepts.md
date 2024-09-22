# Concepts

## Is Python compiled or interpreted language?

Python does partially compiles the code into 'bytecode' which is not exactly machine readable but it is a form of low level language. It would only check for minor issues like syntax validation. Then the interpreter takes the bytecode and converts to machine code line by line one at a time on the go to make the program run on the machine. Hence, an interpreter is important.

Mobile/other devices might have issues with python as they might not have an interpreter to convert the bytecode into machine code. Most of the program is exectured during runtime and hence it can be termed as interpretted language, which sometimes can lead into issues as there might be problems undetected during compilation.

## Can you define a class inside a function?

Yes, python can be used to do interesting things like that. You cann define the class and the methods inside int and you can return the class itself istead of an instance. Now, you can use the return value to initiate instances of that class.

```python
def defineClass(x):
    class myClass:
        def __init__(self,name):
            self.name = name
        def consoleOut(self):
            print(x)
    return myClass

myClassTemplate = defineClass(9)
# myClassTemplate can be used as a class to initiate objects
myClassObject = myClassTemplate("Raghu")
print(myClassObject.name) #prints out 'Raghu'
print(myClassObject.consoleOut)   #prints out 9, as we gave that input when we created the class
```

## Can you get the source code for python objects?

You can use the `inspect` module to do this:

```python
import inspect
import datetime
print(inspect.getsource(datetime))
```

## Multithreading vs Multiprocessing in Python

Thread: Smallest unit of processing that can be scheduled by an operating system. In Python, threads are used to run multiple operations concurrently within the same process space. They are particularly useful for I/O-bound applications, where the program often waits for external events and performs less computation.

Process: Instance of a program that can execute independently from other processes. Processes have their own memory space and are managed by the operating system. They are suitable for CPU-bound tasks that require heavy computation and little I/O.

## Concurrency vs Parallelism

Concurrency is a condition wherein two or more tasks can be initiated and completed in overlapping periods on a single processor and core. Parallelism is a condition wherein multiple tasks or distributed parts of a task run independently and simultaneously on multiple processors. So, parallelism isn't possible on machines with a single processor and a single core.

Let's take an example of a barber shop, with 2 chairs and queues of customers for both those chairs. \
If a single barber switches between the 2 chairs and cuts hair, it would be concurrency. \
If 2 barbers attend to 2 chairs at the same time and cut hair, it would be parallelism.

## Pilars in OOP

### Abstraction

Abstraction is the concept of hiding all the implementation of your class away from anything outside of the class. \
Example: When we call a method of a class, we don't care about how it is working. Say, `list.append()`

Inheritance: Inheritance is the mechanism for creating a child class that can inherit behavior and properties from a parent(derived) class.

Encapsulation: Encapsulation is the method of keeping all the state, variables, and methods private unless declared to be public.

Polymorphism: Polymorphism is a way of interfacing with objects and receiving different forms or results. \
Example: Function overriding, when we inherit and overwrite the function in a child class.