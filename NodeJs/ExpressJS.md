# What is ExpressJs

![image-20200707173453768](assets/ExpressJS/image-20200707173453768.png)

# Middleware

Request will be handled through a bunch of function in middle, from top to bottom in file

![image-20200707174013841](assets/ExpressJS/image-20200707174013841.png)

```js
const express = require("express")

const app = express();
app.use((req,res,next) => {
    console.log("this is 1 middleware")
    next() // move to next middleware
})
app.use((req,res,next) => {
    console.log("this is 2 middleware")
    res.send('<h1>Hello</h1>') // return the respone to the client, default is html content type
    // request end here
})
app.listen(3000)
```

![image-20200707204330129](assets/ExpressJS/image-20200707204330129.png)

# Parse request body

```js
const express = require("express")
const bodyParser = require("body-parser")

const app = express();
app.use(bodyParser.urlencoded({extened: false}));

app.use("/product", (req,res) => {
    const { title } = req.body;
    res.send(`<h1> ${title} page</h1>`)
})
app.listen(3000)
```

# Routes

`app.use([path],callback[,callback...]) `(Only GET, order of middleware matter)

```js
app.use("/", (req,res,next) => {
    console.log("all request come here")
    next() // move to next middleware
})
app.use("/product", (req,res/*, next no need here*/) => {
    res.send("<h1>product page</h1>")
})
app.use("/", (req,res/*, next no need here*/) => {
    res.send("<h1>home page</h1>")
})
```

`app.get()` `app.post()`  (Exact path, order of middleware none matter)

```js
app.use("/", (req,res,next) => {
    console.log("all request come here")
    next() // move to next middleware
})
app.post("/product", (req,res/*, next no need here*/) => {
    console.log(req.body)
    res.redirect("/");
})
app.get("/", (req,res/*, next no need here*/) => {
    res.send("<h1>home page</h1>")
})
```

![image-20200707204357826](assets/ExpressJS/image-20200707204357826.png)

## outsourcing routes

```js
// routes/admin.js
const express = require('express');

const router = express.Router();

router.get('/add-product', (req, res, next) => {
  res.send(
    '<form action="/product" method="POST"><input type="text" name="title"><button type="submit">Add Product</button></form>'
  );
});

router.post('/product', (req, res, next) => {
  console.log(req.body);
  res.redirect('/');
});

module.exports = router;
// routes/shop.js
const express = require('express');

const router = express.Router();

router.get('/', (req, res, next) => {
  res.send('<h1>Hello from Express!</h1>');
});

module.exports = router;
// app.js
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

const adminRoutes = require('./routes/admin');
const shopRoutes = require('./routes/shop');

app.use(bodyParser.urlencoded({extended: false}));

app.use(adminRoutes);
app.use(shopRoutes);

app.use((req,res,next) => {
    res.status(400).send('<h1>Page not found</h1>')
})
app.listen(3000);
```

## filter routes

```js
app.use("/admin", adminRoutes);
/*
	only /admin/add-product or /admin/product will be handled
*/
```

## 404 page

```js
//blah blah 
//end of all valid middleware
app.use((req,res,next) => {
    res.status(400).send('<h1>Page not found</h1>')
})
app.listen(3000);
```

# Authentication

![image-20200813010614484](assets/ExpressJS/image-20200813010614484.png)

![image-20200812224628329](assets/ExpressJS/image-20200812224628329.png)

![image-20200812224823331](assets/ExpressJS/image-20200812224823331.png)

## Encrypting password

### bcryptjs

![image-20200812234337195](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200812234337195.png)

![image-20200813000240286](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200813000240286.png)

## protected route

![image-20200813001657184](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200813001657184.png)

## Resetting password

create and store a reset token (with expire date) to verify that user need reset password

![image-20200814100700397](assets/ExpressJS/image-20200814100700397.png)

![image-20200814091933775](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814091933775.png)

![image-20200814091956370](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814091956370.png)

![image-20200814092023612](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814092023612.png)

![image-20200814092619924](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814092619924.png)

![image-20200814094853864](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814094853864.png)

![image-20200814095529043](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814095529043.png)

![image-20200814095608989](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814095608989.png)

# Authorization

restrict permission of a login user

![image-20200814100720306](assets/ExpressJS/image-20200814100720306.png)

![image-20200814100106063](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814100106063.png)

![image-20200814100314808](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814100314808.png)

![image-20200814100456001](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200814100456001.png)

# Validation

![image-20200814103128645](assets/ExpressJS/image-20200814103128645.png)

![image-20200814105028960](assets/ExpressJS/image-20200814105028960.png)

## Error message

https://stackoverflow.com/questions/47176945/difference-between-flash-connect-flash-and-express-flash

![image-20200813005839961](assets/ExpressJS/image-20200813005839961.png)

![image-20200813010018898](assets/ExpressJS/image-20200813010018898.png)

![image-20200813010055242](assets/ExpressJS/image-20200813010055242.png)

![image-20200813010418924](assets/ExpressJS/image-20200813010418924.png)

## express-validator

https://express-validator.github.io/docs/

https://github.com/chriso/validator.js

![image-20200814111412990](assets/ExpressJS/image-20200814111412990.png)

![image-20200814111455210](assets/ExpressJS/image-20200814111455210.png)

![image-20200814114906216](assets/ExpressJS/image-20200814114906216.png)

![image-20200814115048965](assets/ExpressJS/image-20200814115048965.png)

![image-20200814115134729](assets/ExpressJS/image-20200814115134729.png)

async validator

![image-20200814115342108](assets/ExpressJS/image-20200814115342108.png)

![image-20200814111625285](assets/ExpressJS/image-20200814111625285.png)

store old input

![image-20200814115546142](assets/ExpressJS/image-20200814115546142.png)

sanitizing data

![image-20200814120414983](assets/ExpressJS/image-20200814120414983.png)

# Error handling

https://expressjs.com/en/guide/error-handling.html

![image-20200814121953646](assets/ExpressJS/image-20200814121953646.png)

![image-20200814122113198](assets/ExpressJS/image-20200814122113198.png)

![image-20200814122220753](assets/ExpressJS/image-20200814122220753.png)

![image-20200814132010074](assets/ExpressJS/image-20200814132010074.png)

## Status

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

https://httpstatuses.com/

![image-20200814132024559](assets/ExpressJS/image-20200814132024559.png)

![image-20200814131621099](assets/ExpressJS/image-20200814131621099.png)

## Error page

![image-20200814124046840](assets/ExpressJS/image-20200814124046840.png)

![image-20200814124115063](assets/ExpressJS/image-20200814124115063.png)

![image-20200814124155985](assets/ExpressJS/image-20200814124155985.png)

![image-20200814124219656](assets/ExpressJS/image-20200814124219656.png)

## Error middleware

![image-20200814124355078](assets/ExpressJS/image-20200814124355078.png)

```js
(req,res,next) => {
    throw new Error("balh") // => will go to error middleware if throw sync place
    //promise.then(/*blah*/).catch(err => throw new Error(err)) // not working because in async
    promise.then(/*blah*/).catch(err => next(err)) // this will work
}
```

![image-20200814124443675](assets/ExpressJS/image-20200814124443675.png)

![image-20200814131159130](assets/ExpressJS/image-20200814131159130.png)

# Data sharing

persisting data across request

## request

```js
(res,req) => {
    req.someData = "some data"
    req.locals.viewData = "some data will be passed to the views"
    next()
}
```

## params

`/products/:id` (/product/123)

=> id = 123

```js
router.get("/products/:id",(res,req) => {
    const prodId = req.params.id
})
```

## query params

`/products/:id?edit=true`

```js
router.get("/products/:id",(res,req) => {
    const isEdit = req.query.edit
})
```

## cookie

https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

![image-20200811210557113](assets/ExpressJS/image-20200811210557113.png)

user can simply manipulating cookie value => use to store value to tracking users, advertisements tracking, not store sensitive data

can send to another page (**Third-party cookies**)

## sessions

https://www.quora.com/What-is-a-session-in-a-Web-Application

entry store in database or in memory of server

connect separate request of an user

![image-20200812105326281](assets/ExpressJS/image-20200812105326281.png)

=> data in session will be protected in server-side, cannot change in client-side, only for a client session

A client need to tell the server which sessions he belongs by send to server a hashed session id which is encrypted and only server can confirm that value is a certain ID in it's session store

## cookies vs sessions

![image-20200812213039239](assets/ExpressJS/image-20200812213039239.png)

# Serving file

![image-20200707204424034](assets/ExpressJS/image-20200707204424034.png)

## serving views

```js
// routes/shop.js
const path = require('path');

const express = require('express');

const router = express.Router();

router.get('/', (req, res, next) => {
	res.sendFile(path.join(__dirname,"..","views","shop.html")) // ./views/shop.html
});

module.exports = router;
// app.js
// blah blah 
app.use((req,res,next) => {
    res.status(400).send(path.join(__dirname,"views","404.html")) // ./views/404.html
})
app.listen(3000);
```

## static (public)

```js
//app.js
app.use(express.static(path.join(__dirname,"public")) //public => css, image, video, ... blah blah
```

```html
<link rel="stylesheet" href="/css/main.css"></link>
```

![image-20200815193904259](assets/ExpressJS/image-20200815193904259.png)

url.com/image.png

![image-20200815193934456](assets/ExpressJS/image-20200815193934456.png)

url.com/images/image.png

![image-20200815212412220](assets/ExpressJS/image-20200815212412220.png)

server relative path

![image-20200815195556861](assets/ExpressJS/image-20200815195556861.png)

## privately file

![image-20200815215544609](assets/ExpressJS/image-20200815215544609.png)

![image-20200815215613902](assets/ExpressJS/image-20200815215613902.png)

## readFile

![image-20200815220105060](assets/ExpressJS/image-20200815220105060.png)

check file access

![image-20200815220309899](assets/ExpressJS/image-20200815220309899.png)

## file stream

https://medium.freecodecamp.org/node-js-streams-everything-you-need-to-know-c9141306be93

we can pipe our readable stream, the file stream into the response and that means that the data will basically be downloaded by the browser step by step

if this is a large file, that will be a huge advantages because server never has to pre-loading file data to the memory but just **streams it to the client on the fly** and just has to store a **chunk** of date

here we don't wait for all the chunks to come together and concatenate them into one object, instead we forward them to the browser which then is also able to concatenate the incoming data pieces 

![image-20200815224506020](assets/ExpressJS/image-20200815224506020.png)

## pdf on the fly

### pdfkit

http://pdfkit.org/docs/getting_started.html

![image-20200815232119404](assets/ExpressJS/image-20200815232119404.png)

![image-20200815232450732](assets/ExpressJS/image-20200815232450732.png)

![image-20200815232822286](assets/ExpressJS/image-20200815232822286.png)



# File upload

https://github.com/expressjs/multer

![image-20200814234130182](assets/ExpressJS/image-20200814234130182.png)

## multer

parse multi part (file, text, ... mixed ) data of request 

![image-20200815001540768](assets/ExpressJS/image-20200815001540768.png)

![image-20200815002111940](assets/ExpressJS/image-20200815002111940.png)

test multer

![image-20200815002205472](assets/ExpressJS/image-20200815002205472.png)

![image-20200815002657060](assets/ExpressJS/image-20200815002657060.png)

![image-20200815003233224](assets/ExpressJS/image-20200815003233224.png)

![image-20200815003326539](assets/ExpressJS/image-20200815003326539.png)

best practice setup

![image-20200815011021429](assets/ExpressJS/image-20200815011021429.png)

filter

![image-20200815121406351](assets/ExpressJS/image-20200815121406351.png)

![image-20200815121434213](assets/ExpressJS/image-20200815121434213.png)

handler file data

![image-20200815192228980](assets/ExpressJS/image-20200815192228980.png)

![image-20200815192200909](assets/ExpressJS/image-20200815192200909.png)

## Image Names & Windows

One **important** note for **Windows users only**:

On Windows, the file name that includes a date string is not really supported and will lead to some strange CORS errors. Adjust your code like this to avoid such errors:

Instead of

```js
const storage = multer.diskStorage({    
    destination: function(req, file, cb) {        
        cb(null, 'images');    
    },    
    filename: function(req, file, cb) {        
        cb(null, new Date().toISOString() + file.originalname);    
    }
});
```

which we'll write in the next lecture, you should use this slightly modified version:

```js
const uuidv4 = require('uuid/v4') 
const storage = multer.diskStorage({    
    destination: function(req, file, cb) {        
        cb(null, 'images');    
    },    
    filename: function(req, file, cb) {        
        cb(null, uuidv4())    
    }
});
```

To ensure that images can be loaded correctly on the frontend, you should also change the logic in the `feed.js` controller:

```js
exports.createPost = (req, res, next) => {    
    //...    
    const imageUrl = req.file.path.replace("\\" ,"/");    
    //...
}
```

On macOS and Linux, you can ignore that and stick to the code I show in the videos.

# Email

https://sendgrid.com/docs/

![image-20200814010528712](assets/ExpressJS/image-20200814010528712.png)

## nodemailer

https://nodemailer.com/about/

![image-20200814010822952](assets/ExpressJS/image-20200814010822952.png)

### nodemailer-sendgrid-transport

![image-20200814010834456](assets/ExpressJS/image-20200814010834456.png)

![image-20200814010937421](assets/ExpressJS/image-20200814010937421.png)

![image-20200814011140212](assets/ExpressJS/image-20200814011140212.png)

# pagination

![image-20200816001439705](assets/ExpressJS/image-20200816001439705.png)

## mongoose

![image-20200816001318232](assets/ExpressJS/image-20200816001318232.png)

![image-20200816144220535](assets/ExpressJS/image-20200816144220535.png)

## sequelize

https://bezkoder.com/node-js-sequelize-pagination-mysql/

https://stackoverflow.com/questions/38211170/sequelize-pagination

## sql

https://stackoverflow.com/questions/3799193/mysql-data-best-way-to-implement-paging

## prepare data

![image-20200816001053478](assets/ExpressJS/image-20200816001053478.png)

## front-end

![image-20200816003441957](assets/ExpressJS/image-20200816003441957.png)

# Security

![image-20200813010636833](assets/ExpressJS/image-20200813010636833.png)



## CSRF

https://www.acunetix.com/websitesecurity/csrf-attacks/

CSRF ( Cross Site Request Forgery) abuse server session and trick user execute malicious code. 

=> Protect: only let session available on your views

![image-20200611220835360](assets/ExpressJS/image-20200611220835360.png)

### csurf

![image-20200813004056555](assets/ExpressJS/image-20200813004056555.png)

![image-20200813004133283](assets/ExpressJS/image-20200813004133283.png)

![image-20200813004148392](assets/ExpressJS/image-20200813004148392.png)

=> this package will look for the existence of a csrf token in your views, in your request body

![image-20200813004536651](assets/ExpressJS/image-20200813004536651.png)

![image-20200813005323466](assets/ExpressJS/image-20200813005323466.png)

![image-20200813004716969](assets/ExpressJS/image-20200813004716969.png)

# payment

![image-20200816011057562](assets/ExpressJS/image-20200816011057562.png)

## Stripe

https://stripe.com/docs

![image-20200816011155968](assets/ExpressJS/image-20200816011155968.png)

![image-20200816011418657](assets/ExpressJS/image-20200816011418657.png)

![image-20200816011445849](assets/ExpressJS/image-20200816011445849.png)

sever

![image-20200816012643477](assets/ExpressJS/image-20200816012643477.png)

![image-20200816011850267](assets/ExpressJS/image-20200816011850267.png)

![image-20200816012111889](assets/ExpressJS/image-20200816012418311.png)

![image-20200816012437213](assets/ExpressJS/image-20200816012437213.png)

client

![image-20200816012508696](assets/ExpressJS/image-20200816012508696.png)



# Best practice

## rootDir

```js
const path = require('path');
module.exports = path.dirname(process.mainModule.filename) // app.js
```







