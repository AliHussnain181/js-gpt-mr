Decorators are a stage-2 ECMAScript proposal that allow for modifying classes and their members. They provide a way to annotate and modify classes and properties at design time. Here's a breakdown of decorators for beginners, intermediate, and advanced levels.

### Beginners

**What are Decorators?**
- Decorators are functions that are used to modify the behavior of classes and class members.
- They provide a clean syntax to add meta-programming features to your code.

**Basic Syntax:**
- Decorators are prefixed with an `@` symbol and placed above the class or class member they are intended to modify.

**Simple Example:**
```javascript
function log(target) {
    console.log(target);
}

@log
class MyClass {
    constructor() {
        this.name = 'MyClass';
    }
}
```
In this example, the `log` decorator simply logs the class definition.

**Use Cases:**
- Logging
- Adding metadata

### Intermediate

**Decorating Methods:**
- Decorators can also be used to modify methods within a class.

**Example:**
```javascript
function logMethod(target, key, descriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args) {
        console.log(`Calling ${key} with arguments: ${args}`);
        return originalMethod.apply(this, args);
    };

    return descriptor;
}

class MyClass {
    @logMethod
    sayHello(name) {
        return `Hello, ${name}!`;
    }
}

const myClass = new MyClass();
console.log(myClass.sayHello('World'));
```
This example logs the method call and its arguments.

**Using Parameters:**
- Decorators can take parameters to customize their behavior.

**Example:**
```javascript
function log(prefix) {
    return function (target, key, descriptor) {
        const originalMethod = descriptor.value;

        descriptor.value = function (...args) {
            console.log(`${prefix} ${key} with arguments: ${args}`);
            return originalMethod.apply(this, args);
        };

        return descriptor;
    };
}

class MyClass {
    @log('Executing')
    sayHello(name) {
        return `Hello, ${name}!`;
    }
}

const myClass = new MyClass();
console.log(myClass.sayHello('World'));
```
This example uses a parameterized decorator to prefix log messages.

### Advanced

**Decorating Properties:**
- Decorators can also be used to modify properties within a class.

**Example:**
```javascript
function readOnly(target, key, descriptor) {
    descriptor.writable = false;
    return descriptor;
}

class MyClass {
    @readOnly
    myProperty = 'This is read-only';

    constructor() {
        this.myProperty = 'Trying to change'; // This will not change the value
    }
}

const myClass = new MyClass();
console.log(myClass.myProperty); // Output: This is read-only
```
This example demonstrates a decorator that makes a property read-only.

**Applying Multiple Decorators:**
- You can apply multiple decorators to a single method or property.

**Example:**
```javascript
function log(target, key, descriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args) {
        console.log(`Calling ${key} with arguments: ${args}`);
        return originalMethod.apply(this, args);
    };

    return descriptor;
}

function readonly(target, key, descriptor) {
    descriptor.writable = false;
    return descriptor;
}

class MyClass {
    @log
    @readonly
    sayHello(name) {
        return `Hello, ${name}!`;
    }
}

const myClass = new MyClass();
console.log(myClass.sayHello('World')); // Logs call and returns value
```
This example demonstrates applying both logging and read-only decorators to a method.

**Decorator Factories:**
- Decorator factories are functions that return decorators, allowing for more flexible and reusable decorators.

**Example:**
```javascript
function debounce(delay) {
    return function (target, key, descriptor) {
        const originalMethod = descriptor.value;
        let timeout;

        descriptor.value = function (...args) {
            clearTimeout(timeout);
            timeout = setTimeout(() => originalMethod.apply(this, args), delay);
        };

        return descriptor;
    };
}

class MyClass {
    @debounce(300)
    handleResize() {
        console.log('Resize event handled');
    }
}

const myClass = new MyClass();
window.addEventListener('resize', () => myClass.handleResize());
```
This example demonstrates a debounce decorator that delays method execution.

### Summary
- **Beginners:** Understand the basic syntax and simple use cases of decorators.
- **Intermediate:** Learn to use decorators for methods, and explore parameterized decorators.
- **Advanced:** Explore property decorators, multiple decorators, and decorator factories for complex use cases.

Decorators provide a powerful way to extend and enhance the functionality of classes and their members, making code more modular and easier to maintain.