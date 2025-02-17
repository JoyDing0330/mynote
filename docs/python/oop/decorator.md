``` py
def decorator(func):
    def wrapper():
        print("Do something before calling the function.")
        func()
        print("Do something after calling the function.")
    return wrapper

@decorator
def greet():
    print("Hello, world!")

greet()
```