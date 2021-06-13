http://docs.sequelizejs.com/

# Squelize

![image-20200718212728810](assets/Sequelize/image-20200718212728810.png)

![image-20200718212801892](assets/Sequelize/image-20200718212801892.png)

# Connect

![image-20200718213031244](assets/Sequelize/image-20200718213031244.png)

```js
const Sequelize = require('sequelize');

const sequelize = new Sequelize('node-complete', 'root', 'nodecomplete', {
  dialect: 'mysql',
  host: 'localhost'
});

module.exports = sequelize;
```

# Sync Sequelize definition to the database

```js
const sequelize = require('../util/database');
//if in production force should bt false
sequelize.sync({ force: true }).then(db => {
    console.log(db)
    app.listen(3000);
}).catch(err => {
    console.log(err)
})
```

# Models

```js
const Sequelize = require('sequelize');

const sequelize = require('../util/database');

const Product = sequelize.define('product', {
  id: {
    type: Sequelize.INTEGER,
    autoIncrement: true,
    allowNull: false,
    primaryKey: true
  },
  title: Sequelize.STRING,
  price: {
    type: Sequelize.DOUBLE,
    allowNull: false
  },
  imageUrl: {
    type: Sequelize.STRING,
    allowNull: false
  },
  description: {
    type: Sequelize.STRING,
    allowNull: false
  }
});

module.exports = Product;
```

## create

```js
// import Product
Product.create({
    title,
    price,
    imageUrl,
    description,
}).then(product => console.log(product)).catch(err => console.log(err))
```

# find

```js
Product.findAll({where}).then(product => console.log(product)).catch(err => console.log(err)) //Array
Product.findOne({where}).then(product => console.log(product)).catch(err => console.log(err))
Product.findByPk(pk,options).then(product => console.log(product)).catch(err => console.log(err))
```

# update

```js
Product.findByPk(pk,options)
    .then(product => {
    	product.title = "NEW TITLE"
    	return product.save()
	})
	.then(res => {
    	console.log("UPDATED")
	})
    .catch(err => console.log(err))
```

# destroy (deleted)

```js
Product.findByPk(pk,options)
    .then(product => {
    	return product.destroy();
	})
	.then(res => {
    	console.log("DELETED")
	})
    .catch(err => console.log(err))
```

# Association / Relation

![image-20200718232834555](assets/Sequelize/image-20200718232834555.png)

```js
const sequelize = require('../util/database');
// import Product, User
// Add relation here
sequelize.sync().then(db => {
    console.log(db)
    app.listen(3000);
}).catch(err => {
    console.log(err)
})
```

## One-To-Many

```js
Product.belongsTo(User, {//can have options in an Association 
    constraints: true,
    onDelete: 'CASADE' //delete when user delete
})
User.hasMany(Product)
let user
User.findByPk(id).then(u => {
    user = u
})
// => Default create an userId field in product table which is a FK refs the ID field in the users table
// Create
Product.create({
    title,
    price,
    imageUrl,
    description,
    userId: user.id
}).then(product => console.log(product)).catch(err => console.log(err))
//or
user.createProduct({//createProduct auto created by sequelize if have an association 
    title,
    price,
    imageUrl,
    description,
}).then(product => console.log(product)).catch(err => console.log(err))
// Find related product
user.getProducts().then(products => console.log(products)).catch(err => console.log(err))

// User vs Order
User.hasMany(Order)
Order.belongsTo(User)
```

## One-To-One

```js
User.hasOne(Cart)
Cart.belongsTo(User) 
// One direction is enough
// => Add userId to cart
user.createCart().then(cart => console.log(cart)).catch(err => console.log(err))
user.getCart().then(cart => console.log(cart)).catch(err => console.log(err))
```

## Many-To-Many

```js
Cart.belongsToMany(Product, {through: CartProduct})
Product.belongsToMany(Cart, {through: CartProduct})	
//=> create CartItem table to make the Many-To-Many relation
// get Products
user.getCart().then(cart => {
    cart.getProducts(products => console.log(products))
}).catch(err => console.log(err))

// create Products item in Cart
let fetchedCart;
let quantity = 1;
user.getCart()
    .then(cart => {
        fetchedCart = cart
        return cart.getProducts({
            where: {productId}
        })
    })
	.then(products => {
        let product = products.length ? products[0] : null;
        if(product) {
            // get existing cart product item
            quantity = product.cartProduct.quantity + 1
            return product
        }
        return Product.findByPk(productId)
    })
	.then(product => {
		return fetchedCart.addProduct(//addProduct auto created by sequelize if have a Many-To-Many association 
            product,
            {through: {quantity}}
        )
    })
	.then(cart => console.log(cart))
    .catch(err => console.log(err))
// delete Products item in Cart
user.getCart()
    .then(cart => {
        return cart.getProducts({
            where: {id}
        })
    })
	.then([product] => {
    	return product.cartProduct.destroy()
    })
	.catch(err => console.log(err))

// Order vs Product
Order.belongsToMany(Product, {through: OrderProduct}) 
// create Products item as Orders item of User
let fetchedCart;
user.getCart()
    .then(cart => {
    	fetchedCart = cart
        return cart.getProducts()
    })
	.then(products => {
    	return user.createOrder()
    			.then(order => {
                    return order.addProducts(products.map(product => {
                        product.OrderProduct.quantity = { quantity: product.cartItem.quantity };
                        return product
                    }))
                })
	})
	.then(res => {
    	return fetchedCart.setProducts(null) // clear Cart
	})
	.then(res => {
    	console.log(res)
	})
	.catch(err => console.log(err))
// get Orders item on orders of users
user.getOrders({include: ['products']})
	.then(orders => {
    	orders.map(order => {
            console.log(order.id)
            order.products.forEach(product => {
                console.log(product.OrderItem)
            })
        })
	})
	.catch(err => console.log(err))
```

