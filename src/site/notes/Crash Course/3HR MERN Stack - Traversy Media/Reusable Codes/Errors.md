---
{"dg-publish":true,"permalink":"/crash-course/3-hr-mern-stack-traversy-media/reusable-codes/errors/","noteIcon":""}
---

`errors/validationError.js`
```js
export default class ValidationError extends Error {
  constructor(message, errors) {
    super(message);
    this.name = this.constructor.name;
    this.errors = errors;
  }
}
```
### Explanation:

1. **Class Name: `ValidationError`**

    - This class represents an error related to validation.
    - It extends the built-in `Error` class, which allows it to inherit error-handling behavior.
2. **Constructor:**
    
    - The constructor function is called when you create an instance of the `ValidationError` class.
    - It accepts two parameters:
        - `message`: A string representing the error message.
        - `errors`: An additional parameter (likely an array or object) that holds specific details about validation errors.
3. **Properties:**
    
    - `name`: The `name` property is set to the name of the class (`ValidationError`).
    - `errors`: This property holds the validation errors associated with the instance.