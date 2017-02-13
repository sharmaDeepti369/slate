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


