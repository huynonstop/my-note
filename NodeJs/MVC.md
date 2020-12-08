# MVC

https://developer.mozilla.org/en-US/docs/Glossary/MVC

![image-20200710222322856](assets/MVC/image-20200710222322856.png)

# Template engine

![image-20200710205108632](assets/MVC/image-20200710205108632.png)

dynamic HTML page create by server data on the fly

![image-20200710205253101](assets/MVC/image-20200710205253101.png)

## installing

`npm isntall --save ejs pug express-handlebars`

## implementing

```js
//app.js
//pug
app.set('views engine', 'pug');
app.set('views', 'views');
//express-handlebars
const expressHbs = require('express-handlebars');
app.engine('hbs',expressHbs({
    layoutsDir: "views/layout",
    defaultLayout: "main",
    extname: "hbs"
}));
app.set('views engine', 'hbs');
app.set('views', 'views');
//ejs
app.set('views engine', 'ejs');
app.set('views', 'views');
```

## dynamic content

```js
//controler.js
const products = [/*blah blah*/]
const controller = (req,res) => {
    res.render('shop', { products, title: 'shop', path: "/", hasProduct: products.length > 0}) // ./views/shop.pug (hbs, ejs) 
}
```

### pug

https://pugjs.org/api/getting-started.html

```jade
head
	title #{title}
body
	main
		if products.length > 0 
            each product in products
                div.product 
                    div.title #{product.title}
          else
          	h1 no product
```

layout

```jade
//./views/layout/main.pug
<!DOCTYPE html>
html(lang="en")
	head
		title #{title}
		block styles
	body
        header
            nav
                ul
                    li
                        a(herf="/", class=(path === "/" ? 'active' : ""))
                    li
                        a(herf="/products", class=(path === "/products" ? 'active' : ""))
		block content
		
//./views/404.pug
extends layout/main.pug
block content
	h1 Page not found
```

### handlebars

no logic inside template => logic in the controller

```handlebars
<!DOCTYPE html>
<html>
	<head>
    	<title>{{ title }}</title>
    </head>   
 	<body>
        {{#if hasProduct}}
        	{{#each products}}
        		<div class="product">
        			<div class="title">
        				{{ this.title }}
        			</div>
        		</div>
        	{{/each}}
        {{else}}
        	<h1>no product</h1>
        {{/if}}	
    </body>
</html>
```

layout

```handlebars
//./views/layout/main.hbs
<!DOCTYPE html>
<html>
	<head>
    	<title>{{ title }}</title>
    	{{#if someCSS}}
    		<link rel="stylesheet" href="/css/some.css">
    	{{/if}}
    </head>   
 	<body>
 		<header>
 			<ul>
 			   <li><a class="{{#if activeShop}}active{{/if}}" href="/">Home</a></li>
             	<li><a class="{{#if activeProduct}}active{{/if}}" href="/product">Product</a></li>
 			</ul>
 		</header>
        {{{ body }}}
    </body>
</html>
//./views/produt.hbs auto use default layout
<main>
	{{#if hasProduct}}
        {{#each products}}
        	<div class="product">
        		<div class="title">
        			{{ this.title }}
        		</div>
        	</div>
        {{/each}}
	{{else}}
        <h1>no product</h1>
	{{/if}}	
</main>
```

### ejs

```ejs
<!DOCTYPE html>
<html>
	<head>
    	<title><%= title %></title>
    </head>   
 	<body>
        <%= if (products.length > 0) { %>
        	<%= for(let product of products) %>
        		<div class="product">
        			<div class="title">
        				<%= product.title %> 
        			</div>
        		</div>
        	<%= } %>
        <%= } else { %>
            <h1>no product</h1>
       	<%= } %>
    </body>
</html>
```

include

```ejs
//./views/include/header.ejs
<header>
	<ul>
		<li><a class="<%= path === "/" ? 'active' : "" %>" href="/">Home</a></li>
		<li><a class="<%= path === "/product" ? 'active' : "" %>" href="/product">Product</a></li>
	</ul>
</header>
//./views/includes/head.ejs
<!DOCTYPE html>
<html>
	<head>
    	<title><%= title %></title>
        
//./views/404.ejs
<%- include('includes/head.ejs') %>
    </head>
    <body>
        <%- include('includes/header.ejs') %>
        <h1>
            page not found
        </h1>
    </body>
</html>
```

![image-20200816003311147](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816003311147.png)

## request

```js
(res,req) => {
    req.someData = "some data"
    req.locals.viewData = "some data will be passed to the views"
    next()
}
```

# cookie

https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

![image-20200811210557113](assets/MVC/image-20200811210557113.png)

user can simply manipulating cookie value => use to store value to tracking users, advertisements tracking, not store sensitive data

can send to another page (**Third-party cookies**)

expired-in

max-age (default is a session)

secured

httpOnly

## set

![image-20200811224940148](assets/MVC/image-20200811224940148.png)

## get

![image-20200811225023175](assets/MVC/image-20200811225023175.png)

# sessions

https://www.quora.com/What-is-a-session-in-a-Web-Application

entry store in database or in memory of server

connect separate request of an user

![image-20200812105326281](assets/MVC/image-20200812105326281.png)

=> data in session will be protected in server-side, cannot change in client-side, only for a client session

A client need to tell the server which sessions he belongs by send to server a hashed session id which is encrypted and only server can confirm that value is a certain ID in it's session store

## express-session

https://github.com/expressjs/session

![image-20200812172200159](assets/MVC/image-20200812172200159.png)

![image-20200812212938371](assets/MVC/image-20200812212938371.png)

![image-20200812172817484](assets/MVC/image-20200812172817484.png)

## connect-mongodb-session

![image-20200812175050327](assets/MVC/image-20200812175050327.png)

![image-20200812175255496](assets/MVC/image-20200812175255496.png)

![image-20200812175306686](assets/MVC/image-20200812175306686.png)

## delete cookie

![image-20200812191259969](assets/MVC/image-20200812191259969.png)

# cookies vs sessions

![image-20200812213039239](assets/MVC/image-20200812213039239.png)