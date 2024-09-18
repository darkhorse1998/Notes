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

However, it would not allow you to check the source code of buily-in classes like list.

