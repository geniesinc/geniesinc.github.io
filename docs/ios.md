# iOS

## Avatar Creator

For integrating the Avatar Creator in your Android App, see the [Avatar Creator / iOS section](avatar_creator.md#ios)

## Authentication & Avatar API wrapper library

The Authentication API wrapper library provides a convenient way
to [authenticate with your Partner API Account](authentication.md#partner-api-account-authentication)

The iOS Avatar Authentication library can be found
at: [https://github.com/geniesinc/GeniesAvatarKit](https://github.com/geniesinc/GeniesAvatarKit)

## Installation

You can install the iOS Authentication library via **Cocoapods**:

If you just want to use just the API Client:

1. Add `pod "GeniesAvatarKit/AvatarAPIClient", git: "https://github.com/geniesinc/GeniesAvatarKit.git"` (proper github credentials are
   required)
   
If you want to also use the Avatar Creator:

1. Add `pod "GeniesAvatarKit", git: "https://github.com/geniesinc/GeniesAvatarKit.git"` (proper github credentials are
   required)

2. Disable Bitcode in XCode project settings - In XCode go to your `project settings` > `Build Settings` > Make
   sure `All` is selected and search for `bitcode` - Set it to `Enabled = No`

![Screenshot](img/disable_bitcode.png)

## Usage

### State

The SDK provides an `AvatarAppState` object for handling the Genies Avatar API, which will handle the session and API
calls

The `AvatarAppState` object is the central point of the SDK and requires an initialization with the configuration
objects presented in [Configuration](#configuration).

There should only be one instance of the `AvatarAppState` running throughout the app's lifecycle. The code samples below
will reference this by `AvatarAppState.shared`, which should be a singleton instance of `AvatarAppState`

### Configuration

Configuring the `AvatarAppState` consists of 3 parts:

1. One `AvatarAPIConfig` object, configured with an `api key` and a `base URL` for the Genies Avatar API. Example:

``` swift
AvatarAPIConfig(apiKey: "some-api-key",
                baseURL: URL(string: "https://avatar-api-dev.genies.com/dev")!)
```

2. One `AvatarAWSConfig` configured with `client ID`, `AWS Region`, `username` and `password`. Example:

```
AvatarAWSConfig(clientID: "some-client-id",
                region: .usWest2,
                username: "some-username",
                password: "some-password")
```

3. One `AvatarCredentialsStore` instance which should handle storing and getting credentials for communicating with the
   Genies Avatar API. The example app provides a predefined `KeychainCredentialsStore` which is used to manage
   credentials using the underlying iOS Keychain.

If needed You can build your own credentials store by implementing the `AvatarCredentialsStore` protocol, which requires
two methods:

- `storeCredentials(_ credentials: AvatarClientCredentials)` - should store the `IdToken` and `refreshToken` strings
  from the `AvatarClientCredentials` instance
- `getStoredCredentials()` - should provide an instance of `AvatarClientCredentials` that were previously stored, if any
  exists

These methods are called automatically by the SDK

### Signing in with a Partner API Account

Signing in is done using the `AvatarAppState` shared instance:

```swift
AvatarAppState.shared.signIn { result in
   switch result {
   case .success:
       // Sign in was successful
   case .failure(let error):
       // There was an error while signing in
       print(error.localizedDescription)
   }
}
```

Signing out from the Partner API Account is done by calling:

```swift
AvatarAppState.shared.signOut()
```

This operation will cleanup the internal state of the `AvatarAppState` instance, and you'll need to sign in again in
order to use the Genies Avatar API.

### Getting a user by `username`

To get a user's `userId` use:

```swift
AvatarAppState.shared.client.getUser(username: username) { result in
   switch result {
   case .success(let user):
       // Getting the user operation was successful
       // In this step you get a User object with userId and username string properties
   case .failure(let error):
       // There was an error while getting the user
       print(error.localizedDescription)
   }
}
```

### Getting an user's [Avatar Closet](assetsapi.md#closet)

To get a list of all the [assets](assetsapi.md#assets) assigned to a user's closet use:

```swift
AvatarAppState.shared.client.getCloset(userId: userId) { result in
    switch result {
    case .success(let closetItems):
        // Getting the closet operation was successful
        // In this step you get a list of [ClosetItem] objects
    case .failure(let error):
        // There was an error while getting the user
        print(error.localizedDescription)
    }
}
```

### Depositing an asset to a user's closet

To deposit an asset to a user's closet use:

```swift
AvatarAppState.shared.client.deposit(userId: userId, assetId: assetId) { result in
    switch result {
    case .success(let closetItems):
        // Depositing the closet item operation was successful
        // In this step you get a list of [ClosetItem] objects
    case .failure(let error):
        // There was an error while getting the user
        print(error.localizedDescription)
    }
}
```

### Withdrawing an asset from a user's closet

To withdraw an asset from a user's closet use:

```swift
AvatarAppState.shared.client.withdraw(userId: userId, assetId: assetId, instanceId: instanceId) { result in
    switch result {
    case .success(_):
        // Withdrawing the closet item operation was successful
    case .failure(let error):
        // There was an error while getting the user
        print(error.localizedDescription)
    }
}
```

### Creating an anonymous user

To create a managed anonymous user use:

```swift
AvatarAppState.shared.client.createUser(username) { result in
    switch result {
    case .success(let user):
        // Creating the user operation was successful
        // In this step you get a User object
    case .failure(let error):
        // There was an error while creating the user
        print(error.localizedDescription)
    }
}
```

### Deleting an anonymous user

To delete a managed anonymous user use:

```swift
AvatarAppState.shared.client.deleteUser(username) { result in
    switch result {
    case .success(_):
        // Deleting the user operation was successful
    case .failure(let error):
        // There was an error while deleting the user
        print(error.localizedDescription)
    }
}
```