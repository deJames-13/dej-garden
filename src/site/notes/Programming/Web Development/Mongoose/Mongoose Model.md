---
{"dg-publish":true,"permalink":"/programming/web-development/mongoose/mongoose-model/","noteIcon":""}
---


> [!NOTE] Model
> In *MongoDB*, a `model` is a representation of a collection in the database. It defines the structure of the documents within that collection, including the fields and their data types. When using `Mongoose`, an **Object Data Modeling (ODM)** library for MongoDB and Node.js, models are created using schemas.
> 
> >[!NOTE] Key Points
> > - **Schema:** Defines the structure and data types of the documents in a collection.
> >- **Model:** Provides an interface to interact with the database, allowing for CRUD operations.

### Installation
```sh
npm install mongoose
```

### Schema
```js
// Define a schema
const bookSchema = new mongoose.Schema({
  title: { type: String, required: true },
  author: { type: String, required: true },
  publishedYear: Number,
  genres: [String]
});
```

### Model
```js
const Book = mongoose.model('Book', bookSchema);
```
