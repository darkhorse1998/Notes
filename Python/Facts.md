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