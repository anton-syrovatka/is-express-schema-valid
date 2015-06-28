# is-express-schema-valid

![](http://img.shields.io/badge/Status-Work%20In%20Progress-FA572C.svg?style=flat-square)

> Middleware to validate json schema of `req.body`, `req.params` and `req.query`. It is based on [JSONSchema](http://json-schema.org) spec and [is-my-json-valid](https://github.com/mafintosh/is-my-json-valid) _"that uses code generation to be extremely fast"_.

```bash
npm install is-express-schema-valid --save
```

## Example

```javascript
import express from 'express';
import bodyParser from 'body-parser';
import validate from 'is-express-schema-valid';

const app = express();

const loginSchema = {
    payload: {
        email: {
            type: 'string',
            required: true,
            format: 'email'
        },
        password: {
            type: 'string',
            required: true,
            minLength: 1
        }
    }
};

app.use(bodyParser.json());
app.post('/login',
    validate(loginSchema),
    findAndLoginUser
);
app.use(handleErrors);

function findAndLoginUser (req, res, next) {
    // if schema validation fails 
    // this middleware won't be called
}

function handleErrors (err, req, res, next) {
    // validation error will passed as first argument
    // you can return it or match with your api responses
}

app.listen(3000);
```

## Defining schemas

When defining a schema for request's payload/params/query you're able to pass a plain object. In this case `is-express-schema-valid` will automagically populate your schema with necessary properties:

```javascript
const schema = {
    payload: {
        foo: {
            type: 'string',
            required: true
        }
    }
};

// will be passed to validator as:
// { 
//   type: 'object', 
//   required: true, 
//   additionalProperties: false, 
//   properties: { 
//     foo: { 
//       type: 'string', 
//       required: true 
//     }
//   }
// }
```

Or you can use a full schema if default one doesn't match your needs, for example when payload needs to be an array or another type:

```javascript
const schema = {
    payload: {
        type: 'array',
        "uniqueItems": true,
        items: {
            type: 'number'
        }
    }
};

// it will be used as is by validator
```

## Built-in formats

## JSONSchema

In order to get comfortable with JSON Schema spec and know its' additional features I advice you to check this book ["Understanding JSON Schema"](http://spacetelescope.github.io/understanding-json-schema/UnderstandingJSONSchema.pdf) or look at [examples](http://json-schema.org/examples.html).

### To Do

- [ ] Response schemas

---

**MIT Licensed**
