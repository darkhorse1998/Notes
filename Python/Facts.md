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
18. `with open("file.txt", "r") as file:` is actually a context manager which ensures that the the file closes even if there are any exception when the file is open. It does that through the `__exit__()` dunder method.
19. Python is dynamically typed language. The type of objects or variables are decided at runtime. Hence, you don't need to declare variable type beforehand. Also, a variable storing integer can later be assigned to store strings.
20. `list.pop()` removes last element of list. `list.insert(<index>,<value>)` can be used to insert elements at certain index, which is O(n) operation.
21. `list.sort(key=lamda x: len(x))` will help sort a list based on length of the elements.
22. Strings are immutable. For example, you cannot change the second letter of a string. You can concatenate, but that will create a new string. You can re-assign the variable to a new string, but that is not changing the string.
23. You can get the ASCII value of a character by `ord(<character>)`
24. 