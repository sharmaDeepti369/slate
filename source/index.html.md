---
title: API Reference


includes:
  - errors

search: true
---

# Introduction

Welcome to the Floupr API! You can use our API to access Floupr API endpoints, which can get information on various entities in our database.

We have language bindings in Ruby. 

<strong>Every API for this project have to be prefixed with "http://gharwale.com" and have to include "Auth-Token" and "Scope" (user;s role) as headres once the user is login.</strong>


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
  "message":  "Please check you email. A verification email has been sent.",
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
    "password": "1234567890",
    "role": "buyer",
    "device_id": "453243523423",
    "device_type": "android"
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
  "message": "Sign in successfully"
}

```

> Sample reponse when invalid email or password:

```json

{
  "status": false,
  "message": "Invalid email or password"
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
role | yes | role of user (buyer/ seller)
device_id | yes | device_id of user
device_type | yes | device type by which user login

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



## Facebook signup/sign in

> Sample Request:

```json

{
 "users" : {
    "name": "deepti sharma",
    "provider": "facebook",
    "uid": "7868374658734",
    "email": "sharma.deepti369@gmail.com",
    "profile_pic": "https://www.facebook.com/photo.php?fbid=1704207913224014&set=a.1391569554487853.1073741826.100009044661363&type=3&theater",
    "role": "buyer"
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
role | yes | role by which user want to sign in

# Countries


## Get countries

> Sample Request:

```json


```


> Sample Response :

```json
{
  "status": true,
  "countries": [
    {
      "id": 1,
      "name": "Afghanistan"
    },
    {
      "id": 2,
      "name": "Albania"
    },
    {
      "id": 3,
      "name": "Algeria"
    },
    {
      "id": 4,
      "name": "Andorra"
    },
    {
      "id": 5,
      "name": "Angola"
    }
  ],
  "total_pages": 42
}

```



This end point returns the list of cuntries


### HTTP Request

`GET /countries`


# Profile

## Get profile

> Sample Request:

```json
  {
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
    "state": "UP",
    "city": "noida",
    "zipcode": 201301,
    "country": "india",
    "about": "test desc",
    "profile_pic": "https://radiant-island-87705.herokuapp.com/uploads/user/profile_pic/2/file.jpeg",
    "cards": [],
    "vat": "5627865482",
    "social_accounts": {
      "id": 1,
      "twitter": false,
      "facebook": false,
      "instagram": false
    }
  },
  "status": true
}

```


This end point get the profile for login user.


### HTTP Request

`GET /users/profile`

<aside class="notice">
No parameters required for this api.
</aside>

## Update profile

> Sample Request with fb profile_pic url:

```json
{
 "users" : {
    "name": "deepti sharma",
    "phone_number": "12353737978",
    "address": "sector 61, noida",
    "state": "UP",
    "city": "noida",
    "zipcode": 201301,
    "country_id": 1,
    "about": "test desc",
    "profile_pic": "https://www.facebook.com/photo.php?fbid=1704207913224014&set=a.1391569554487853.1073741826.100009044661363&type=3&theater",
    "vat": "6876876",
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
     "vat": "6876876",
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
    "state": "UP",
    "city": "noida",
    "zipcode": 201301,
    "country": "India",
    "about": "test desc",
     "vat": "6876876",
    "profile_pic": "https://radiant-island-87705.herokuapp.com/uploads/user/profile_pic/2/file.jpeg",
    "social_accounts": {
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
vat | no | vat number for supplier profile
country_id | yes | id for country of the user


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


## Get cards

> Sample Request:

```json

  {
  }


```


> Sample Response:

```json
 {
  "status": true,
  "cards": [
    {
      "id": 1,
      "card_number": "6876",
      "card_type": "visaa"
    },
    {
      "id": 2,
      "card_number": "6876",
      "card_type": "visa"
    },
    {
      "id": 3,
      "card_number": "2876",
      "card_type": "visaa"
    }
  ]
}

```



This end point get credit cards for user.


### HTTP Request

`GET /cards`



## Add card

> Sample Request if billing and shipping address are different:

```json

  {
    "card": {
      "holder_name": "me",
      "card_number": "565332565",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa"
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": false
    },
    "shipping_address": {
      "city": "noida",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 2,
      "zipcode": "12345"
    }
  }

```


> Sample Request if card selected from list:

```json

{
    "card": {
      "id": 2
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": true
    },
    "shipping_address": {
      "city": "noida",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 2,
      "zipcode": "12345"
    }
  }

```
> Sample Request if billing and shipping address are same:

```json

  {
    "card": {
      "holder_name": "me",
      "card_number": "565332565",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa"
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": true
    }
  }

```

> Sample Response if card added successfully:

```json
  {
  "status": true,
  "message": "Card added successfully",
  "card": {
    "id": 3,
    "card_number": "2876",
    "card_type": "visaa"
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
      "card_type": "visaa"
    }
  }


```


> Sample Response if card added successfully:

```json
{
  "status": true,
  "message": "Card updated successfully",
  "card": {
    "id": 2,
    "card_number": "6876",
    "card_type": "visa"
  }
}

```

> Sample Response if already card with id does not exists:

```json
{
  "status": false,
  "message": "Card with this id does not exists"
}

```



This end point update a credit card for user.


### HTTP Request

`PUT /cards`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | credit card id
holder_name | no | credit card holder name
card_number | no | credit card number
expiry_date | no | credit card expiry date
cvv | no | credit card cvv number
card_type | no | credit card type


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

## Get card with addresses

> Sample Request:

```json

  {
    
  }


```


> Sample Response:

```json
{
  "status": true,
  "shipping_address": {
    "id": 25,
    "address": "sector 60",
    "city": "noida",
    "state": "U.P.",
    "zipcode": "12345",
    "country": "Albania"
  },
  "billing_address": {
    "id": 2,
    "address": "sector 60",
    "city": "delhi",
    "state": "Delhi.",
    "zipcode": "15789",
    "country": "Algeria",
    "is_shipping_address": false
  },
  "cards": [
    {
      "id": 1,
      "card_number": "6876",
      "card_type": "visaa"
    },
    {
      "id": 2,
      "card_number": "6876",
      "card_type": "visaa"
    }
  ]
}
```


This end point returns all cards with all address.


### HTTP Request

`GET /get_card_address`


# Addresses

## Get all shipping address

> Sample Request:

```json
```


> Sample Response:

```json
{
  "status": true,
  "shipping_addresses": [
    {
      "id": 1,
      "address": "sector 60",
      "city": "noida",
      "state": "U.P.",
      "zipcode": "12345",
      "country": "Albania"
    },
    {
      "id": 2,
      "address": "sector 61",
      "city": "noida",
      "state": "U.P.",
      "zipcode": "12345",
      "country": "Albania"
    }
  ]
}

```


This end point returns all shipping address for user.


### HTTP Request

`GET /shipping_addresses`


## Get shipping address

> Sample Request:

```json
```


> Sample Response:

```json
 {
  "status": true,
  "shipping_address": {
    "id": 1,
    "address": "sector 60",
    "city": "mumbai",
    "state": "U.P.",
    "zipcode": "12345",
    "country": "india"
  }
}

```


This end point returns a shipping address for user.


### HTTP Request

`GET /shipping_address/<id>`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | shipping address id


## Get billing address

> Sample Request:

```json


```


> Sample Response:

```json
 {
  "status": true,
  "billing_address": {
    "id": 1,
    "address": "sector 60",
    "city": "mumbai",
    "state": "U.P.",
    "zipcode": "12345",
    "country": "india"
  }
}

```


This end point returns a billing address for user.


### HTTP Request

`GET /billing_address`



## add shipping address

> Sample Request:

```json
 
  {
    "address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 2,
      "zipcode": "12345"
    }
  }


```


> Sample Response:

```json
  {
  "status": true,
  "shipping_address": {
    "id": 1,
    "address": "sector 60",
    "city": "delhi",
    "state": "U.P.",
    "zipcode": "12345",
    "country": "Albania"
  }
}
```

This end point adds a shipping address for user.


### HTTP Request

`POST /shipping_address`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
city | yes | city of the user
address | yes | address of the user
state | yes | state of the user
country_id | yes | id for country of the user
zipcode | yes | zipcode of the user


## add billing address

> Sample Request:

```json
 
  {
    "address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 2,
      "zipcode": "12345"
    }
  }


```


> Sample Response:

```json
  {
  "status": true,
  "billing_address": {
    "id": 1,
    "address": "sector 60",
    "city": "delhi",
    "state": "U.P.",
    "zipcode": "12345",
    "country": "Albania"
  }
}
```


This end point adds a billing address for user.


### HTTP Request

`POST /billing_address`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
city | yes | city of the user
address | yes | address of the user
state | yes | state of the user
country_id | yes | id for country of the user
zipcode | yes | zipcode of the user

## update shipping address

> Sample Request:

```json
   
  {
    "address": {
      "id": 2,
      "city": "delhi",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 4,
      "zipcode": "12345"
    }
  }

```


> Sample Response:

```json
  {
  "status": true,
  "shipping_address": {
    "id": 3,
    "address": "sector 60",
    "city": "delhi",
    "state": "M.P.",
    "zipcode": "12345",
    "country": "Albania"
  }
}

```


This end point updates a shipping address for user.


### HTTP Request

`PUT /shipping_address`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | id of address
city | no | city of the user
address | no | address of the user
state | no | state of the user
country_id | yes | id for country of the user
zipcode | no | zipcode of the user

## update billing address

> Sample Request:

```json
   
  {
    "address": {
      "id": 2,
      "city": "delhi",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 4,
      "zipcode": "12345"
    }
  }

```


> Sample Response:

```json
 {
  "status": true,
  "billing_address": {
    "id": 3,
    "address": "sector 60",
    "city": "delhi",
    "state": "M.P.",
    "zipcode": "12345",
    "country": "Albania"
  }
}
```


This end point updates a billing address for user.


### HTTP Request

`PUT /billing_address`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | id of address
city | no | city of the user
address | no | address of the user
state | no | state of the user
country_id | yes | id for country of the user
zipcode | no | zipcode of the user

## get all addresses

> Sample Request:

```json
   
  {
  }

```


> Sample Response:

```json
  {
  "status": true,
  "shipping_address": {
    "id": 3,
    "address": "sector 60",
    "city": "delhi",
    "state": "M.P.",
    "zipcode": "12345",
    "country": "Albania"
  },
  "billing_address": {
    "id": 3,
    "address": "sector 60",
    "city": "delhi",
    "state": "M.P.",
    "zipcode": "12345",
    "country": "Albania"
  }
}

```


This end point return all addresses for user.


### HTTP Request

`GET /get_all_addresses`



# Products

## Get all products

> Sample Request:

```json
{
  "page": 3
}

```


> Sample Response:

```
{
  "status": true,
  "products": [
    {
      "id": 2,
      "name": "shirt",
      "image_url": "/uploads/product_image/image/4/image.jpeg",
      "color": "blue",
      "size": "s",
      "orders": 1
    },
    {
      "id": 3,
      "name": "shoes",
      "image_url": "/uploads/product_image/image/6/image.jpeg",
      "color": "brown",
      "size": "s",
      "orders": 5
    },
    {
      "id": 4,
      "name": "watch",
      "image_url": "/uploads/product_image/image/8/image.jpeg",
      "color": "silver",
      "size": "s",
      "orders": 1
    }
  ],
  "total_pages": 3
}

```


This end point return all products sort on name.


### HTTP Request

`GET /all_products`



## Get product

> Sample Request:

```json

  {
   "product_id" : 1
   }



```


> Sample Response:

```json
{
  "status": true,
  "product": {
    "id": 1,
    "name": "jacket",
    "color": "black",
    "size": "xxl",
    "description": "black color jacket",
    "discount_price": 16.016,
    "min_price": 20.02,
    "product_images": [
      {
        "id": 1,
        "url": "/uploads/product_image/image/1/image.jpeg"
      },
      {
        "id": 2,
        "url": "/uploads/product_image/image/2/image.jpeg"
      }
    ]
  }
}

```


This end point gets a product for user.


### HTTP Request

`GET /products`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product

<aside class="notice">
In case of buyer min_price & discount_price will be in response
And in case of supplier orders will be in response
</aside>


## Create product

> Sample Request:

```json

  {
   "product" : {
      "name": "jacket-leather",
      "color": "green",
      "size": "s",
      "description": "test description",
      "product_images_attributes": [{"image": "data:image/jpeg;base64, /9j/4AAQSkZJRgABAQAAAQABAA..."}, {"image": "data:image/jpeg;base64, /9j/4AAQSkZJRgABAQAAAQABAA..."}],
      "category_id": 1,
      "sub_category_id":1,
      "child_category_id":1,
      "brand": {"id": 4, "name": "allingz"},
      "low_price_link_attributes": {"link": "https://www.google.co.in/"},
      "min_price": 5.00,
      "link": "http://www.amazon.in/Gordania-Bomber-Zipper-Design-Jacket/dp/B01KKM6R54/ref=sr_1_2?s=apparel&ie=UTF8&qid=1491555351&sr=1-2&nodeID=6648217031&psd=1&keywords=lather+jacket",
      "weight": 5.9
    }
   }



```


> Sample Response:

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
      "min_price": 5.00,
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
min_price| minimum price of product
brand[id] | yes if user select the brands from dropdown| id of product's brand
brand[name] | yes | name of product's brand

<aside class="notice">
In case of buyer min_price & discount_price will be in response
And in case of supplier orders will be in response
</aside>



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
      "product_images_attributes": [{"image": "data:image/jpeg;base64, /9j/4AAQSkZJRgABAQAAAQABAA..."}, {"image": "data:image/jpeg;base64, /9j/4AAQSkZJRgABAQAAAQABAA..."}],
      "category_id": 1,
      "sub_category_id":1,
      "child_category_id":1,
      "brand": {"name": "allingz"},
      "min_price": 5.00,
      "link": "http://www.amazon.in/Gordania-Bomber-Zipper-Design-Jacket/dp/B01KKM6R54/ref=sr_1_2?s=apparel&ie=UTF8&qid=1491555351&sr=1-2&nodeID=6648217031&psd=1&keywords=lather+jacket",
      "weight": 5.9
      
    }
   }



```


> Sample Response:

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
      "min_price": 5.00,
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
min_price|  minimum price of product
brand[id] | yes if user select the brands from dropdown| id of product's brand
brand[name] | yes | name of product's brand

<aside class="notice">
In case of buyer min_price & discount_price will be in response
And in case of supplier orders will be in response
</aside>


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

`DELETE /product_image`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product 
image_id | yes | id of image to delete


# Deals

## Get deals


> Sample Request:

```json
  {
    "page": 2
  }

```

> Sample Response if deals found successfully:

```json
 {
  "status": true,
  "products": [
    {
      "id": 1,
      "name": "Trousers",
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "color": "black",
      "size": "xxl",
      "min_price": 20.02
    },
    {
      "id": 1,
      "name": "Trousers",
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "color": "black",
      "size": "xxl",
      "min_price": 20.02
    }
  ],
  "total_pages": 2
}
```



This end point return deals for buyer.


### HTTP Request

`GET /deals`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
show_popular | yes | if deals based on popular 
page | yes | if deals needed more than top 5

## Get supplier deals


> Sample Request:

```json
  {
    "show_popular": true
  }

```

> Sample Response if deals found successfully:

```json
 {
  "status": true,
  "products": [
    {
      "id": 1,
      "name": "Trousers",
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "orders": 6,
      "color": "black",
      "size": "xxl"
    },
    {
      "id": 1,
      "name": "Trousers",
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "orders": 6,
      "color": "black",
      "size": "xxl"
    }
  ]
}
```



This end point return top 5 deals for advertisement.


### HTTP Request

`GET /supplier_deals`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
show_popular | yes | if deals based on popular 
page | yes | if deals needed more than top 5


# Favorites

## Get favorites


> Sample Request:

```json
  {
    "page": 2
  }

```

> Sample Response :

```json
 {
  "status": true,
  "deals": [
    {
      "id": 3,
      "name": "shoes",
      "image_url": "/uploads/product_image/image/5/image.jpeg",
      "color": "brown",
      "size": "s",
      "orders": 3
    },
    {
      "id": 2,
      "name": "shirt",
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "color": "blue",
      "size": "s",
      "orders": 1
    }
  ],
  "total_pages": 1
}

```



This end point return favorites for user.


### HTTP Request

`GET /favorites`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number for product (default 1)

<aside class="notice">
In case of buyer min_price & discount_price will be in response
And in case of supplier orders will be in response
</aside>

## Set favorites


> Sample Request:

```json
  {
   "product_id":5
  }


```

> Sample Response:

```json
 {
  "status": false,
  "message": "Product added to favorites"
}

```



This end set favorite product for user.


### HTTP Request

`POST /favorites`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product to set favorite

## Remove favorites


> Sample Request:

```json
{
 "product_id":1
}


```

> Sample Response if product removed from favorites:

```json
{
  "status": true,
  "message": "Product removed as favorites"
}

```


> Sample Response if product not found:

```json
{
  "status": false,
  "message": "Product not found"
}

```



This api end removes favorite product for user.


### HTTP Request

`DELETE /favorites`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product to set favorite



# Buyer

## Adding product to cart

> Sample Request:

```json
  {
    "order": {
      "product_id": 1,
      "quantity": 4,
      "price": 20.04, 
      "type": "name_price" ,
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


This end point adds a product to cart.


### HTTP Request

`POST /order`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product
type | yes if order type is name price | type of order
price | yes if order type is name price | name price for product 
quantity | yes | quantity of product
order_confirm | yes | false for products to be added in cart


## Getting cart products

> Sample Request:

```json
  {
  "page": 2
  }

```

> Sample Response:

```json
  {
    "status": true,
    "orders": [
      {
        "id": 4,
        "quantity": 4,
        "price": 20.02,
        "name": "Trousers",
        "color": "black",
        "size": "xxl",
        "image_url": "/uploads/product_image/image/1/image.jpeg"
      },
      {
        "id": 6,
        "quantity": 4,
        "price": 20.04,
        "name": "Trousers",
        "color": "black",
        "size": "xxl",
        "image_url": "/uploads/product_image/image/1/image.jpeg"
      }
    ],
  "total_pages": 2
  }

```


This end point get orders for cart.


### HTTP Request

`GET /get_cart`

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number for orders


## Ordering a single product

> Sample Request if card selected from list:

```json

  {
    "order": {
      "product_id": 1,
      "quantity": 4,
      "price": 20.04, 
      "type": "name_price" ,
      "order_confirm": true
    },
     "card": {
      "id": 4
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": false
    },
    "shipping_address": {
      "city": "noida",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 2,
      "zipcode": "12345"
    }
  }

```

> Sample Request if card is new:

```json

  {
    "order": {
      "product_id": 1,
      "quantity": 4,
      "price": 20.04, 
      "type": "name_price" ,
      "order_confirm": true
    },
     "card": {
      "holder_name": "me",
      "card_number": "565332565",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa"
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": false
    },
    "shipping_address": {
      "city": "noida",
      "address": "sector 60",
      "state": "U.P.",
      "country_id": 2,
      "zipcode": "12345"
    }
  }

```

> Sample Request if shipping address select from list:

```json

  {
    "order": {
      "product_id": 1,
      "quantity": 4,
      "price": 20.04, 
      "type": "name_price" ,
      "order_confirm": true
    },
     "card": {
      "holder_name": "me",
      "card_number": "565332565",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa"
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": false
    },
    "shipping_address": {
      "id": 1
    }
  }

```

> Sample Request if billing and shipping address are same:

```json

  {
    "order": {
      "product_id": 1,
      "quantity": 4,
      "price": 20.04, 
      "type": "name_price" ,
      "order_confirm": true
    },
     "card": {
      "holder_name": "me",
      "card_number": "565332565",
      "expiry_date":"09/21",
      "cvv": 754,
      "card_type": "visaa"
    },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": true
    }
  }

```

> Sample Response if order successful:

```json
  {
    "status": true,
    "message": "Product ordered successfully"
  }

```

> Sample Response shipping address sent already exists:

```json
  {
    "status": false,
    "message": "Shipping address already exists"
  }

```

This end point add a product to cart.


### HTTP Request

`POST /order`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product
type | yes if order type is name price | type of order
price | yes if order type is name price | name price for product 
quantity | yes | quantity of product
order_confirm | yes | true 

## Placing order form cart

> Sample Request if card select from list:

```json


{
  "orders":[ 
    {
        "id": 3,
        "quantity": 5,
        "order_confirm": true
    },
    {
      "id": 2,
      "quantity": 5,
      "order_confirm": true
    }
  ],
  "card": {
    "holder_name": "me",
    "card_number": "565332565",
    "expiry_date":"09/21",
    "cvv": 754,
    "card_type": "visaa"
  },
  "billing_address": {
    "city": "delhi",
    "address": "sector 60",
    "state": "Delhi.",
    "country_id": 3,
    "zipcode": "15789",
    "is_shipping_address": false
  },
  "shipping_address": {
    "city": "noida",
    "address": "sector 60",
    "state": "U.P.",
    "country_id": 2,
    "zipcode": "12345"
  }
}


```

> Sample Request if card is new:

```json


{
  "orders":[ 
    {
        "id": 3,
        "quantity": 5,
        "order_confirm": true
    },
    {
      "id": 2,
      "quantity": 5,
      "order_confirm": true
    }
  ],
  "card": {
      "id":5
  },
  "billing_address": {
    "city": "delhi",
    "address": "sector 60",
    "state": "Delhi.",
    "country_id": 3,
    "zipcode": "15789",
    "is_shipping_address": false
  },
  "shipping_address": {
    "city": "noida",
    "address": "sector 60",
    "state": "U.P.",
    "country_id": 2,
    "zipcode": "12345"
  }
}


```
> Sample Request if shipping address selected from list:

```json


{
  "orders":[ 
    {
        "id": 3,
        "quantity": 5,
        "order_confirm": true
    },
    {
      "id": 2,
      "quantity": 5,
      "order_confirm": true
    }
  ],
  "card": {
      "id":5
  },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": false
    },
    "shipping_address": {
      "id": 1
    }
}


```
> Sample Request if shipping address and billing address are same:

```json


{
  "orders":[ 
    {
        "id": 3,
        "quantity": 5,
        "order_confirm": true
    },
    {
      "id": 2,
      "quantity": 5,
      "order_confirm": true
    }
  ],
  "card": {
      "id":5
  },
    "billing_address": {
      "city": "delhi",
      "address": "sector 60",
      "state": "Delhi.",
      "country_id": 3,
      "zipcode": "15789",
      "is_shipping_address": true
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

This end point delete a order for user.


### HTTP Request

`DELETE /order`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
order_id | yes | id of order to delete from cart



## Recent histoy


> Sample Request:

```json
  {
    "page": 2
  }

```

> Sample Response :

```json
 {
  "status": true,
  "orders": [
    {
      "id": 1,
      "quantity": 4,
      "price": 10.04,
      "name": "jacket",
      "color": "black",
      "size": "xxl",
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 2,
      "quantity": 3,
      "price": 30.05,
      "name": "shirt",
      "color": "blue",
      "size": "s",
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 3,
      "quantity": 6,
      "price": 28.04,
      "name": "shoes",
      "color": "brown",
      "size": "s",
      "image_url": "/uploads/product_image/image/5/image.jpeg",
      "order_date": "2017-03-16"
    }
  ],
  "total_pages": 2
}
```



This end point return recent history for buyer.


### HTTP Request

`GET /history/recent`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number of results (default 1)



## Pending history


> Sample Request:

```json
  {
    "page": 2
  }

```

> Sample Response :

```json
 {
  "status": true,
  "orders": [
    {
      "id": 1,
      "quantity": 4,
      "price": 10.04,
      "name": "jacket",
      "color": "black",
      "size": "xxl",
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 2,
      "quantity": 3,
      "price": 30.05,
      "name": "shirt",
      "color": "blue",
      "size": "s",
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 3,
      "quantity": 6,
      "price": 28.04,
      "name": "shoes",
      "color": "brown",
      "size": "s",
      "image_url": "/uploads/product_image/image/5/image.jpeg",
      "order_date": "2017-03-16"
    }
  ],
  "total_pages": 2
}
```



This end point return pending history for buyer.


### HTTP Request

`GET /history/pending`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number of results (default 1)




## Won hitory


> Sample Request:

```json
  {
    "page": 2
  }

```

> Sample Response :

```json
{
  "status": true,
  "orders": [
    {
      "id": 1,
      "quantity": 4,
      "price": 10.04,
      "name": "jacket",
      "color": "black",
      "size": "xxl",
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 2,
      "quantity": 3,
      "price": 30.05,
      "name": "shirt",
      "color": "blue",
      "size": "s",
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 3,
      "quantity": 6,
      "price": 28.04,
      "name": "shoes",
      "color": "brown",
      "size": "s",
      "image_url": "/uploads/product_image/image/5/image.jpeg",
      "order_date": "2017-03-16"
    }
  ],
  "total_pages": 2
}
```



This end point return won history for buyer.


### HTTP Request

`GET /history/won`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number of results (default 1)



## Lost history


> Sample Request:

```json
  {
    "page": 2
  }

```

> Sample Response :

```json
 {
  "status": true,
  "orders": [
    {
      "id": 1,
      "quantity": 4,
      "price": 10.04,
      "name": "jacket",
      "color": "black",
      "size": "xxl",
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 2,
      "quantity": 3,
      "price": 30.05,
      "name": "shirt",
      "color": "blue",
      "size": "s",
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "order_date": "2017-03-16"
    },
    {
      "id": 3,
      "quantity": 6,
      "price": 28.04,
      "name": "shoes",
      "color": "brown",
      "size": "s",
      "image_url": "/uploads/product_image/image/5/image.jpeg",
      "order_date": "2017-03-16"
    }
  ],
  "total_pages": 2
}
```



This end point return lost history for buyer.


### HTTP Request

`GET /history/lost`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number of results (default 1)


## Get history details


> Sample Request:

```json
  {
    "order_id": 7
  }

```

> Sample Response :

```json
{
  "status": true,
  "order": {
    "id": 7,
    "quantity": 5,
    "price": 50,
    "name": "shoes",
    "color": "brown",
    "size": "s",
    "images": [
      "/uploads/product_image/image/5/image.jpeg",
      "/uploads/product_image/image/6/image.jpeg"
    ],
    "order_date": "2017-03-16",
    "description": "brown laces shoes"
  }
}

```



This end point return detail history for order.


### HTTP Request

`GET /order?order_id=<order-id>`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
order_id | yes | id of order from history

# Search

## Get search results


> Sample Request:

```json
  {
    "query": "black",
    "page": 3,
    "sort_by": "popular/name"
  }

```

> Sample Response :

```json
 {
  "status": true,
  "products": [
    {
      "id": 1,
      "name": "jacket",
      "discount_price": 16.016,
      "image_url": "/uploads/product_image/image/1/image.jpeg",
      "orders": 1,
      "color": "black",
      "size": "xxl",
      "min_price": 20.02
    },
    {
      "id": 2,
      "name": "shirt",
      "discount_price": 24.04,
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "orders": 1,
      "color": "blue",
      "size": "s",
      "min_price": 30.05
    }
  ],
  "total_pages": 2
}
```



This end point returns search results corresponding to a search keyword.


### HTTP Request

`GET /search`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
query | yes | search keyword entered by user
page | yes |page number for results (default 1)
sort_by | yes | sort on popularity or name

<aside class="notice">
In case of supplier min_price & discount_price will not be in response
</aside>


## Get search on category/brand basis


> Sample Request:

```json
  {
    "category_ids": [3,1, 2],
    "sub_category_ids": [3,4],
    "child_category_ids": [1,5,6],
    "brand_ids": [3,2],
    "page": 3,
    "sort_by": "popular/name"
  }


```

> Sample Response:

```json
{
  "status": true,
  "products": [
    {
      "id": 2,
      "name": "shirt",
      "image_url": "/uploads/product_image/image/3/image.jpeg",
      "color": "blue",
      "size": "s",
      "orders": 1
    }
  ],
  "total_pages": 1
}
```



This end point returns search results corresponding to brand and category selected.


### HTTP Request

`POST /search_by_category`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
category_ids | no | category selected by user
sub_category_ids | no | sub category selected by user
child_category_ids | no | child category selected by user
brand_ids | no |brand selected by user
page | yes | page number for response
sort_by | yes | sort on popularity or name

<aside class="notice">
In case of supplier min_price & discount_price will not be in response
</aside>



# Supplier

## Orders list

> Sample Request:

```json
  {

  }

```

> Sample Response :

```json
  {
  "status": true,
  "products": [
    {
      "id": 5,
      "name": "Rolex Shirt l red",
      "image_url": "/uploads/product_image/image/9/image.jpeg",
      "regular_orders": {
        "quantity": 3,
        "price": 338.1
      },
      "name_price_orders": {
        "quantity": 2,
        "price": 0
      },
      "total_price": 1014.30,
      "total_countries": 1
    },
    {
      "id": 7,
      "name": "SanDisk Pendrive 8 GB Black",
      "image_url": "/uploads/product_image/image/13/image.png",
      "regular_orders": {
        "quantity": 0,
        "price": 102.9
      },
      "name_price_orders": {
        "quantity": 7,
        "price": 0
      },
      "total_price": 0,
      "total_countries": 1
    }
  ],
  "total_pages": 1
}
```



This end point returns orders base on prefrences for supplier.


### HTTP Request

`GET /supplier/orders`


## Orders search by keyword

> Sample Request:

```json
  {

  }

```

> Sample Response :

```json
{
  "status": true,
  "products": [
    {
      "id": 5,
      "name": "Rolex Shirt l red",
      "image_url": "/uploads/product_image/image/9/image.jpeg",
      "regular_orders": {
        "quantity": 3,
        "price": 338.1
      },
      "name_price_orders": {
        "quantity": 2,
        "price": 0
      },
      "total_price": 1014.30,
      "total_countries": 1
    },
    {
      "id": 7,
      "name": "SanDisk Pendrive 8 GB Black",
      "image_url": "/uploads/product_image/image/13/image.png",
      "regular_orders": {
        "quantity": 0,
        "price": 102.9
      },
      "name_price_orders": {
        "quantity": 7,
        "price": 0
      },
      "total_price": 0,
      "total_countries": 1
    }
  ],
  "total_pages": 1
}
```



This end point returns orders base on query search.


### HTTP Request

`GET /orders/search?query=<query_string>`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
query_string | yes | string user searching for


## Orders search by categories

> Sample Request:

```json
  {
    "category_ids": [1,3],
    "sub_category_ids": [3,4],
    "child_category_ids": [12,15],
    "brand_ids": [8,9],
    "country_ids": [3,4,5],
    "sort_by": "popular/name",
    "page": 2
  }


```

> Sample Response :

```json

  {
  "status": true,
  "products": [
    {
      "id": 5,
      "name": "Rolex Shirt l red",
      "image_url": "/uploads/product_image/image/9/image.jpeg",
      "regular_orders": {
        "quantity": 3,
        "price": 338.1
      },
      "name_price_orders": {
        "quantity": 2,
        "price": 0
      },
      "total_price": 1014.30,
      "total_countries": 1
    },
    {
      "id": 7,
      "name": "SanDisk Pendrive 8 GB Black",
      "image_url": "/uploads/product_image/image/13/image.png",
      "regular_orders": {
        "quantity": 0,
        "price": 102.9
      },
      "name_price_orders": {
        "quantity": 7,
        "price": 0
      },
      "total_price": 0,
      "total_countries": 1
    }
  ],
  "total_pages": 1
}

```



This end point returns orders base on categories search.


### HTTP Request

`POST /orders/search`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
category_ids | no | category selected by user
sub_category_ids | no | sub category selected by user
child_category_ids | no | child category selected by user
brand_ids | no |brand selected by user
country_ids | no |countries selected by user
page | yes | page number for response (by default 1)
sort_by | yes | sort on popularity or name


## Get countries list

> Sample Request:

```json
  {
  }


```

> Sample Response :

```json
{
  "status": true,
  "countries": [
    {
      "id": 1,
      "name": "Afghanistan",
      "selected": true,
      "shipping_cost": 0
    },
    {
      "id": 2,
      "name": "Albania",
      "selected": false,
      "shipping_cost": 4
    },
    {
      "id": 3,
      "name": "Algeria",
      "selected": false,
      "shipping_cost": 0
    },
    {
      "id": 4,
      "name": "Andorra",
      "selected": false,
      "shipping_cost": 3
    },
    {
      "id": 5,
      "name": "Angola",
      "selected": false,
      "shipping_cost": 0
    }
  ],
  "total_pages": 42
}

```



This end point returns list of countries with shipping cost and selected for supplier.


### HTTP Request

`GET /supplier/countries`


## Set Shipping costs

> Sample Request:

```json
  {
  "shipping_costs": [
    {
    "country_id": 1,
    "cost": 3
    },
    {
    "country_id": 2,
    "cost": 2
    },
    {
    "country_id": 4,
    "cost": 4
    }
  ]
  
}

```

> Sample Response:

```json
  {
  "status": true,
  "countries": [
    {
      "id": 1,
      "name": "Afghanistan",
      "selected": true,
      "shipping_cost": 3
    },
    {
      "id": 2,
      "name": "Albania",
      "selected": false,
      "shipping_cost": 2
    },
    {
      "id": 3,
      "name": "Algeria",
      "selected": false,
      "shipping_cost": 0
    },
    {
      "id": 4,
      "name": "Andorra",
      "selected": false,
      "shipping_cost": 4
    },
    {
      "id": 5,
      "name": "Angola",
      "selected": false,
      "shipping_cost": 0
    }
  ],
  "total_pages": 42
}
```



This end point will set shipping costs for countries.


### HTTP Request

`POST /set_shipping_cost`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
country_id | yes | id of country selected
cost | yes | shipping cost for that country ser wants to set

## Get order details

> Sample Request:

```json
  
```

> Sample Response:

```
{
  "status": true,
  "product": {
    "id": 2,
    "name": "Apple iphone 7 plus 12 grey",
    "image_url": "/uploads/product_image/image/3/image.jpeg",
    "description": "iphone 6 grey",
    "category": "Appliances",
    "sub_category": "Floor Care, Seasonal & Parts",
    "regular_orders": {
      "orders": [
        {
          "id": 19,
          "quantity": 4,
          "price": 0,
          "country_id": 4
        }
      ],
      "quantity": 4,
      "guarnteed_price": 956.48,
      "regular_total": 3825.92
    },
    "name_price_orders": {
      "orders": [
        {
          "id": 17,
          "quantity": 4,
          "price": 20.04,
          "country_id": 2
        },
        {
          "id": 18,
          "quantity": 4,
          "price": 20.04,
          "country_id": 88
        }
      ],
      "quantity": 8
    },
    "shipping_cost": 28,
    "countries": [
      {
        "id": 2,
        "name": "Albania",
        "shipping_cost": 3,
        "orders": 1
      },
      {
        "id": 4,
        "name": "Andorra",
        "shipping_cost": 4,
        "orders": 1
      },
      {
        "id": 88,
        "name": "India",
        "shipping_cost": 0,
        "orders": 1
      }
    ],
    "purchase_price": 0
  }
}
```
> Sample Response if one of the orders not present:

```
{
  "status": true,
  "product": {
    "id": 3,
    "name": "Apple iphone 7 plus 12 grey",
    "image_url": "/uploads/product_image/image/5/image.jpeg",
    "description": "iphone 6 grey",
    "category": "Appliances",
    "sub_category": "Floor Care, Seasonal & Parts",
    "regular_orders": {
      "orders": 0,
      "quantity": 0,
      "guarnteed_price": 0,
      "regular_total": 0
    },
    "name_price_orders": {
      "orders": 0,
      "quantity": 0
    },
    "shipping_cost": 0,
    "countries": [],
    "purchase_price": 12
  }
}

```



This end point returns details of order.


### HTTP Request

`GET /orders/<product_id>`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product for details


## Create Bids

> Sample Request:

```json
  {
    "bid" : {
    "product_id": 5,
    "regular_quantity": 33,
    "regular_price": 4,
    "regular_total": 56,
    "np_quantity": 89,
    "np_price": 75,
    "np_total": 356,
    "purchase_price": 12,
    "my_profit": 11,
    "shipping_cost": [
      {
      "country_id": 1,
      "cost": 3
      },
      {
      "country_id": 2,
      "cost": 3
      },
      {
      "country_id": 4,
      "cost": 4
      }
    ],
    "is_deleted": false,
    "is_confirmed": false
  }
```

> Sample Response :

```json
  {
    "status": true,
    "message": "Order moved to bid successfully"
  }
```



This end point create a bid for orders.


### HTTP Request

`POST /bids`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
product_id | yes | id of product
regular_quantity | yes | quantity of regular orders user wants to fulfill
regular_price | yes | price per piece for regular orders
regular_total | yes | total price for regular orders
np_quantity | yes | quantity of name price orders user wants to fulfill
np_price | yes | price per piece for name price orders
np_total | yes | total price for name price orders
purchase_price | yes | purchase price of the product filled by user
my_profit | yes | my profit calculated for user
country_id | yes | id of country selected
cost | yes | shipping cost for that country ser wants to set


## Update Bids

> Sample Request:

```json
  {
    "bid" : {
    "id": 1,
    "product_id": 5,
    "regular_quantity": 33,
    "regular_price": 4,
    "regular_total": 56,
    "np_quantity": 89,
    "np_price": 75,
    "np_total": 356,
    "purchase_price": 12,
    "my_profit": 11,
    "shipping_cost": [
      {
      "country_id": 1,
      "cost": 3
      },
      {
      "country_id": 2,
      "cost": 3
      },
      {
      "country_id": 4,
      "cost": 4
      }
    ],
    "is_deleted": false,
    "is_confirmed": false
  }
```

> Sample Response :

```json
  {
    "status": true,
    "message": "Order moved to bid successfully"
  }
```



This end point updates a bid for orders.


### HTTP Request

`PUT /bids`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | id of bid
product_id | yes | id of product
regular_quantity | yes | quantity of regular orders user wants to fulfill
regular_price | yes | price per piece for regular orders
regular_total | yes | total price for regular orders
np_quantity | yes | quantity of name price orders user wants to fulfill
np_price | yes | price per piece for name price orders
np_total | yes | total price for name price orders
purchase_price | yes | purchase price of the product filled by user
my_profit | yes | my profit calculated for user
country_id | yes | id of country selected
cost | yes | shipping cost for that country ser wants to set


## Get Bids

> Sample Request:

```json
  
```

> Sample Response :

```json
 {
  "status": true,
  "bids": [
    {
      "id": 7,
      "name": "Apple iphone 7 plus 12 grey",
      "image_url": "/uploads/product_image/image/5/image.jpeg",
      "regular_orders": {
        "quantity": 33,
        "price": 4
      },
      "name_price_orders": {
        "quantity": 89,
        "price": 75
      },
      "total_price": 428,
      "total_countries": 3
    }
  ],
  "total_pages": 1
}
```



This end point returns list of bids for orders.


### HTTP Request

`GET /bids`

## Get Single Bid

> Sample Request:

```json
  
```

> Sample Response :

```json
 {
  "status": true,
  "bid": {
    "id": 7,
    "product_id": 3,
    "name": "Apple iphone 7 plus 12 grey",
    "image_url": "/uploads/product_image/image/5/image.jpeg",
    "description": "iphone 6 grey",
    "category": "Appliances",
    "sub_category": "Floor Care, Seasonal & Parts",
    "regular_orders": {
      "quantity": 33,
      "guarnteed_price": 4,
      "regular_total": 56
    },
    "name_price_orders": {
      "orders": [
        {
          "id": 16,
          "quantity": 4,
          "price": 20.04
        }
      ],
      "quantity": 89,
      "np_total": 356,
      "np_price": 75
    },
    "shipping_cost": 16,
    "countries": [
      {
        "id": 4,
        "name": "Andorra",
        "shipping_cost": 4,
        "orders": 1
      }
    ],
    "purchase_price": 12,
    "my_profit": 11
  }
}
```



This end point returns a bid for orders.


### HTTP Request

`GET /bids/<id>`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
id | yes | id of bid



# Categories

## Get all categories

> Sample Request:

```json
  {
   "page": 2
  }

```

> Sample Response:

```json
  {
  "status": true,
  "categories": [
    {
      "id": 14,
      "name": "Toys, games & drones",
      "child_exists": false,
      "selected": false
    },
    {
      "id": 2,
      "name": "Tv & home theater",
      "child_exists": true,
      "selected": true
    },
    {
      "id": 7,
      "name": "Video games",
      "child_exists": true,
      "selected": false
    },
    {
      "id": 13,
      "name": "Wearable technology",
      "child_exists": true,
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for categories.


### HTTP Request

`GET /categories`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number for response
paginate | no | flase if no pagination required

<aside class="notice">
In case of buyer selected will not be in response
</aside>


## Get all sub categories

> Sample Request:

```json
  {
   "page": 2
  }

```

> Sample Response:

```json
 {
  "status": true,
  "categories": [
    {
      "id": 6,
      "name": "Audio & video components",
      "child_exists": true,
      "selected": false
    },
    {
      "id": 13,
      "name": "Camcorderscamcorders",
      "child_exists": true,
      "selected": false
    },
    {
      "id": 12,
      "name": "Cameras & lenses",
      "child_exists": true,
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for sub categories.


### HTTP Request

`GET /sub_categories`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number for response
paginate | no | flase if no pagination required

<aside class="notice">
In case of buyer selected will not be in response
</aside>


## Get all child categories

> Sample Request:

```json
  {
   "page": 2
  }

```

> Sample Response:

```json
 {
  "status": true,
  "categories": [
    {
      "id": 64,
      "name": "Action camcorders",
      "selected": false
    },
    {
      "id": 145,
      "name": "Action camcorders",
      "selected": false
    },
    {
      "id": 146,
      "name": "Activity trackers & pedometers",
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for child categories.


### HTTP Request

`GET /child_categories`


### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number for response
paginate | no | flase if no pagination required

<aside class="notice">
In case of buyer selected  will not be in response
</aside>


## Get all brands

> Sample Request:

```json
  {
   "page": 1,
   "sort_by": "name/popular"
  }

```

> Sample Response:

```json
  {
  "status": true,
  "brands": [
    {
      "id": 1,
      "name": "brand 1",
      "selected": true
    },
    {
      "id": 2,
      "name": "brand 2",
      "selected": false
    },
    {
      "id": 3,
      "name": "brand 3",
      "selected": false
    },
    {
      "id": 4,
      "name": "brand 4",
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for brands.


### HTTP Request

`GET /brands`



### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
page | yes | page number for response
paginate | no | flase if no pagination required
sort_by | yes | sort by name or popular brands

<aside class="notice">
In case of buyer selected  will not be in response
</aside>


## Get sub_category for category

> Sample Request:

```json
  {
    "page": 2,
   "category_ids": [1, 2, 3]
  }

```

> Sample Response:

```json
{
  "status": true,
  "categories": [
    {
      "id": 7,
      "name": "Accessories",
      "child_exists": true,
      "selected": false
    },
    {
      "id": 6,
      "name": "Audio & video components",
      "child_exists": true,
      "selected": false
    },
    {
      "id": 10,
      "name": "Computer accessories",
      "child_exists": true,
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for sub categories belongs to a category.


### HTTP Request

`POST /get_sub_categories`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
category_ids | yes | id of category
page | yes | page number for response
paginate | no | flase if no pagination required

<aside class="notice">
In case of buyer selected will not be in response
</aside>


## Get child category for sub category

> Sample Request:

```json
 {
    "page": 1,
   "sub_category_ids": [1,2,3]
  }


```

> Sample Response:

```json
{
  "status": true,
  "categories": [
    {
      "id": 15,
      "name": "Appliance parts & accessories",
      "selected": false
    },
    {
      "id": 3,
      "name": "Dishwashers",
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for child categories belongs to a sub category.


### HTTP Request

`POST /get_child_categories`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
sub_category_ids | yes | id of sub_category
page | yes | page number for response
paginate | no | flase if no pagination required

<aside class="notice">
In case of buyer selected  will not be in response
</aside>


## Get brands for categories

> Sample Request:

```json
{
  "page": 1,
  "key": "child_category",
  "category_ids": [1,2,3],
  "sort_by": "name/popular"
}


```

> Sample Response:

```json
{
  "status": true,
  "brands": [
    {
      "id": 3,
      "name": "brand 3",
      "selected": false
    }
  ],
  "total_pages": 4
}
```



This end point gives the list for brands based on categories/sub_categories/child_categories


### HTTP Request

`POST /get_brands`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
key | yes | can be "category"/"sub_categories"/"child_categories" based on requirement
category_ids | yes | id of "category"/"sub_categories"/"child_categories" based on requirement
page | yes | page number for response
paginate | no | flase if no pagination required
sort_by | yes | sort by name or popular brands

<aside class="notice">
In case of buyer selected  will not be in response
</aside>

# Prefrences

## Set categories for prefrences

> Sample Request:

```json
{
 "category_ids": [7,9]
}

```

> Sample Response:

```json
{
  "status": true,
  "sub_categories": [
    {
      "id": 22,
      "name": "Consoles",
      "child_exists": true,
      "selected": false
    },
    {
      "id": 23,
      "name": "Accessories",
      "child_exists": true,
      "selected": true
    }
  ]
}
```



This end point set the preference categories and return top 5 sub categories.

### HTTP Request

`POST /preferences/categories`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
category_ids | yes | id of categories to set in preferences


## Set sub_categories for prefrences

> Sample Request:

```json
{
 "sub_category_ids": [23,24,25,26]
}


```

> Sample Response:

```json
{
  "status": true,
  "child_categories": [
    {
      "id": 119,
      "name": "Playstation vr",
      "selected": false
    },
    {
      "id": 120,
      "name": "Xbox one accessories",
      "selected": true
    },
    {
      "id": 121,
      "name": "Playstation 4 accessories",
      "selected": true
    }
  ]
}
```



This end point set the preference sub_categories and return top 5 child categories.

### HTTP Request

`POST /preferences/sub_categories`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
sub_category_ids | yes | id of sub-categories to set in preferences


## Set child_categories for prefrences

> Sample Request:

```json
{
 "child_category_ids": [123,122,121,120]
}

```

> Sample Response:

```json
{
  "status": true,
  "brands": [
    {
      "id": 2,
      "name": "brand 2",
      "selected": true
    },
    {
      "id": 1,
      "name": "brand 1",
      "selected": true
    },
    {
      "id": 3,
      "name": "brand 3",
      "selected": true
    }
  ]
}
```



This end point set the preference child_categories and return top 5 brands.

### HTTP Request

`POST /preferences/child_categories`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
child_category_ids | yes | id of child-categories to set in preferences


## Set brands for prefrences

> Sample Request:

```json
{
 "brand_ids": [1,2,3]
}


```

> Sample Response:

```json
{
  "status": true,
  "deals": [
    {
      "id": 8,
      "name": "glasses",
      "image_url": "/uploads/product_image/image/15/image.jpeg",
      "color": "grey",
      "size": "s",
      "orders": 0
    },
    {
      "id": 9,
      "name": "glasses",
      "image_url": "/uploads/product_image/image/17/image.jpeg",
      "color": "grey",
      "size": "s",
      "orders": 0
    }
  ]
}
```



This end point set the preference brands and return top 5 products based on preferences.

### HTTP Request

`POST /preferences/brands`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
brand_ids | yes | id of brands to set in preferences

## Set countries for prefrences

> Sample Request:

```json
{
 "country_ids": [1,2,3]
}


```

> Sample Response:

```json
{
  "status": true,
  "message": "preferences saved successfully"
}
```



This end point set the preference countries.

### HTTP Request

`POST /preferences/countries`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
country_ids | yes | id of countries to set in preferences

# App Asset

## Get Auction Date

> Sample Request:

```json
{
}


```

> Sample Response:

```json
{
  "status": true,
  "date": '12-16-2017'
}
```



This end point returns the date for auction day.

### HTTP Request

`GET /aution_day`


## Get App Logo

> Sample Request:

```json
{
}


```

> Sample Response:

```json
{
    "status": true,
    "logo": "/uploads/app_asset/logo/1/thumb_logo.png"
}
```



This end point returns the logo of app.

### HTTP Request

`GET /app_logo`

## Set last used

> Sample Request:

```json

```

> Sample Response:

```json
{
  "status": true,
  "message": "Last used updated successfully"
}
```



This end point set the last used date of app for an user.

### HTTP Request

`POST /last_used`


# Offers

## Confirm Bids

> Sample Request if type is my profit:

```json
{
  "bid_ids": [1,2],
  "offer_type": "my_profit",
  "my_profit": {
    "max_profit": 2344,
    "min_profit": 5644.3,
    "max_dicount": 9873.3,
    "min_discount": 834.4,
    "offer_steps_attributes":[
      {"profit": 838, "discount": 47.33},
      {"profit": 33, "discount": 22.3},
      {"profit": 2434, "discount": 2.3}
    ]
    
  }
}

```

> Sample Request if type is one offer:

```json
{
  "bid_ids": [1,2],
  "offer_type": "one_offer",
  "one_offer": {
    "offer":7474,
    "discount": 83.33
  }
}

```

> Sample Response:

```json
{
    "status": true,
    "message": "Bids saved successfully. Please check your bid history."
}
```



This end point final the bids for auction method.

### HTTP Request

`POST /offers`

### URL Parameters

Parameter| Mandatory | Description
--------- |--------- |-----------
bid_ids | yes | id of bids to be confirmed
offer_type | yes | type of offer selected by user
max_profit | yes | max  profit in conitinuous discount
min_profit | yes | min  profit in conitinuous discount
max_discount | yes | max profit discount in conitinuous discount
min_discount | yes | min profit discount in conitinuous discount
profit | yes | profit in steps discount
discount | yes | dicount in steps discount
offer | yes | your offer in one offer
dicount | yes | discount in one offer