---
{"dg-publish":true,"permalink":"/crash-course/3-hr-mern-stack-traversy-media/reusable-codes/database-config/","noteIcon":""}
---

`config/db.js`
```js
import mongoose from 'mongoose';

const connectDB = async (uri,success = () => {}) => {
  return mongoose
    .connect(uri)
    .then(() => {
      console.log('Connected to database');
      success();
    })
    .catch((e) => {
      console.log('Error connecting to database: ', e.message);
    });
};
export default connectDB;
```

### Usage
```js
  connectDB(MONGO_URI, () => {
	// connect to database and start server if success
    app.listen(PORT, () => {
      console.log(`Server is running on port ${PORT}`);
    });
  });
```