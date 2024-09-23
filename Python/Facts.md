# Facts

1. lambda functions are one-line annonymous functions like `lamda x: x.upper()`
2. List comprehension mean using one-liners for list operations like `[x for x in items if x>10]`
3. filter function will keep an element in an iterable if the condition returns true.
4. You can sort a dictionary by using **key** attribute of ``sorted`` function
5. You can combile multiple lists using `zip`
6. Using `with open as` convention is better for dealing with files as it closes the files after operation is performed or even after any errors are encountered making sure that the file is not open and resulting in memory leaks. Example: `with open(file1,"w") as file:`
7. If we import a module or any function from the module, it will execute all the code inside sample module, including function definiations. Even if you are importing specific functions, you are not saving any resources as the entire module gets executed when it is imported.
8. `sum([x**2 for x in range(1000000)])` will actually create all the 1000000 values and then add them. It will consume a lot of reosurces. Hence, use generators where the values are not created all at once, but they are generated one at a time when required: `sum(x**2 for x in range(1000000))`
9. `id()` would give the memory address of the python object, function, variables etc. You can use `inspect` module to get information about the python members.
10. Behind all higher level operations on a class, there would be a lower level dunder/magic method that implements that operation.
11. A thread is given a read-only identifier or identity number by the interpreter only after it is started, stored under the instance variable of `ident`. Native identifier is also an unique ID given to the threads by the OS and stored under the property name of `native_id`. Generally, thread identifier & native identifier have the same value.
12. If a thread wants to wait for some other thread, use `join()` method on the dependant thread. For example, `t1.join()` would ensure that other threads are not executed until thread t1's execution is over. We can set a timeout in the `join()` method. Once timeout is exceeded the execution of the other threads would continue along with the dependant thread.
13. `time.time()` returns value in epoch form; time elapsed since system's starting time.
14. **Race condition** is a bug in multithreading and occurs when 2 or more threads try to update the same variable (concurrent access to shared resources) and produce unreliable results. The portion where race condition happens is called **critical section**.
15. Race condition can be avoided by using Locks, R-Locks, Semaphores.
16. Lock is a class of threading module. It has `acquire()` & `release()` methods through which we can avoid the race condition in the critical conditions.
17. RLock is an upgraded form of Lock class that can help in acquiring and releasing multiple times together. Also it provides information about the lock, including the thread ID which locked it, how many times etc. RLock helps to avoid issues in case due to some abstraction we are not able to see locks already present, and we end up making another lock.
18. Semaphore can be used to define the maximum number of threads that can executed at a time. If no values are set on the number of threads, default value is 1. It has built in RLock. But, be careful about how many times you acquire and release; it should be equal. With Semaphore, you can't avoid race condition unless you set it to default values.
19. To avoid inconsistencies with unequal acquire and release counts, you can use `BoundedSemaphore` instead of Semaphore.
20. When exception occurs in one of the threads, other threads are left undisturbed. In case of main thread, when an exception occurs, the `sys.excepthook` is called but for the created threads, during an exception, `threading.excepthook` is called and if there are any exceptions in the `threading.excepthook` then `sys.excepthook` is called.
21. We can override the `threading.excepthook` from our end.
22. A thread is a flow of execution and every program has atleast 1 thread calld Main Thread.
23. Thread lifecycle: new thread -> running thread (blocked) -> terminated thread (normlly or with exceptions)
24. Decorators can be used to modify the bevaiour of a fucntion without changing any code. This can be useful in cases where we want to test or validate a function without changing aything on it. It can also help in capturing the time it took to run a particular function.
25. Generator functions are a special kind of function that return a lazy iterator. These are objects that you can loop over like a list. However, unlike lists, lazy iterators do not store all their contents in memory at a time.
26. Generators can help resolve memory issues we might encounter with large numbers of infinite sequences.
27. `with open("file.txt", "r") as file:` is actually a context manager which ensures that the the file closes even if there are any exception when the file is open. It does that through the `__exit__()` dunder method.
28. Python is dynamically typed language. The type of objects or variables are decided at runtime. Hence, you don't need to declare variable type beforehand. Also, a variable storing integer can later be assigned to store strings.
29. `list.pop()` removes last element of list. `list.insert(<index>,<value>)` can be used to insert elements at certain index, which is O(n) operation.
30. `list.sort(key=lamda x: len(x))` will help sort a list based on length of the elements.
31. Strings are immutable. For example, you cannot change the second letter of a string. You can concatenate, but that will create a new string. You can re-assign the variable to a new string, but that is not changing the string.
32. You can get the ASCII value of a character by `ord(<character>)`
33. `*args` can accept any number positional arguements and stores them in a tuple. `**kwargs` can accept any number of keyword arguments and sore it in a dictionary.
34. Positional arguments should be defined before keyword arguments.
35. Exponential operator follows right to left associativity evaluation. `2**3**2` would result in 512 and not 64.
36. `<str>[start:stop:step]` depending on step, direction is decided. If stop value is not found then the output would be blank. \
For example, let `name` store the value of "shantanu". If we use `name[2,8,-2]`, python would start at character "a" on index 2 and go towards left side because step is negative. It won't find stop index 8 (or 7, as 8 is not inclusive) and hence return blank reponse.
37. Dictionary keys cannot be duplicate. If there are duplicates, the latest value will be considered. If you do a `len` on a dictionary with 3 elements where 2 elements have same keys then the length would be shown as 2 and not 3.
