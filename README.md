# marketing-resource

8ndpoint team marketing stuff.

## Shopify Google tag manager tracking code

### How to setting on shopify

1. go to shopify edit theme. Create a file on `Snippet` category and paste `shopify-theme.liquid`. Update your google tag manager tracking code.

- shopify-theme.liquid

```
 var gtmCode = '${Your gtm code}';
 ...
```

And import this snippets on `theme.liquid`.

- theme.liquid

```
      ...

      {% include 'MB_GA4_dataLayer' %}
      ...
```

2. go to `setting/Checkout`. Paste the `shopify-checkout.liquid` code on `Order status page` section. Update your google tag manager tracking code.

- shopify-theme.liquid

```
 var gtmCode = '${Your gtm code}';
 ...
```

3. view your shopify store page. open your devTool to check whether `dataLayer` correctly generate.

### what the dataLayer we push

1.  user first time visit the page. ( expire in 1 day )

```
{
  'pageType': 'Landing',
  'event': 'first_time_visitor'
}
```

2. user account status

- logout dataLayer

```
{
  "logState": "Logged Out",
  "firstLog": false,
  "customerEmail": null,
  "timestamp": "Fri Dec 09 2022 16:08:21 GMT+0800 ",
  "customerType": "New",
  "customerTypeNumber": "1",
  "shippingInfo": {
    "fullName": null,
    "firstName": null,
    "lastName": null,
    "address1": null,
    "address2": null,
    "street": null,
    "city": null,
    "province": null,
    "zip": null,
    "country": null,
    "phone": null
  },
  "billingInfo": {
    "fullName": null,
    "firstName": null,
    "lastName": null,
    "address1": null,
    "address2": null,
    "street": null,
    "city": null,
    "province": null,
    "zip": null,
    "country": null,
    "phone": null
  },
  "checkoutEmail": null,
  "currency": "USD",
  "pageType": "Log State",
  "event": "logState"
}
```

- login.

`firstLog` will be updated to `true` when user first time login. `customerType` and `customerTypeNumber` will be dynamic depend on `orders_count`.

```
{% if customer.orders_count > 2 %}
  'customerType'       : 'Returning',
  'customerTypeNumber' : '0',
{% else %}
  'customerType'       : 'New',
  'customerTypeNumber' :'1',
```

- login's dataLayer

```
{
  "userId": 111111111111,
  "customerEmail": "hello@mobagel.com",
  "logState": "Logged In",
  "customerInfo": {
    "firstName": null,
    "lastName": null,
    "address1": null,
    "address2": null,
    "street": null,
    "city": null,
    "province": null,
    "zip": null,
    "country": null,
    "phone": null,
    "totalOrders": 0,
    "totalSpent": "0.00"
  },
  "firstLog": false,
  "timestamp": "Fri Dec 09 2022 16:02:37 GMT+0800 ",
  "customerType": "New",
  "customerTypeNumber": "1",
  "shippingInfo": {
    "fullName": null,
    "firstName": null,
    "lastName": null,
    "address1": null,
    "address2": null,
    "street": null,
    "city": null,
    "province": null,
    "zip": null,
    "country": null,
    "phone": null
  },
  "billingInfo": {
    "fullName": null,
    "firstName": null,
    "lastName": null,
    "address1": null,
    "address2": null,
    "street": null,
    "city": null,
    "province": null,
    "zip": null,
    "country": null,
    "phone": null
  },
  "checkoutEmail": null,
  "currency": "USD",
  "pageType": "Log State",
  "event": "logState"
}
```

3. View page event.

- visit homepage (pathname equal '/')

```
{
  'pageType': 'Homepage',
  'event': 'homepage',
  logState // user account status
};
```

- visit 404 page

```
{
  'event': '404',
  'page': window.location.pathname
}
```

- visit article page

```
{
  'author': 'hello',
  'title': 'hello',
  'dateCreated': "Fri Dec 09 2022 16:02:37 GMT+0800 ",
  'pageType': 'Blog',
  'event': 'blog'
}
```

- visit product list page

```
{
  "productList": "Products",
  "pageType": "Collection",
  "event": "view_item_list",
  "ecommerce": {
    "items": [
      {
        "item_id": 123213213213,
        "item_variant": null,
        "item_name": "xxx",
        "price": "1.00",
        "item_brand": "My Store",
        "item_category": "",
        "item_list_name": "Products",
        "imageURL": "https://cdn.shopify.com/shopifycloud/shopify/assets/xxxxxxx.gif",
        "productURL": "https://xxxxx.in/products/test",
        "sku": null
      },
    ]
  }
}
```

- visit product page

```
{
  "pageType": "Product",
  "event": "view_item",
  "ecommerce": {
    "items": [
      {
        "item_id": 123213213213,
        "item_variant": null,
        "item_name": "test",
        "price": "1.00",
        "item_brand": "My Store",
        "item_category": "",
        "item_list_name": null,
        "description": "test",
        "imageURL": "https://cdn.shopify.com/shopifycloud/shopify/assets/xxxxxxxxxxx.gif",
        "productURL": "/products/xxxxx"
      }
    ]
  }
}
```

- visit cart page

```
{
  "pageType": "Cart",
  "event": "view_cart",
  "ecommerce": {
    "currency": "USD",
    "value": 1,
    "items": [
      {
        "item_id": 21312312312213,
        "item_variant": "Default Title",
        "item_name": "test",
        "price": "1.00",
        "item_brand": "My Store",
        "item_category": "",
        "item_list_name": null,
        "quantity": 1,
        "discount": null
      }
    ]
  }
}
```

- visit search page

```
{
  "pageType": "Search",
  "search_term": "123123",
  "event": "search",
  "item_list_name": null,
  "ecommerce": {
    "items": [{
      "item_id": 21312312312213,
      "item_variant": "Default Title",
      "item_name": "test",
      "price": "1.00",
      "item_brand": "My Store",
      "item_category": "",
      "item_list_name": null,
      "quantity": 1,
      "discount": null
    }]
  }
}
```

4. user interaction event

- add to cart

```
{
  "event": "add_to_cart",
  "ecommerce": {
    "items": [
      {
        "item_id": "123123213123",
        "item_name": "test",
        "item_brand": "My Store",
        "item_category": "Home page",
        "item_variant": "",
        "currency": "USD",
        "price": "1.0"
      }
    ]
  }
}
```

- remove cart

```
{
  'pageType': "Remove from cart",
  'event': 'remove_from_cart',
  "ecommerce": {
    "items": [
      {
        "item_id": "123123213123",
        "item_name": "test",
        "item_brand": "My Store",
        "item_category": "Home page",
        "item_variant": "",
        "currency": "USD",
        "price": "1.0"
      }
    ]
  }
}
```

- begin checkout

```
{
  'event': 'begin_checkout',
  'pageType': 'Customer Information',
  'step': 1,
  "ecommerce": {
    "items": [
      {
        "item_id": "123123213123",
        "item_name": "test",
        "item_brand": "My Store",
        "item_category": "Home page",
        "item_variant": "",
        "currency": "USD",
        "price": "1.0"
      }
    ]
  }
}
```

- add shipping info

```
{
  'event': 'add_shipping_info',
  'pageType': 'Shipping Information',
  "ecommerce": {
    "items": [
      {
        "item_id": "123123213123",
        "item_name": "test",
        "item_brand": "My Store",
        "item_category": "Home page",
        "item_variant": "",
        "currency": "USD",
        "price": "1.0"
      }
    ]
  }
)
```

- add payment info

```

 {
  'event': 'add_payment_info',
  'pageType': 'Add Payment Info',
  "ecommerce": {
    "items": [
      {
        "item_id": "123123213123",
        "item_name": "test",
        "item_brand": "My Store",
        "item_category": "Home page",
        "item_variant": "",
        "currency": "USD",
        "price": "1.0"
      }
    ]
  }
}

```

- finish checkout

```
{
  'pageType': 'Transaction',
  'event': 'purchase',
  "ecommerce": {
    "items": [
      {
        "item_id": "123123213123",
        "item_name": "test",
        "item_brand": "My Store",
        "item_category": "Home page",
        "item_variant": "",
        "currency": "USD",
        "price": "1.0"
      }
    ]
  }
}
```
