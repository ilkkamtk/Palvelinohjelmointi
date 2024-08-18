# Mongoose

* ["Elegant MongoDB object modeling for Node.js"](http://mongoosejs.com/)
* Provides a schema-based solution to model data
* Built-in type casting, validation, query building, business logic hooks etc.
* Methods return a promise
    * All async operations return promises
    * Queries do return object with await (or .then())
---

## Connecting

For example as separated module (utils/db.ts):
```typescript
import mongoose from 'mongoose';

const mongoConnect = async () => {
  try {
    const connection = await mongoose.connect(process.env.DATABASE_URL as string);
    console.log('DB connected successfully');
    return connection;
  } catch (error) {
    console.error('Connection to db failed: ', (error as Error).message);
  }
};

export default mongoConnect;
```

Then in .env file (make sure to have it in .gitignore):
```dotenv
DATABASE_URL=mongodb://<username>:<password>@<host>/name-of-database
PORT=3000
```
And in index.ts (or app.ts or server.ts):
```typescript
import dotenv from 'dotenv';
dotenv.config();
import express from 'express';
import mongoConnect from './utils/db';

const app = express();
const port = process.env.PORT || 3000;

(async () => {
  try {
    await mongoConnect();
    app.listen(port, () => {
      console.log(`Listening: http://localhost:${port}`);
    });
  } catch (error) {
    console.log('Server error', (error as Error).message);
  }
})();
```
* In case connection is lost, Mongoose automatically reconnects and executes all commands in buffer
* Format for URL is: mongodb://[username:password@]host[:port][/databasename]

## Task 1
* [Download starter files](https://github.com/ilkkamtk/ts_mongo_starter)
* Create a new GitHub repository and push the starter files there
* Connect to the animal database in your MongoDB Atlas cluster


---

## Schema

* maps to a MongoDB collection and defines the shape of the documents

E.g. as a module (models/blog.ts):
```typescript
import mongoose from 'mongoose';
import SomeType from '../path/to/interface'

const Schema = mongoose.Schema;

const blogSchema = new Schema<SomeType>({
  title:  String,
  author: String,
  body:   String,
  comments: [{ body: String, date: Date, author: String }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});

export default mongoose.model<SomeType>('Blog', blogSchema);
```
---

## Schema types

* [Schema](http://mongoosejs.com/docs/guide.html) supports basic [types](http://mongoosejs.com/docs/schematypes.html):
    * String
    * Number
    * Date
    * Buffer
    * Boolean
    * Mixed
    * ObjectId
    * Array
    * ...
* And [validation](https://mongoosejs.com/docs/validation.html):
    * all types have the built-in `required` validator.
    * Numbers have the built-in `min` and `max` validators.
    * Strings have the built-in `enum`, `match`, `minLength`, and `maxLength` validators.
    * and an option to create custom [validators](https://mongoosejs.com/docs/validation.html#custom-validators)

---
## TypeScript support
* [Mongoose Docs](https://mongoosejs.com/docs/typescript.html)

---

## Task 2

Create TypeScript types for 'Animal', 'Species' and 'Category' models. Then create mongoose schemas for them.


Category
* category_name (string, at least 2 characters long)

Species
* species_name (string, at least 2 characters long)
* image (Url), hint: [mongoose-type-url](https://www.npmjs.com/package/mongoose-type-url) or string
* category (id of the category)
* location ([geoJson point](https://mongoosejs.com/docs/geojson.html))
    * idea of this is to save the location where the species can be found

Animal
* animal_name (string, at least 2 characters long)
* birthdate (Date, is in the past (maximum is `Date.now`))
    * Check ES6 [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
    * Should prevent future dates
* species (id of the species)
* location ([geoJson point](https://mongoosejs.com/docs/geojson.html))
    * idea of this is to save the location where the animal is found

---

## Model

* To use the schema, we need to create a model associated with it:

```javascript
export default mongoose.model('Blog', blogSchema);
```

* Each instance of model is a document
* Only way to store and retrieve data to/from database

---

## CRUD

* To create a new document, call the [model](http://mongoosejs.com/docs/models.html) `create` or `save` methods
* To modify existing documents, call the `updateOne` method and the `remove` or `deleteOne` to delete
* Documents can be retrieved using models' `find`, `findById`, `findOne`, or `where`
* Specifies which document fields to include or exclude with `select` (or projection)
* and [more](https://mongoosejs.com/docs/api/query.html)

E.g. as a router (routes/blogRoute.ts):
```typescript
import express, {Request, Response} from 'express';
import blog from '../models/blog';

const router = express.Router();

router.route('/')
  .post(async (req: Request, res: Response) => {
    const post = await blog.create({
      title: req.body.title,
      body: req.body.body,
      hidden: false
    });
    res.send(`blog post ${post.title} created with id: ${post._id}`);
  })
  .get(async (req: Request, res: Response) => {
    res.send(await blog.find({ hidden: false }).where('date').gt(new Date(new Date().setFullYear(new Date().getFullYear() - 1))));
  });

router.route('/:id')
  .get(async (req: Request, res: Response) => {
    res.send(await blog.findById(req.params.id));
  })
  .patch(async (req: Request, res: Response) => {
    const mod = await blog.updateOne({ _id: req.params.id }, { title: req.body.title });
    res.status(200).send(`updated sucessfully ${mod.modifiedCount} blog post`);
  })
  .delete(async (req: Request, res: Response) => {
    const del = await blog.deleteOne({ _id: req.params.id });
    res.send(`deleted ${del.deletedCount} blog post`);
  });

export default router;
```

(and in your index.ts)
```typescript
import blogRoute from './routes/blogRoute';
//...
app.use(express.urlencoded({ extended: false })); // for parsing html form x-www-form-urlencoded
app.use(express.json()); // for parsing application/json
app.use('/blog', blogRoute);
```
---

## Task 3

* Expose your models through controller + router:
* Create endpoints `categories`, `species` and `animals`
    * On `POST` create a new category, species or animal
    * Respond to `GET /` with JSON containing all data from category, species or animal
    * On `GET /:id`, find the category, species or animal by id.
    * On `PUT /:id` update the category, species or animal
    * On `DELETE /:id`, delete the category, species or animal.
    * Respond to `GET /species/location` with an array of species located in a area specified by area. E.g  `GET /species/location?topRight=58.05,17.22&lowLeft=49.10,10.32` will return species inside Europe.
        * use `/species/location` endpoint and `?topRight=lat,lon&lowLeft=lat,lon` to specify the area. If you use other endpoint and/or query parameters, the test will fail.
    * Do the same location query also for animals.
* Test with postman.


---

# Instance methods

* Models have built-in instance methods like `save`, `remove`, `update`, `validate` etc.
* Models can also have custom [instance methods](http://mongoosejs.com/docs/guide.html#methods):

```javascript
// Do not declare methods using ES6 arrow functions (=>).
// Arrow functions explicitly prevent binding this-keyword
blogSchema.methods.findFromSameDate = function(cb) {
  return this.model('Blog').find({ date: this.date }, cb);
};
const post1 = new Blog({ date: Date.now(), ... });

const posts = await post1.findFromSameDate();
```

* Built-in instance methods should not be overwritten

---

# Static methods

* [Static methods](http://mongoosejs.com/docs/guide.html#statics) can be used for example to perform more sophisticated lookups:

```javascript
blogSchema.statics.findByTitle = function(title, cb) {
  return this.find({ title: new RegExp(title, 'i') }, cb);
};

const posts = await blog.findByTitle('My title');
```

---

# Query helpers

* [Query helpers](http://mongoosejs.com/docs/guide.html#query-helpers) allow chained query building:

```javascript
blogSchema.query.byTitle = function(title) {
  return this.find({ title: new RegExp(title, 'i') });
};

try {
  const posts = blog.find().byTitle('My title');
} catch(err) {
  console.error('query failed', err);
}
```

---

## Task 4

* Add a static method tht 'Animal' model to find all animals of certain species: `bySpecies(species: string): Promise<Animal[]>`
   * The endpoint should be `GET /animals/species/:species`
* Add a query helper to 'Species' model to find all species within a certain area specified by a geoJson polygon: `byLocation(polygon: Polygon): Promise<Species[]>` 
   * The endpoint should be `POST /species/area` and the body should contain the geoJson polygon in the following format:
   ```json
   {
     "type": "Polygon",
     "coordinates": [
       [
         [100.0, 0.0], [101.0, 0.0], [101.0, 1.0],
         [100.0, 1.0], [100.0, 0.0]
       ]
     ]
   }
   ```
* When ready, run `npm run test` to test your endpoints.
* Add a screenshot of test results to the README.md file in your repository. Submit the repository link to Oma.


---

# Sources

* [mongoose](http://mongoosejs.com/index.html)
* [mongoose NPM package](https://www.npmjs.com/package/mongoose)

---
