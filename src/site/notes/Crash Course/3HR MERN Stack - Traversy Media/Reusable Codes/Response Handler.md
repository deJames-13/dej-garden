---
{"dg-publish":true,"permalink":"/crash-course/3-hr-mern-stack-traversy-media/reusable-codes/response-handler/","noteIcon":""}
---

`utils/responseHandler.js`
```js
import ValidationError from '../errors/validationError.js';

const errorHandler = ({ res, statusCode = 400, message, ...details }) => {
  res.status(statusCode);
  if (statusCode === 422) throw new ValidationError(message, details.errors);
  throw new Error(message);
};

  
const successHandler = ({ res, statusCode = 200, message, ...details }) => {
  return res.status(statusCode).json({
    success: true,
    status: statusCode,
    message: message ?? '',
    ...details,
  });
};

export { errorHandler, successHandler };
```


> [!NOTE] Key Points
> `ValidationError` - a custom Error for handling validation errors, see [[Crash Course/3HR MERN Stack - Traversy Media/Reusable Codes/Errors\|Errors]]
> `errorHandler` -  a handler function that throws an error with default status code and a formatted message and error details
> `successHandler` - a handler function that sends a success status code and a JSON response
> `res` - is the response from the route or controller
> 
