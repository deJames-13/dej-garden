---
{"dg-publish":true,"permalink":"/crash-course/3-hr-mern-stack-traversy-media/reusable-codes/token-handler/","noteIcon":""}
---

```js
import jwt from 'jsonwebtoken';
import { JWT_EXPIRE, JWT_SECRET, NODE_ENV } from '../env/index.js';

  
const defaultCookieOptions = {
  httpOnly: true,
  secure: NODE_ENV !== 'development',
  sameSite: 'strict',
  maxAge: JWT_EXPIRE * 24 * 60 * 60 * 1000,
};

  
const generateToken = (userId, tokenName, tokenAge, options = {}) => {
  if (!userId) return null;
  const token = jwt.sign({ userId }, JWT_SECRET, {
    expiresIn: `${JWT_EXPIRE}d` || '30d',
  });


  return [
    tokenName || 'jwt',
    token,
    {
      ...defaultCookieOptions,
      maxAge: tokenAge || JWT_EXPIRE * 24 * 60 * 60 * 1000,
      ...options,
    },
  ];
};

  
const destroyToken = (tokenName, options = {}) => {
  return [
    tokenName || 'jwt',
    '',
    {
      ...defaultCookieOptions,
      maxAge: 0,
      ...options,
    },
  ];
};

export { destroyToken, generateToken };
```

The provided code defines two functions, [`generateToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") and [`destroyToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"), which are used to create and invalidate JWTs (JSON Web Tokens) respectively. These functions return arrays that can be used to set cookies with the appropriate options. Here's a detailed explanation:

### Imports and Constants

import jwt from 'jsonwebtoken';

import { JWT_EXPIRE, JWT_SECRET, NODE_ENV } from '../env/index.js';

- **jwt**: The [`jsonwebtoken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") library is used to create and verify JWTs.
- **JWT_EXPIRE, JWT_SECRET, NODE_ENV**: These constants are imported from an environment configuration file. They represent the token expiration time, the secret key for signing tokens, and the current environment (development or production), respectively.

### Default Cookie Options

```js
const defaultCookieOptions = {
  httpOnly: true,
  secure: NODE_ENV !== 'development',
  sameSite: 'strict',
  maxAge: JWT_EXPIRE * 24 * 60 * 60 * 1000,
};
```

1 vulnerability

- **defaultCookieOptions**: This object defines default settings for cookies:
    - [`httpOnly: true`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The cookie is inaccessible to JavaScript running on the client side.
    - [`secure: NODE_ENV !== 'development'`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The cookie is only sent over HTTPS connections in non-development environments.
    - [`sameSite: 'strict'`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The cookie is only sent with requests originating from the same site.
    - [`maxAge: JWT_EXPIRE * 24 * 60 * 60 * 1000`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The cookie's maximum age is set based on the [`JWT_EXPIRE`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") value, converted to milliseconds.

### [`generateToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") Function

```js
const generateToken = (userId, tokenName, tokenAge, options = {}) => {
  if (!userId) return null;
  const token = jwt.sign({ userId }, JWT_SECRET, {
    expiresIn: `${JWT_EXPIRE}d` || '30d',
  });
  return [
    tokenName || 'jwt',
    token,
    {
      ...defaultCookieOptions,
      maxAge: tokenAge || JWT_EXPIRE * 24 * 60 * 60 * 1000,
      ...options,
    },
  ];
};

```
- **Parameters**:
    
    - [`userId`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The ID of the user for whom the token is being generated.
    - [`tokenName`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The name of the cookie (default is `'jwt'`).
    - [`tokenAge`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The optional age of the token in milliseconds.
    - [`options`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): Additional options to override the default cookie settings.
- **Functionality**:
    
    - If [`userId`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") is not provided, the function returns [`null`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition").
    - A JWT is created with the payload `{ userId }`, signed with [`JWT_SECRET`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"), and an expiration time based on [`JWT_EXPIRE`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition").
    - The function returns an array containing:
        - The cookie name ([`tokenName`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") or `'jwt'`).
        - The generated token.
        - The cookie options, combining [`defaultCookieOptions`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"), the provided [`tokenAge`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"), and any additional [`options`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition").

### [`destroyToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") Function

```js
const destroyToken = (tokenName, options = {}) => {
  return [
    tokenName || 'jwt',
    '',
    {
      ...defaultCookieOptions,
      maxAge: 0,
      ...options,
    },
  ];
};
```

- **Parameters**:
    
    - [`tokenName`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): The name of the cookie to be invalidated (default is `'jwt'`).
    - [`options`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"): Additional options to override the default cookie settings.
- **Functionality**:
    
    - The function returns an array containing:
        - The cookie name ([`tokenName`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") or `'jwt'`).
        - An empty string as the token value, effectively invalidating the token.
        - The cookie options, combining [`defaultCookieOptions`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition"), setting [`maxAge`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") to [`0`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") (to expire the cookie), and any additional [`options`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition").

### Export

```js
export { destroyToken, generateToken };
```

- The [`destroyToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") and [`generateToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition") functions are exported for use in other parts of the application.

### Summary

- **[`generateToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition")**: Creates a JWT for a user and returns an array to set a cookie with the token and appropriate options.
- **[`destroyToken`](vscode-file://vscode-app/d:/i/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Go to definition")**: Invalidates a JWT by returning an array to set a cookie with an empty value and an expiration time in the past.

These functions are useful for managing user authentication tokens in a web application, ensuring that tokens are securely stored and can be invalidated when necessary.
