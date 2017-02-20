---
title: API Reference


includes:
  - errors

search: true
---

# Introduction

Welcome to the Floupr API! You can use our API to access Floupr API endpoints, which can get information on various entities in our database.

We have language bindings in Ruby. 

<strong>Every API for this project have to be prefixed with "https://radiant-island-87705.herokuapp.com" and have to include "Auth-Token" and "Email" as headres once the user is login.</strong>


# Authentication

## Sign Up

> Sample Request:

```json
{
  "users": {
    "email": "someoneanonymousiam@gmail.com",
    "password": "1234567890",
    "role": "buyer",
    "device_id": "12566525"
  }
}

```

> Sample response when the user register first time and email is not verified :

```json
{
  "status": true,
  "message": "Signed up successfully",
  "confirmed": false
}

```
> Sample response when the user register using another role and email is verified :

```json
{
  "status": true,
  "message": "Signed up successfully",
  "confirmed": true
}

```

> Sample response when register with another role using different password :

```json
{
  "status": false,
  "message": "Please use same password as for your buyer/seller account"
}

```

> Sample response when the user already registerd with both buyer/seller or with the existing role :

```json
{
  "status": false,
  "message": "User already exists",
  "confirmed": true
}

```

This endpoint register the user using email and password.

### HTTP Request

`POST /users`

### Query Parameters

Parameter | Mandatory | Description
--------- | ------- | -----------
email | yes | user email
password | yes | User password (min length 8)
role | yes | role as user wants to regiter with
device_id| yes| device id of the user's device 

<aside class="success">
Remember — Role of user can be "buyer" / "supplier" / "both"
</aside>

## Add Role

> Sample Request:

```json
{
    "users": {
        "role": "seller"
    }
}

```

> Sample reponse when user successfully registed with new role:

```json

{
  "status": true,
  "message": "User already exists",
  "confirmed": true
}

```


This end point add role for user when user is login and signup with other role.


### HTTP Request

`POST /users/add_role`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
user_id | yes | the id of user current login
role | yes | role as user wants to regiter with

<aside class="success">
Remember — Role of user can be "buyer" / "supplier" / "both"
</aside>


## Login

> Sample Request:

```json
{
    "users": {
        "email": "someoneanonymousiam@gmail.com",
        "password": "1234567890"
    }
}


```

> Sample reponse when user email is not verified yet:

```json

{
  "status": false,
  "message": "Please confirm the email to login"
}

```


> Sample reponse when user login successfully:

```json

{
  "user_id": 21,
  "auth_token": "IoKnPvIPlnVuX1_hVK6Rcg==",
  "status": true,
  "message": "signed up successfully"
}

```

> Sample reponse when invalid email or password:

```json

{
  "status": false,
  "message": "invalid email or password"
}

```


This end point login the user.


### HTTP Request

`POST /users/sign_in`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
email | yes | email of the user to login
password | yes | password of thr user to login

<aside class="success">
Remember — Set "Auth-Token" and "email" as header after user login successfully
</aside>

## Logout

> Sample Request:

```json
{
  
}

```

> Sample reponse when user logout successfully:

```json
{
  "status": true,
  "message": "sign out succesfully"
}
```



This end point logout the user.


### HTTP Request

`DELETE /users/sign_out`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
user_id | yes | id of user currently login


<aside class="notice">
No parameters required for this api. Only use auth_token in header
</aside>



## Forgot Password

> Sample Request:

```json
{
    "users": {
        "email": "someoneanonymousiam@gmail.com"
    }
}



```

> Sample reponse when reset password email sent:

```json
{
  "status": true,
  "message": "reset password email sent"
}

```

> Sample reponse when user does not exists:

```json
{
  "status": false,
  "message": "user does not exists"
}

```



This end point send email to user with reset password instructions.


### HTTP Request

`POST /users/password`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
email | yes | email of the user 


## Reset Password

> Sample Request:

```json

  {
      "users": {
          "password": "newPass",
          "token": "8968"
      }
  }

```

> Sample reponse when password update successfully:

```json
  {
    "status": true,
    "message": "password updated successfully"
  }

```

> Sample reponse when invalid token (otp):

```json
  {
    "status": false,
    "message": "invalid token"
  }

```



This end point reset the new password for the user.


### HTTP Request

`PUT /users/password`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
password | yes | new password for the user
token | yes | otp/token sent in email for reset password instructions

<aside class="success">
Remember — Once password updates successfully using otp, that otp expires.
</aside>


## Resend confirmation instructions

> Sample Request:

```json

{
  "users": {
    "email": "someoneanonymousiam@gmail.com"
  }
}

```

> Sample reponse when email sucessfully sent:

```json
  {
    "status": true,
    "message": "confirmation instructions email sent"
  }

```

> Sample reponse when invalid email:

```json
  {
    "status": false,
    "message": "user doe not exists"
  }

```



This end point sends the email to user with email confirmations instructions.


### HTTP Request

`POST /users/confirmation`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
email | yes | user's email



## facebook signup/sign in

> Sample Request:

```json

{
 "users" : {
    "name": "deepti sharma",
    "provider": "facebook",
    "uid": "7868374658734",
    "email": "sharma.deepti369@gmail.com",
    "profile_pic": "https://www.facebook.com/photo.php?fbid=1704207913224014&set=a.1391569554487853.1073741826.100009044661363&type=3&theater"
 }
}

```

> Sample reponse when email sucessfully sent:

```json
 {
  "user_id": 19,
  "auth_token": "uciTFI-1KWsUzFs97Oj_vQ==",
  "status": true,
  "message": "signed up successfully"
}

```


This end point login or register user via facebook.


### HTTP Request

`POST /users/social_login`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
email | yes | user's email from facebook
name | yes | user's name
uid | yes | user's uid from facebook
provider| yes |facebook
profile_pic | no |facebook profile pic url of user 


# Profile

## Update profile

> Sample Request with fb profile_pic url:

```json
{
 "users" : {
    "name": "deepti sharma",
    "phone_number": "12353737978",
    "address": "sector 61, noida",
    "about": "test desc",
    "profile_pic": "https://www.facebook.com/photo.php?fbid=1704207913224014&set=a.1391569554487853.1073741826.100009044661363&type=3&theater",
    "social_accounts": {
      "twitter": false,
      "facebook":false,
      "instagram":false
    }
  }
 }


```


> Sample Request with image base64 encoding:

```json
{
 "users" : {
    "name": "deepti sharma",
    "phone_number": "12353737978",
    "address": "sector 61, noida",
    "about": "test desc",
    "profile_pic": "data:image/jpeg;base64, /9j/4AAQSkZJRgABAQEAYABgAAD/4QEMRXhpZgAATU0AKgAAAAgAAYdpAAQAAAABAAAAFgABkoYABwAAAOA............................",
    "social_accounts": {
      "twitter": false,
      "facebook":false,
      "instagram":false
    }
  }
 }


```

> Sample reponse:

```json
{
  "profile_data": {
    "name": "deepti sharma",
    "email": "someoneanonymousiam@gmail.com",
    "phone_number": "12353737978",
    "address": "sector 61, noida",
    "about": "test desc",
    "profile_pic": "https://radiant-island-87705.herokuapp.com/uploads/user/profile_pic/2/file.jpeg",
    "social_account": {
      "id": 1,
      "twitter": false,
      "facebook": false,
      "instagram": false
    }
  },
  "status": true,
  "message": "profile saved successfully"
}

```


This end point updates the profile of user.


### HTTP Request

`PUT /users/profile`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
name | yes | user's name
profile_pic | no |facebook profile pic url of user or base64
phone_number | no | phone number of uer
address | no | users's address
about | no | user's description
social_accounts| yes | user's social accounts
address | no |user,s address


## Update password

> Sample Request:

```json
{
 "users" : {
  "old_password": "1234567890",
  "new_password": "0987654321"
  }
 }


```


> Sample Response if password update successfully:

```json
{
  "status": true,
  "message": "Password updated successfully"
}

```

> Sample Response if old password does not match:

```json
{
  "status": false,
  "message": "Old password does not match"
}

```



This end point updates password of user from profile.


### HTTP Request

`PUT /profile/update_password`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
old_password | yes | user's current password
new_password | yes | user's new password

# Credit Cards


## Add card

> Sample Request:

```json

  {
    "card": {
      "holder_name": "me",
      "card_number": "24228333126876",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa",
      "shipping_address": {
        "city": "mumbai",
        "address": "sector 60",
        "state": "U.P.",
        "country": "india",
        "zipcode": "12345"
      },
      "billing_address": {
        "city": "mumbai",
        "address": "sector 60",
        "state": "U.P.",
        "country": "india",
        "zipcode": "12345"
      }
    }
  }


```


> Sample Response if card added successfully:

```json
  {
    "status": true,
    "message": "Card added successfully",
    "card": {
      "id": 4,
      "holder_name": "me",
      "card_number": "24228333126876",
      "expiry_date": "09/21",
      "cvv": 754,
      "card_type": "visaa",
      "shipping_address": {
        "id": 2,
        "address": "sector 60",
        "city": "mumbai",
        "state": "U.P.",
        "zipcode": "12345",
        "country": "india"
      },
      "billing_address": {
        "id": 2,
        "address": "sector 60",
        "city": "mumbai",
        "state": "U.P.",
        "zipcode": "12345",
        "country": "india"
      }
    }
  }

```

> Sample Response if already card with same number exists:

```json
{
  "status": false,
  "message": "Card with this number already exists"
}

```



This end point add a credit card for user.


### HTTP Request

`POST /cards`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
holder_name | yes | credit card holder name
card_number | yes | credit card number
expiry_date | yes | credit card expiry date
cvv | yes | credit card cvv number
card_type | yes | credit card type
shipping_address | yes | card's shipping address
billing_address | yes | card's billing address

## Update card

> Sample Request:

```json

  {
    "card": {
      "id": 4,
      "holder_name": "me",
      "card_number": "24228333126876",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa",
      "shipping_address": {
        "city": "mumbai",
        "address": "sector 60",
        "state": "U.P.",
        "country": "india",
        "zipcode": "12345"
      },
      "billing_address": {
        "city": "mumbai",
        "address": "sector 60",
        "state": "U.P.",
        "country": "india",
        "zipcode": "12345"
      }
    }
  }


```


> Sample Response if card added successfully:

```json
  {
    "status": true,
    "message": "Card added successfully",
    "card": {
      "id": 4,
      "holder_name": "me",
      "card_number": "24228333126876",
      "expiry_date": "09/21",
      "cvv": 754,
      "card_type": "visaa",
      "shipping_address": {
        "id": 2,
        "address": "sector 60",
        "city": "mumbai",
        "state": "U.P.",
        "zipcode": "12345",
        "country": "india"
      },
      "billing_address": {
        "id": 2,
        "address": "sector 60",
        "city": "mumbai",
        "state": "U.P.",
        "zipcode": "12345",
        "country": "india"
      }
    }
  }

```

> Sample Response if already card with id does not exists:

```json
{
  "status": false,
  "message": "Card with this number does not exists"
}

```



This end point update a credit card for user.


### HTTP Request

`PUT /cards`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | credit card id
holder_name | yes | credit card holder name
card_number | yes | credit card number
expiry_date | yes | credit card expiry date
cvv | yes | credit card cvv number
card_type | yes | credit card type
shipping_address | yes | card's shipping address
billing_address | yes | card's billing address


## Delete card

> Sample Request:

```json

  {
    "card_id": 3
  }


```


> Sample Response if card deleted successfully:

```json
 {
  "status": true,
  "message": "Card deleted successfully"
}

```

> Sample Response if already card with id does not exists:

```json
{
  "status": false,
  "message": "Card with id number does not exists"
}

```



This end point delete a credit card for user.


### HTTP Request

`DELETE /card`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
card_id | yes | credit card id



# Products


## Create product

> Sample Request:

```json

  {
   "product" : {
      "name": "jacket-leather",
      "color": "green",
      "size": "s",
      "description": "test description",
      "images": ["data:image/jpeg;base64, /9j/4AAQSkZJRgABAQEAYABgAAD/4QEMRXhpZg...","data:image/jpeg;base64, /9j/4AAQSkZJRgABAQEAYABgAAD/4QEMRXhpZg..."],
      "category_id": 1,
      "sub_category_id":1,
      "brand_id":1,
      "auction_day": "25-10-17"
    }
   }



```


> Sample Response if card deleted successfully:

```json
  {
    "status": true,
    "message": "Product added successfully",
    "product": {
      "id": 7,
      "name": "jacket-leather",
      "color": "green",
      "size": "s",
      "description": "test description",
      "auction_day": "25-10-17",
      "category": {
        "id": 1,
        "name": "category 1"
      },
      "sub_category": {
        "id": 1,
        "name": "category 1-1"
      },
      "brand": {
        "id": 1,
        "name": "brand 1"
      },
      "product_images": [
        {
          "id": 1,
          "url": "/uploads/product_image/image/19/file.jpeg"
        }
      ]
    }
  }

```


This end point creates a product for user.


### HTTP Request

`POST /product`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
name | yes | name / title of product
color | yes | color of product
size | yes |size of product
description | yes | description of product
images | yes | images of product
category_id | yes | id of product's category
sub_category_id | yes | id of product's sub category
brand_id | yes | id of product's brand
auction_day| auction day for product


## Update product

> Sample Request:

```json

  {
   "product" : {
      "id": 1,
      "name": "jacket-leather",
      "color": "green",
      "size": "s",
      "description": "test description",
      "images": ["data:image/jpeg;base64, /9j/4AAQSkZJRgABAQEAYABgAAD/4QEMRXhpZg...","data:image/jpeg;base64, /9j/4AAQSkZJRgABAQEAYABgAAD/4QEMRXhpZg..."],
      "category_id": 1,
      "sub_category_id":1,
      "brand_id":1,
      "auction_day": "25-10-17",
      "max_price": 20.00
    }
   }



```


> Sample Response if card deleted successfully:

```json
  {
    "status": true,
    "message": "Product added successfully",
    "product": {
      "id": 1,
      "name": "jacket-leather",
      "color": "green",
      "size": "s",
      "description": "test description",
      "auction_day": "25-10-17",
      "max_price": 20.00,
      "category": {
        "id": 1,
        "name": "category 1"
      },
      "sub_category": {
        "id": 1,
        "name": "category 1-1"
      },
      "brand": {
        "id": 1,
        "name": "brand 1"
      },
      "product_images": [
        {
          "id": 1,
          "url": "/uploads/product_image/image/19/file.jpeg"
        }
      ]
    }
  }

```


This end point update a product for user.


### HTTP Request

`PUT /product`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | id of product
name | yes | name / title of product
color | yes | color of product
size | yes |size of product
description | yes | description of product
images | yes | images of product
category_id | yes | id of product's category
sub_category_id | yes | id of product's sub category
brand_id | yes | id of product's brand
auction_day| auction day for product


## Delete product

> Sample Request:

```json

  {
    "product_id" : 5588
  }

```


> Sample Response if product not found:

```json
  
  {
    "status": false,
    "message": "Product not found"
  }

```

> Sample Response if product deleted successfully:

```json
  
  {
    "status": true,
    "meaasge": "Product deleted successfully"
  }

```

This end point delete a product for user.


### HTTP Request

`DELETE /product`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product to delete


## Delete product's image

> Sample Request:

```json
  {
   "product_id" : 2,
   "image_id": 15
  }


```

> Sample Response if product deleted successfully:

```json
  
  {
    "status": true,
    "meaasge": "Image deleted successfully"
  }

```


> Sample Response if product not found:

```json
  
  {
    "status": false,
    "message": "Product not found"
  }

```



This end point delete a product for user.


### HTTP Request

`DELETE /product`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product to delete
image_id | yes | id of image to delete



# Order

## Adding product to cart

> Sample Request:

```json
  {
    "order": {
      "product_id": 3,
      "max_price": 20.03,
      "expires_at": "2", 
      "quantity": 4,
      "order_confirm": false
    }
  }

```

> Sample Response if product added to cart successfully:

```json
  {
   "status": true,
    "message": "Added to cart successfully"
  }

```


This end point ass a product to cart.


### HTTP Request

`POST /order`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product
max_price | yes | max price for product
expires_at | yes | Time by which user wants to wait for product's auction (in days)
quantity | yes | quantity of product
order_confirm | yes | false for products to be added in cart


## Ordering a single product

> Sample Request:

```json

  {
    "order": {
      "product_id": 3,
      "max_price": 20.03,
      "expires_at": "2", 
      "quantity": 4,
      "order_confirm": true
    }
  }

```

> Sample Response if product added to cart successfully:

```json
  {
    "status": true,
    "message": "Product ordered successfully"
  }

```


This end point add a product to cart.


### HTTP Request

`POST /order`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product
max_price | yes | max price for product
expires_at | yes | Time by which user wants to wait for product's auction (in days)
quantity | yes | quantity of product
order_confirm | yes | true 

## Placing order form cart

> Sample Request:

```json

{
  "orders":{
    "order1": {
      "id": 1,
      "product_id": 3,
      "max_price": 20.03,
      "expires_at": "2", 
      "quantity": 4,
      "order_confirm": true
    },
    "order2": {
      "id": 2,
      "product_id": 3,
      "max_price": 20.03,
      "expires_at": "2", 
      "quantity": 4,
      "order_confirm": true
    },
    "order3": {     
      "id": 3,
      "product_id": 3,
      "max_price": 20.03,
      "expires_at": "2", 
      "quantity": 4,
      "order_confirm": true
    }
  }
}

```

> Sample Response if order placed successfully:

```json
  {
    "status": true,
    "message": "Order placed successfully"
  }

```


This end point add a product to cart.


### HTTP Request

`PUT /order`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | id of order
product_id | yes | id of product
max_price | yes | max price for product
expires_at | yes | Time by which user wants to wait for product's auction (in days)
quantity | yes | quantity of product
order_confirm | yes | true 


## Delete product from cart

> Sample Request:

```json
  {
    "order_id": 2
  }

```

> Sample Response if product deleted successfully:

```json
  
  {
    "status": true,
    "message": "Order deleted from cart"
  }
```


> Sample Response if product not found in cart:

```json
  
  {
    "status": false,
    "message": "Order with this id not found"
  }

```



This end point delete a product for user.


### HTTP Request

`DELETE /product`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
order_id | yes | id of order to delete from cart


# Deals

## Get buyer deals


> Sample Request:

```json
  {
    
  }

```

> Sample Response if product deleted successfully:

```json
  {
  "status": true,
  "deals": [
    {
      "id": 19,
      "name": "T-shirt yellow",
      "max_price": 20.02,
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/33/image.jpeg",
      "orders": 3,
      "time_left": "128:9:24",
      "days_left": 6
    },
    {
      "id": 18,
      "name": "nike shoes",
      "max_price": 30.02,
      "discount_price": 24.016,
      "image_url": "/uploads/product_image/image/32/image.jpeg",
      "orders": 2,
      "time_left": "80:9:24",
      "days_left": 4
    },
    {
      "id": 20,
      "name": "jacket",
      "max_price": 50.02,
      "discount_price": 40.016000000000005,
      "image_url": "/uploads/product_image/image/34/image.jpeg",
      "orders": 0,
      "time_left": "176:9:24",
      "days_left": 8
    },
    {
      "id": 21,
      "name": "Shirt",
      "max_price": 50.02,
      "discount_price": 40.016000000000005,
      "image_url": "/uploads/product_image/image/35/image.jpeg",
      "orders": 0,
      "time_left": "152:9:24",
      "days_left": 7
    },
    {
      "id": 22,
      "name": "Jeans",
      "max_price": 20.02,
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/36/image.jpeg",
      "orders": 0,
      "time_left": "56:9:24",
      "days_left": 3
    }
  ]
}
```



This end point return deals for advertisement for buyer.


### HTTP Request

`GET /buyer_deals`


<aside class="success">
  No parameters required for this api`
</aside>

## Get supplier deals


> Sample Request:

```json
  {
    
  }

```

> Sample Response if product deleted successfully:

```json
  {
  "status": true,
  "deals": [
    {
      "id": 19,
      "name": "T-shirt yellow",
      "max_price": 20.02,
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/33/image.jpeg",
      "orders": 3,
      "time_left": "128:7:13",
      "days_left": 6
    },
    {
      "id": 18,
      "name": "nike shoes",
      "max_price": 30.02,
      "discount_price": 24.016,
      "image_url": "/uploads/product_image/image/32/image.jpeg",
      "orders": 2,
      "time_left": "80:7:13",
      "days_left": 4
    },
    {
      "id": 20,
      "name": "jacket",
      "max_price": 50.02,
      "discount_price": 40.016000000000005,
      "image_url": "/uploads/product_image/image/34/image.jpeg",
      "orders": 0,
      "time_left": "176:7:13",
      "days_left": 8
    },
    {
      "id": 21,
      "name": "Shirt",
      "max_price": 50.02,
      "discount_price": 40.016000000000005,
      "image_url": "/uploads/product_image/image/35/image.jpeg",
      "orders": 0,
      "time_left": "152:7:13",
      "days_left": 7
    },
    {
      "id": 22,
      "name": "Jeans",
      "max_price": 20.02,
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/36/image.jpeg",
      "orders": 0,
      "time_left": "56:7:13",
      "days_left": 3
    }
  ],
  "current_bid": {
    "id": 18,
    "name": "nike shoes",
    "max_price": 30.02,
    "discount_price": 24.016,
    "image_url": "/uploads/product_image/image/32/image.jpeg",
    "orders": 2,
    "time_left": "80:7:13",
    "days_left": 4
  }
}
```



This end point return deals and current bid for advertisement for supplier.


### HTTP Request

`GET /supplier_deals`


<aside class="success">
  No parameters required for this api`
</aside>

