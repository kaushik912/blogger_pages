# Javascript Challenges
## Challenge 1 

- Explain why below code prints `undefined` instead of `John Doe`
- Fix the code to print `John Doe`

```
let person = {
  name: 'John Doe',
  getName: function() {
      console.log(this.name);
  }
};

setTimeout(person.getName, 1000);
```
Lets do some analysis
### When Method is in an Object: 
- The method `getName` is initially defined as part of the `person` object. 
- When you call `person.getName()`, the `this` keyword refers to the `person` object. So `this.name` would correctly return 'John Doe'.

### The Issue with `setTimeout`
- However, when a function is passed as a callback (like `person.getName` in this case), it loses its context (i.e., the reference to the person object). 
- Instead, the value of `this` inside `getName` changes to the global object (window in browsers, or global in Node.js).
- In non-strict mode, the global object (`window `in browsers) doesn't have a name property, so `this.name` becomes `undefined`.

### How `this` changes in `setTimeout`:
- When `setTimeout` calls `person.getName`, it is invoked as a regular function, not as a method on the `person` object.
- In this case, `this` inside `getName` no longer refers to the `person` object, but instead refers to the global object (`window`), which does not have a name property.
- Therefore, console.log(this.name) outputs `undefined`.

## Solutions
- Solution 1: Use .bind() to explicitly set the context of this:

    `setTimeout(person.getName.bind(person), 1000);`

- Solution 2: Use an Arrow Function (which doesn't have its own this):

    `setTimeout(() => person.getName(), 1000);`

- Solution 3: Store this in a variable (for older browsers or environments that don’t support .bind() or arrow functions)

    ```
    let self = person;
    setTimeout(function() {
        self.getName();
    }, 1000);
    ```
- Solution 4: Simply using anonymous function and person object
    ```
    setTimeout(function() {
        person.getName();
    }, 1000);
    ```

### Challenge 2
Consider the following function declaration.
```
    function MyClass() {
    this.name = 'John Doe';
    
    setTimeout(function() {
        console.log("Printing..")
        console.log(this.name);  // 'this' will refer to the global object, not MyClass
    }, 1000);
    }
```
- Try to guess what the below code prints.
- Please fix the code below so that it prints 'John Doe'.

