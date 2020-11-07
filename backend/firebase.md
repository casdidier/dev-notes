# tooling

`npm i firebase-tools -g`
`firebase login

# cloud functions

complete isolated node.js env agnostic to frontend code

## deploy

`firebase deploy --only functions

## http functions

triggered manually from the client

```ts
export const basicHTTP = functions.https.onRequest((request, response) => {
  const name = request.query.name;

  if (!name) {
    response.status(400).send("ERROR you must supply a name :(");
  }

  response.send(`hello ${name}`);
});

// Express.js

// Multi Route ExpressJS HTTP Function
const app = express();

app.get("/cat", (request, response) => {
  response.send("CAT");
});

app.get("/dog", (request, response) => {
  response.send("DOG");
});

export const api = functions.https.onRequest(app);

// Custom Middleware
const auth = (request, response, next) => {
  if (!request.headers.authorization) {
    response.status(400).send("unauthorized");
  }
  next();
};

// Multi Route ExpressJS HTTP Function
const app = express();
app.use(cors({ origin: true }));
app.use(auth);
```

## background functions

triggered
event happens => call some functions => trigger some code in that function

### firestore functions
