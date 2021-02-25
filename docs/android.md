# Android 

## Avatar Creator

For integrating the Avatar Creator in your Android App, see the [Avatar Creator / Android section](avatar_creator.md#android)

## Authentication API wrapper library

The Authentication API wrapper library provides a convenient way to [authenticate with your Partner API Account](authentication.md#partner-api-account-authentication)

The Android Avatar Authentication library can be found at: [https://github.com/geniesinc/android-genies-auth](https://github.com/geniesinc/android-genies-auth)

To use the Android Authentication library: 

1. Open gradle.properties file

    Add `authToken=TOKEN_PROVIDED_BY_GENIES`

2. Open build.gradle (project) file
    Add the following to allprojects / repositories
``` kotlin
maven {
    url 'https://jitpack.io'
    credentials { username authToken }
}
``` 
3. Open build.gradle (Module: app) file
    Add to dependencies
``` kotlin
implementation 'com.github.geniesinc:android-genies-auth:0.0.2-alpha'
```

### Signing in
To manage the authentication session you first need to create an `AvatarClient` and call `signIn` using your Partner API Account username and password.

If successful, sign in provides an AuthSession object containing the `IdToken` and `RefreshToken` required in other SDK interactions.

**Example**
``` kotlin
private val avatarClient = AvatarClient(context, clientId, null)
avatarClient.signIn(username, password)
    .onSuccess { authSession ->
        this.authSession = authSession
    }
    .onFailure { error -> 
        Log.e("GeniesSignIn", error.reason)
    }
```


### Getting current session
If already signed in, you can use `getSession()` to get an updated `AuthSession`. The library refreshes your tokens behind the scene. If the refresh fails, you'll get a failure of type `NoCachedSession` and need to sign in again.

**Example**
```kotlin
avatarClient.getSession()
    .onSuccess { authSession ->
        this.authSession = authSession
        // Signed in
    }
    .onFailure {
        this.authSession = null
        // Signed out
    }
```

## Avatar API wrapper Library

The Avatar API wrapper Library provides a easy way to interact with the Assets and Closet APIs. 

The Android Avatar Library can be found at: [https://github.com/geniesinc/android-genies-api](https://github.com/geniesinc/android-genies-api)

To integrate the Avatar API Library into your Android project follow the steps: 

1. Open gradle.properties file

    Add `authToken=TOKEN_PROVIDED_BY_GENIES`

2. Open build.gradle (project) file
    Add the following to allprojects / repositories
``` kotlin
maven {
    url 'https://jitpack.io'
    credentials { username authToken }
}
``` 
3. Open build.gradle (Module: app) file
    Add to dependencies
``` kotlin
implementation 'com.github.geniesinc:android-genies-api:0.0.4-alpha:dev@aar'
```
4. Use the `AvatarAPIClient`
```
  private val avatarClient: AvatarAPIClient =
        AvatarAPIClient(YOUR_API_KEY)
```

**All Avatar API Library interactions need to be authenticated with the [Partner's API Account `IdToken`](authentication.md#partner-api-account-authentication)**

### Getting a user by `username`
To get a user's `userId` use: 

```kotlin
avatarClient.getUser(idToken, username)
    .onSuccess { userInfo ->
        Log.d("UserId", userInfo.userId)
    }
    .onFailure { 
        // do something
    }
``


### Getting an user's [Avatar Closet](assetsapi.md#closet)

To get a list of all the [assets](assetsapi.md#assets) assigned to a user's closet use:
```kotlin
avatarClient.getClosetItems(idToken, userId)
    .onSuccess { closetItems ->
        // process closetItems
    }
    .onFailure { 
        // do something
    }
```

### Depositing an asset to a user's closet

To deposit an asset to a user's closet use: 
```
avatarClient.depositAsset(idToken, userId, assetId)
    .onSuccess { closetItems ->
        // process closetItems
    }
    .onFailure { 
        // do something
    }
```

### Withdrawing an asset from a user's closet

To withdraw an asset from a user's closet use: 

```
avatarClient.withdrawAsset(idToken, user.userId, assetId, assetInstanceId)
    .onSuccess { closetItems ->
        // process closetItems
    }
    .onFailure { 
        // do something
    }
```


### Creating an anonymous user

To create a managed anonymous user use: 
```
avatarClient.createUser(idToken: String, username: String)
    .onSuccess { userInfo ->
        Log.d("UserId", userInfo.userId)
    }
    .onFailure { 
        // do something
    }
```

### Deleting an anonymous user

To delete a managed anonymous user use: 
```
avatarClient.deleteUser(idToken: String, userId: String)
    .onSuccess { userInfo ->
        Log.d("UserId", userInfo.userId)
    }
    .onFailure { 
        // do something
    }
```





