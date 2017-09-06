title: BeerJS Theme
author:
  name: Jorge Epuñan H.
  twitter: beerjssantiago
  url: http://www.beerjs.cl
output: presentacion.html
theme: juanbrujo/cleaver-beerjs

--

# VueJS: Simple Carro de Compra

## BeerJS Santiago - Septiembre 2017

## [repo](https://github.com/juanbrujo/beerjs-sept2017) | [demo](https://juanbrujo.github.io/beerjs-sept2017/)

--

### JavaScript:

```js
data: {
  title:  'Simple Carro de Compra con VueJS',
  total:  0,
  items:  [],
  cart:   [],
  price:  9.990,
  image:  'https://unsplash.it/200'
}
```

--

### JavaScript:

```js
filters: {
  currency: function (price) {
    return '$'.concat(price.toFixed(3));
  }
}
```

--

### JavaScript:

```js
created: function () {
  axios.get('http://fakerestapi.azurewebsites.net/api/Books').then(response => {
    this.items = response.data;
  });
}
```
--

### JavaScript: methods

```js
addItem: function (index) {
  var item = this.items[index];
  this.total += this.price;
  var found = false;
  for (var i = 0; i < this.cart.length; i++) {
    if (this.cart[i].id === item.ID) {
      found = true;
      this.cart[i].qty++;
      break;
    }
  }
  if (!found) {
    this.cart.push({
      id: item.ID,
      title: item.Title,
      qty: 1,
      price: this.price
    });
  }
},
```

--

### JavaScript: methods

```js
increment: function (item) {
  item.qty++;
  this.total += item.price;
},
decrement: function (item) {
  item.qty--;
  this.total -= this.price;
  if (item.qty <= 0) {
    for (var i = 0; i < this.cart.length; i++) {
      if(this.cart[i].id === item.id) {
        this.cart.splice(i, 1);
        break;
      }
    }
  }
}
```

--

### Markup:

```html
<div class="product" v-for="(item, index) in items">
  <div class="product-image">
      <img :src="image">
  </div>
  <h4 class="product-title">{{ item.Title }}</h4>
  <p>Precio: {{ price | currency }}</p>
  <button class="btn add-to-cart" v-on:click="addItem(index)">Agregar al carro</button>
</div>
```

--

### Markup:

```html
<ul class="cart">
  <li class="cart-item" v-for="item in cart">
    <div class="item-title">
        {{ item.title }}
    </div>
    <span class="item-qty">{{ item.price | currency }} x {{ item.qty }}</span>
    <button class="btn" v-on:click="increment(item)">+</button>
    <button class="btn" v-on:click="decrement(item)">-</button>
  </li>
</ul>
```

--

### Markup:

```html
<div>
  <div v-if="cart.length">
    <div>Total: {{ total | currency }}</div>
  </div>
</div>
<div class="empty-cart" v-if="cart.length === 0">
  Carro vacío. ¿Por qué no compras algo?
</div>
```
