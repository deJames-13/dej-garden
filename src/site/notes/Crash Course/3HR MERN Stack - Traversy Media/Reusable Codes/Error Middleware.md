---
{"dg-publish":true,"permalink":"/crash-course/3-hr-mern-stack-traversy-media/reusable-codes/error-middleware/","noteIcon":""}
---

`middleware/errorMiddleware.js`
```js
const notFound = (req, res, next) => {
  const error = new Error(`Not Found - ${req.originalUrl}`);
  res.status(404);
  next(error);
};


const errorHandler = (err, req, res, next) => {
  let statusCode = res.statusCode === 200 ? 500 : res.statusCode;
  let message = err.message || 'Internal Server Error';

  if (err.name === 'CastError' && err.kind === 'ObjectId') {
    statusCode = 404;
    message = 'Resource not found';
  }

  res.status(statusCode).json({
    message,
    stack: process.env.NODE_ENV === 'production' ? null : err.stack,
  });

};
export { errorHandler, notFound };
```

The provided code defines two middleware functions for handling errors in an Express application: `notFound` and `errorHandler`. These functions are then exported for use in other parts of the application.

### [`notFound`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") Middleware

```js
const notFound = (req, res, next) => {
  const error = new Error(`Not Found - ${req.originalUrl}`);
  res.status(404);
  next(error);

};
```

#### Explanation:
- **Purpose**: This middleware is used to handle requests to routes that do not exist.
- **Parameters**:
    - [`req`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The request object.
    - [`res`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The response object.
    - [`next`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The next middleware function in the stack.
- **Functionality**:
    - Creates a new [`Error`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") object with a message indicating that the requested URL was not found.
    - Sets the HTTP status code of the response to [`404`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") (Not Found).
    - Passes the error to the next middleware function using [`next(error)`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition").

### [`errorHandler`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") Middleware

```js
const errorHandler = (err, req, res, next) => {
  let statusCode = res.statusCode === 200 ? 500 : res.statusCode;
  let message = err.message || 'Internal Server Error';
  if (err.name === 'CastError' && err.kind === 'ObjectId') {
    statusCode = 404;
    message = 'Resource not found';
  }
  res.status(statusCode).json({
    message,
    stack: process.env.NODE_ENV === 'production' ? null : err.stack,
  });
};
```

#### Explanation:

- **Purpose**: This middleware handles errors that occur in the application and sends a JSON response with error details.
- **Parameters**:
    - [`err`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The error object.
    - [`req`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The request object.
    - [`res`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The response object.
    - [`next`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The next middleware function in the stack (not used here but required for middleware signature).
- **Functionality**:
    - Determines the status code to send in the response. If the response status code is [`200`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") (OK), it sets it to [`500`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") (Internal Server Error). Otherwise, it uses the existing status code.
    - Sets the error message to the error's message or a default message ('Internal Server Error').
    - Checks if the error is a [`CastError`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") with a kind of [`ObjectId`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") (typically a MongoDB error indicating an invalid ObjectId). If so, it sets the status code to [`404`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") and the message to 'Resource not found'.
    - Sends a JSON response with the following properties:
        - [`message`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The error message.
        - [`stack`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The error stack trace (only included if the environment is not production).

### Export

```js
export { errorHandler, notFound };
```

#### Explanation:

- Exports the [`errorHandler`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") and [`notFound`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") middleware functions so they can be imported and used in other parts of the application.