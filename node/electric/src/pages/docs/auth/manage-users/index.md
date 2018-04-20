---
title: "Manage Users"
description: "Create, delete or update users by using WeDeploy Auth."
headerTitle: "Auth"
layout: "guide"
weight: 2
---

### {$page.title}

###### {$page.description}

<aside>

If you are unfamiliar with our API, please visit the [API Clients](/docs/intro/api-clients/) page first to learn how to install and configure it.

</aside>

<article id="1">

## Create user

You create a new user in your WeDeploy project by calling the `createUser` method or by signing in a user for the first time using an identity provider such as Google or Facebook.

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .createUser({
    email: 'user@domain.com',
    password: 'abc'
  })
  .then(function(user) {
    // Successfully created.
  })
  .catch(function(err) {
    // Not created.
  });
```
```swift
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .createUser(email: "user@domain.com", password: "abc", name: "somename")
  .then { user -> Void in
    // Successfully created.
  }
  .catch { err in
    // Not created.
  }
```
```text/x-java
WeDeploy
  .auth("<auth-url>")
  .createUser("user@domain.com", "password", "somename")
  .execute();
```
```text/x-sh
curl -X "POST" "https://<serviceID>-<projectID>.wedeploy.io/users" \
     -H 'Content-Type: application/json' \
     -d '{
       "email": "user@domain.com",
       "password": "abc"
     }'

```

</article>

<article id="2">

## Get current user

```javascript
var currentUser = WeDeploy.auth('https://<serviceID>-<projectID>.wedeploy.io').currentUser;

if (currentUser) {
  // User is signed in.
} else {
  // No user is signed in.
}
```
```swift
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .getCurrentUser()
  .then { user -> Void in
    // User found.
  }
  .catch { error in
    // User does not exist.
  }
```
```text/x-java
WeDeploy
  .auth("<auth-url>")
  .getCurrentUser()
  .execute();
```
```text/x-sh
curl -X "GET" "https://<serviceID>-<projectID>.wedeploy.io/user" \
     -H 'Authorization: Bearer <user-token>'
```

</article>

<article id="3">

## Get user

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .getUser(userId)
  .then(function(user) {
    // User found.
  })
  .catch(function(err) {
    // User does not exist.
  });
```
```swift
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .getUser(id: "userId")
  .then { user -> Void in
    // User found.
  }
  .catch { error in
    // User does not exist.
  }
```
```text/x-java
WeDeploy
  .auth("<auth-url>")
  .getUser("userId")
  .execute();
```
```text/x-sh
curl -X "GET" "https://<serviceID>-<projectID>.wedeploy.io/users/<userID>" \
     -H 'Authorization: Bearer <your-project-master-token>'
```

</article>

<article id="4">

## Get all users

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .getAllUsers()
  .then(function(res) {
    // res contains an array of users.
  })
  .catch(function(err) {
    // An error happened.
  });
```
```swift
WeDeploy
  .auth("https://<serviceID>-<projectID>.wedeploy.io")
  .getAllUsers()
  .then { res -> Void in
    // res contains an array of users.
  }
  .catch { error in
    // An error happened.
  }
```
```text/x-java
WeDeploy
  .auth("https://<serviceID>-<projectID>.wedeploy.io")
  .getAllUsers()
  .execute();
```
```text/x-sh
curl -X "GET" "https://<serviceID>-<projectID>.wedeploy.io/users" \
     -H 'Authorization: Bearer <your-project-master-token>'
```

</article>

<article id="5">

## Delete user

There are two ways to delete a user.

**Delete currentUser**

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .currentUser
  .deleteUser()
  .then(function() {
    // Successfully deleted.
  })
  .catch(function(err) {
    // Not deleted.
  });
```


**Delete user by id**

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .deleteUser("userId")
  .then(function() {
    // Successfully deleted.
  })
  .catch(function(err) {
    // An error happened.
  });
```
```swift
WeDeploy
  .auth("https://<serviceID>-<projectID>.wedeploy.io")
  .deleteUser(id: "userId")
  .then { _ -> Void in
    // Successfully deleted.
  }
  .catch { error in
    // An error happened.
  }
```
```text/x-java
WeDeploy
  .auth("https://<serviceID>-<projectID>.wedeploy.io")
  .deleteUser("userId")
  .execute();
```
```text/x-sh
curl -X "DELETE" "https://<serviceID>-<projectID>.wedeploy.io/users/<userID>" \
     -H 'Authorization: Bearer <your-project-master-token>'
```

</article>

<article id="6">

## Update user

There are two ways to update a user.

**Update currentUser**

```javascript
var currentUser = WeDeploy.auth('https://<serviceID>-<projectID>.wedeploy.io').currentUser;

currentUser
  .updateUser({
    password: "password",
    email: "eleven@hawkinslabs.com",
    name: "Eleven",
    photoUrl: "https://hawkinslabs.com/011/profile.jpg"
  })
  .then(function() {
    // Successfully updated.
  })
  .catch(function(err) {
    // Not updated.
  });
```

**Update user by id**

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .updateUser(userId, {
    password: "password",
    email: "eleven@hawkinslabs.com",
    name: "Eleven",
    photoUrl: "https://hawkinslabs.com/011/profile.jpg"
  })
  .then(function() {
    // Successfully updated.
  })
  .catch(function(err) {
    // Not updated.
  });
```
```swift
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .updateUser(id: "userId" , email: "eleven@hawkinslabs.com", password: "password", name: "Eleven")
  .then { _ -> Void in
    // Successfully updated
  }
  .catch { err in
    // Not updated.
  }
```
```text/x-java
JSONObject fields = new JSONObject()
  .put("email", "eleven@hawkinslabs.com")
  .put("password", "newPassword")
  .put("name", "Eleven");

WeDeploy
  .auth("<auth-url>")
  .updateUser("userId", fields)
  .execute();
```
```text/x-sh
curl -X "PATCH" "https://<serviceID>-<projectID>.wedeploy.io/users/<userID>" \
     -H 'Authorization: Bearer <your-project-master-token>' \
     -H 'Content-Type: application/json'
     -d $'{
        "password": "password",
        "email": "eleven@hawkinslab.com",
        "name": "Eleven",
        "photoUrl": "https://hawkinslabs.com/011/profile.jpg"
      }'
```

</article>

<article id="7">

## Send a password reset email

You can send a password reset email to a user with the `sendPasswordResetEmail` method. For example:

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .sendPasswordResetEmail("user@domain.com")
  .then(function() {
    // Email sent.
  })
  .catch(function(err) {
    // An error happened.
  });
```
```swift
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .sendPasswordReset(email: "user@domain.com")
  .then { _ -> Void in
    // Successfully signed out.
  }
  .catch { err in
    // Not signed out.
  }
```
```text/x-java
WeDeploy
  .auth("<auth-url>")
  .sendPasswordResetEmail("user@domain.com")
  .execute();
```
```text/x-sh
curl -X "POST" "https://<serviceID>-<projectID>.wedeploy.io/user/recover" \
     -H 'Content-Type: application/json' \
     -d $'{
        "email": "user@domain.com"
      }'
```

</article>

<article id="8">

## Sign-out

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .signOut()
  .then(function() {
    // User is signed out.
  })
  .catch(function(err) {
    // User was signed out.
  });
```
```swift
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .signOut()
  .then { _ -> Void in
    // Successfully signed out.
  }
  .catch { err in
    // Not signed out.
  }
```
```text/x-java
WeDeploy
  .auth("<auth-url>")
  .signOut()
  .execute();
```
```text/x-sh
curl -X "POST" "https://<serviceID>-<projectID>.wedeploy.io/oauth/revoke" \
     -H 'Content-Type: application/json' \
     -d $'{
        "token": "<user-token>"
      }'
```

</article>

<article id="9">

## Set User Scopes

User scopes allow you to choose what kind of role or access you provide to each user. This could include things like grouping team users or providing admin access to your app. In order to setup user scopes, you must declare a `supportScope` when creating or updating a user.

**Creating user with user roles**

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .auth('your-master-token')
  .createUser({
    email: 'user@domain.com',
    password: 'abc',
    supportedScopes: ["someScope"]
  })
  .then(function(user) {
    // Successfully created.
  })
  .catch(function(err) {
    // Not created.
  });
```
```text/x-sh
curl -X "POST" "https://<serviceID>-<projectID>.wedeploy.io/users" \
     -H 'Authorization: Bearer <your-project-master-token>' \
     -H 'Content-Type: application/json' \
     -d '{
        "email": "user@domain.com",
        "password": "abc",
        "supportedScopes: ["someScope"]
      }'
```

**Updating user with user roles**

```javascript
WeDeploy
  .auth('https://<serviceID>-<projectID>.wedeploy.io')
  .auth('your-master-token')
  .updateUser(userId, {
    password: "password",
    email: "eleven@hawkinslabs.com",
    name: "Eleven",
    supportedScopes: ["friends", "dont", "lie"]
  })
  .then(function() {
    // Successfully updated.
  })
  .catch(function(err) {
    // Not updated.
  });
```
```text/x-sh
curl -X "PATCH" "https://<serviceID>-<projectID>.wedeploy.io/users/<userID>" \
     -H 'Authorization: Bearer <your-project-master-token>' \
     -H 'Content-Type: application/json' \
     -d $'{
        "password": "password",
        "email": "eleven@hawkinslab.com",
        "name": "Eleven",
        "photoUrl": "https://hawkinslabs.com/011/profile.jpg",
        "supportedScopes": ["friends", "dont", "lie"]
      }'
```

</article>

<aside>

###### <span class="icon-16-alert"></span> Attention

When setting more than one value to the list of `supportedScopes`, it works as an `AND` logical operator.

</aside>

## What's next?

Learn how to sign-in users using their [email and password](/docs/auth/sign-in-with-password/).
