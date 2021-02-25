# Assets API

When creating their Avatar, a user chooses [assets](assetsapi.md#assets) that are used to compose the Avatar's appearance or [Closet](assetapi.md#closet). 

## Assets

An asset represents a feature of the user's avatar. It can be a piece of clothing, an accesory or a body feature. 

Some assets are common for all user avatars, but other assets are customised and need to be assigned to the user by the SDK Partner. 

#### Assets properties
 
 - **Name:** The name of the asset. e.g.: `Bunny Helmet`
 - **Image:** An image representing the asset
 - **Description:** A brief description of the asset
 - **Price:** If applicable: the price of the asset. See [Assets Universe](assetsapi.md#assets-universe)
 - **Rarity:** A representation of the availability of the asset. There are different tiers of rarity: `basic`, `very rare`. Higher rarity assets will be available in lower quanitities and will eventually go out of stock.
 - **Quantity:** If applicable: the remaining available quantity of the asset. Items with a limited quantity go out of stock when reaching the quantity limit.

 Asset representation in the API: 

```json
 {
    "displayName": "Brown Utility Vest",
    "description": "This is a beautiful Brown Utility Vest",
    "assetId": "98658ec1-6a82-4070-a863-9fe7af3b1346",
    "name": "jacket-0017-utilityvest_skin0001",
    "location": "https://s3.us-west-2.amazonaws.com/genies-cms/assetsViews%2Fmain%2Fjacket-0017-utilityvest_skin0001.png",
    "category": "jacket",
    "available": true,
    "priceUsd": null,    
    "maxInstanceCount": 300,
    "status": "visible",
    "rarity": "basic"
}
```

## Closet

The closet represents all the [assets](assetapi.md#asset) currently available to a user. More assets can be [assigned to a user](assetsapi.md#assigning-an-asset-to-a-user-s-closet)


## Assets Universe

Each SDK Partner can define their own Asset Universe. An asset universe is a list of [assets](assetsapi.md#assets) and [custom assets](assetsapi.md#Custom assets) that the users will have available when creating their Avatar. 

### Custom assets
An SDK Partner can create their own custom assets and assign them a [rarity, quantity and price](assetsapi.md#Assets properties). The SDK Partner can provide the custom assets as a user reward, have them available in a Store or assign them to a user based on other logic.

### Assets management CMS
The CMS is a tool provided to SDK Partners where you can manage:

 - default assets for your user's avatars
 - assets that are used as rewards
 - assets that appear in the Asset Store

 **Access to the CMS tool is granted in the [Partner approval process](index.md#step-one-apply-for-a-partner-account)**

## Making HTTP requests

### HTTP Request URL

The Assets API URL is: 

`https://composer-api-dev.genies.com/{{stage}}`

#### Stage
The `stage` represents the Avatar API version. The current `stage` version of the API is `v1`.

For testing purposes the `stage` should be set to `dev`

e.g.: `https://composer-api-dev.genies.com/dev`

### Authenticating requests
Request authentication is done by including the `x-api-key: {{SDK_PARTNER_API_KEY}}` header and the `Authorization: Bearer {{ID_TOKEN}}` header

#### API KEY
The `SDK_PARTNER_API_KEY` is provided for the SDK Partner when [applying for a partner account](index.md#step-one-apply-for-a-partner-account)

For testing purposes the `SDK_PARTNER_API_KEY` in the `x-api-key` header should be set to `defaultapikey-dev-0123456789`

#### ID TOKEN
The `ID_TOKEN` needed when authenticating the requests is the `IdToken` provided when the user [signs in](authentication.md#signing-in)

### Errors
When you send requests to and get responses from the Assets API, you might encounter two types of API error:

- **Client errors**: 

    Client errors are indicated by a 4xx HTTP response code. Client errors indicate that Assets API found a problem with the client request, such as an authentication failure or missing required parameters. 

- **Server errors**: 

    Server errors are indicated by a 5xx HTTP response code, and need to be resolved by the [team at Genies](support.md)

For each API error, the Authentication API returns the following values:

- **A status code**, for example, 400
- **An error message**, for example, `Access Denied`

**Example**
```
{
    "Message": "Access Denied"
}
```

### Getting the Avatar User Id

To interact with a user's Avatar you need to first get the `Avatar User Id`

To get the `Avatar User Id` call `GET` `https://composer-api-dev.genies.com/{{stage}}/user`

**Example**

```
curl --location --request GET 'https://composer-api-dev.genies.com/dev/user' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiJhNDgyY2UzMy1jODRiLTQ4YzAtODFmYy1lZGQxZTVmMGVhZTYiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImV2ZW50X2lkIjoiNDgwMzQ4NTAtNWZlOS00ODNjLWFjMTEtNGRhYmQ5OGQwOTk5IiwidG9rZW5fdXNlIjoiaWQiLCJhdXRoX3RpbWUiOjE2MTE3NzE3OTksImlzcyI6Imh0dHBzOlwvXC9jb2duaXRvLWlkcC51cy13ZXN0LTIuYW1hem9uYXdzLmNvbVwvdXMtd2VzdC0yX0VoYnRyRmlFViIsImNvZ25pdG86dXNlcm5hbWUiOiJhNDgyY2UzMy1jODRiLTQ4YzAtODFmYy1lZGQxZTVmMGVhZTYiLCJleHAiOjE2MTIwMDg5MjcsImlhdCI6MTYxMTkyMjUyNywiZW1haWwiOiJyaGFkb296b296KzFAZ21haWwuY29tIn0.EAVXUe10QTCHf7655aHFipzyNvAcOo0ggP4YOn5mMM-Tv-EbWrIsxWPqqW_0mXykKWyJIuzS2kBWs9IAto6HTZhbWndcGpLR8hyWd4Ls3tRQ2VqZ3wDQzo7rCq97Z6FK3y8dafWzDp72UVUwgfEyAvbaiMONhFAf-9YRKZVpTt7-Xj6eH-zRLfwIb6t_1f5PHmXR7KJ_UOoXerlzQEaKoSOHNkdGAsdpHpO60PLSm-j556mXFhCE2rz9rCX8HWmV1dr-VMf8owK7aWi_-EG9TJB0o6Arc5TwfM3TEO3zp7KU_qiTR5sUMvuUarj37deB56L6ae3V2SI6SoxISMVeUQ'
```

**Response**

```
[
    {
        "userId": "a482ce33-c84b-48c0-81fc-edd1e5f0eae6",
        "emailVerified": false,
        "email": "testuser@genies.com"
    }
]
```

### Getting Avatar Info

To get information about a user's avatar call `GET` `https://composer-api-dev.genies.com/{{stage}}/avatar?userId={{userId}}`

The `{{userId}}` is provided when getting the [Avatar User Id](assetsapi.md#getting-the-avatar-user-id)

The `avatarId` in the response will be used to generate photos and animations using the [Renderer API](renderer.md)

**Example**
```
curl --location --request GET 'https://composer-api-dev.genies.com/dev/avatar?userId=a482ce33-c84b-48c0-81fc-edd1e5f0eae6' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiJhNDgyY2UzMy1jODRiLTQ4YzAtODFmYy1lZGQxZTVmMGVhZTYiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImV2ZW50X2lkIjoiNDgwMzQ4NTAtNWZlOS00ODNjLWFjMTEtNGRhYmQ5OGQwOTk5IiwidG9rZW5fdXNlIjoiaWQiLCJhdXRoX3RpbWUiOjE2MTE3NzE3OTksImlzcyI6Imh0dHBzOlwvXC9jb2duaXRvLWlkcC51cy13ZXN0LTIuYW1hem9uYXdzLmNvbVwvdXMtd2VzdC0yX0VoYnRyRmlFViIsImNvZ25pdG86dXNlcm5hbWUiOiJhNDgyY2UzMy1jODRiLTQ4YzAtODFmYy1lZGQxZTVmMGVhZTYiLCJleHAiOjE2MTIwMDg5MjcsImlhdCI6MTYxMTkyMjUyNywiZW1haWwiOiJyaGFkb296b296KzFAZ21haWwuY29tIn0.EAVXUe10QTCHf7655aHFipzyNvAcOo0ggP4YOn5mMM-Tv-EbWrIsxWPqqW_0mXykKWyJIuzS2kBWs9IAto6HTZhbWndcGpLR8hyWd4Ls3tRQ2VqZ3wDQzo7rCq97Z6FK3y8dafWzDp72UVUwgfEyAvbaiMONhFAf-9YRKZVpTt7-Xj6eH-zRLfwIb6t_1f5PHmXR7KJ_UOoXerlzQEaKoSOHNkdGAsdpHpO60PLSm-j556mXFhCE2rz9rCX8HWmV1dr-VMf8owK7aWi_-EG9TJB0o6Arc5TwfM3TEO3zp7KU_qiTR5sUMvuUarj37deB56L6ae3V2SI6SoxISMVeUQ'
```

**Response**
```
[
    {
        "avatarId": "42c6a6da-1da2-4aef-a74e-feb36ae4d404",
        "ownerId": "a482ce33-c84b-48c0-81fc-edd1e5f0eae6",
        "created": 1611922828,
        "lastModified": 1611923681,
        "status": "visible",
        "gender": "male",
        "defType": "normal",
        "definition": "{\"BodyTypeLabel\":\"male\",\"Clothes\":[\"underwearBottom-0001-base_skin0000\",\"eyebrows-0006-sassy_skin0000\",\"hair-0056-tikTok_skin0000\",\"jacket-0027-floralGreenGucci_skin0000\"],\"FaceBlendshapes\":[{\"Category\":\"lips\",\"AssetName\":\"e\",\"IsEnabled\":true},{\"Category\":\"ears\",\"AssetName\":\"smallBulky\",\"IsEnabled\":true}],\"EyeColor\":\"EyeColor_DarkBrown\",\"EyebrowColor\":\"HairColor_DarkBrown\",\"FacialHairColor\":\"HairColor_DarkBrown\",\"HairColor\":\"HairColor_DarkBrown\",\"SkinColor\":\"skinColor_v0001\"}"
    }
]
```



### Assigning an asset to a user's Closet

In order for an asset to be available to a user it needs to be assigned to their Closet.

To assign an asset to a user closet you should call `PATCH` `https://composer-api-dev.genies.com/{{stage}}/user/{{userId}}/closet` and provide the assignment operation details in the request body. 

The `{{userId}}` is provided when getting the [Avatar User Id](assetsapi.md#getting-the-avatar-user-id)

**Example**

```
curl --location --request PATCH 'https://composer-api-dev.genies.com/dev/user/a482ce33-c84b-48c0-81fc-edd1e5f0eae6/closet' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiJhNDgyY2UzMy1jODRiLTQ4YzAtODFmYy1lZGQxZTVmMGVhZTYiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImV2ZW50X2lkIjoiNDgwMzQ4NTAtNWZlOS00ODNjLWFjMTEtNGRhYmQ5OGQwOTk5IiwidG9rZW5fdXNlIjoiaWQiLCJhdXRoX3RpbWUiOjE2MTE3NzE3OTksImlzcyI6Imh0dHBzOlwvXC9jb2duaXRvLWlkcC51cy13ZXN0LTIuYW1hem9uYXdzLmNvbVwvdXMtd2VzdC0yX0VoYnRyRmlFViIsImNvZ25pdG86dXNlcm5hbWUiOiJhNDgyY2UzMy1jODRiLTQ4YzAtODFmYy1lZGQxZTVmMGVhZTYiLCJleHAiOjE2MTIwMDg5MjcsImlhdCI6MTYxMTkyMjUyNywiZW1haWwiOiJyaGFkb296b296KzFAZ21haWwuY29tIn0.EAVXUe10QTCHf7655aHFipzyNvAcOo0ggP4YOn5mMM-Tv-EbWrIsxWPqqW_0mXykKWyJIuzS2kBWs9IAto6HTZhbWndcGpLR8hyWd4Ls3tRQ2VqZ3wDQzo7rCq97Z6FK3y8dafWzDp72UVUwgfEyAvbaiMONhFAf-9YRKZVpTt7-Xj6eH-zRLfwIb6t_1f5PHmXR7KJ_UOoXerlzQEaKoSOHNkdGAsdpHpO60PLSm-j556mXFhCE2rz9rCX8HWmV1dr-VMf8owK7aWi_-EG9TJB0o6Arc5TwfM3TEO3zp7KU_qiTR5sUMvuUarj37deB56L6ae3V2SI6SoxISMVeUQ' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "op": "deposit",
        "item": {
            "assetId": "2deafeed-13b5-4222-ac48-9ba66e87114a"
        }
    }
]'
```

**Response**
```
[
    {
        "assetId": "2deafeed-13b5-4222-ac48-9ba66e87114a",
        "instanceId": "94b0ff56-58b0-445b-81af-2af2218c814d",
        "inPossession": true,
        "updated": 1611928921
    }
]
```





