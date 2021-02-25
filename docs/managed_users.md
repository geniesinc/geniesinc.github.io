# Managed anonymous Avatar user accounts

Managed users are anonymous Avatar accounts created on a Partner's behalf. A managed user gets associated a random idenitifer int the form of the Avatar `userId`.

**To create anonymous Avatar users you need to have access to a [Partner API Account](authentication.md#the-partner-api-account)**

Managing anonymous Avatar users can be done through the [**Android Library**](android.md/#creating-an-anonymous-user), the **iOS Library** or through **HTTP Requests**. 

## Making HTTP requests

The Assets API URL is: 

`https://composer-api-dev.genies.com/{{stage}}`

#### Stage
The `stage` represents the API version. The current `stage` version of the API is `v1`.

For testing purposes the `stage` should be set to `dev`

e.g.: `https://composer-api-dev.genies.com/dev`

### Authenticating requests
Request authentication is done by including the `x-api-key: {{SDK_PARTNER_API_KEY}}` header and the `Authorization: Bearer {{ID_TOKEN}}` header

#### API KEY
The `SDK_PARTNER_API_KEY` is provided for the SDK Partner when [applying for a partner account](index.md#step-one-apply-for-a-partner-account)

For testing purposes the `SDK_PARTNER_API_KEY` in the `x-api-key` header should be set to `defaultapikey-dev-0123456789`

#### ID TOKEN
The `ID_TOKEN` needed when authenticating the requests is the `IdToken` provided when you [Sign In with your Partner API Account](authentication.md#partner-api-account-authentication)

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

### Creating an Anonymous Avatar User

To create an anonymous Avatar user call `POST` `https://composer-api-dev.genies.com/{{stage}}/user` and provide a username in the request body. 

The `userId` in the response is a random identifier associated with the created user. **The `userId` should be saved and associated with your internal user account**

**Example**
```
curl --location --request POST 'https://composer-api-dev.genies.com/dev/user' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiI0MjQyNjMyOC0zMDU3LTQ1ZWUtYTZhYy0xNjhlZmNkNzhkNzEiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiY29nbml0bzpncm91cHMiOlsiYXBwIl0sImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJldmVudF9pZCI6ImE3Y2I5NmZlLWVkODAtNDQyYy04N2FlLWJiYjc1YzE2NmRhYiIsInRva2VuX3VzZSI6ImlkIiwiYXV0aF90aW1lIjoxNjEyNTMyMzkzLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb21cL3VzLXdlc3QtMl9FaGJ0ckZpRVYiLCJjb2duaXRvOnVzZXJuYW1lIjoiNDI0MjYzMjgtMzA1Ny00NWVlLWE2YWMtMTY4ZWZjZDc4ZDcxIiwiZXhwIjoxNjEyNjE4NzkzLCJpYXQiOjE2MTI1MzIzOTMsImVtYWlsIjoiYXBpK3Rlc3RpbmdAZ2VuaWVzLmNvbSJ9.ViDZy4oM0SeI1_bvjoPhE8JmZSxpV_zx6bJOq-F3FxPOriJaKH1PlhdeWrl8CT0bsgJDQzT-cd0YItxPF1Uk5W7_0yPDBBU-Ubo3YEspSY0Zy43SFXgK2FIMLSZxQH9hAJq38wijmJXM6PI6-c2alCD0fNrhbrjvgmT1MTs5pPHJnZEMOqEG9V_BUco448glXitBUlwOqmmplS8eH8uSbyao6MF_ZjsuRNXlpwxJFqhPLMYumwxoyo3wFajHaA-LTQI0D849WlCCj6rmXkUzvzFRoFBXq6lp_WCzaQdCSnpkIW7lScJOzm2RikuMdSEYlnysPk8iMTHIFVDrpp9zFA' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "funkyuser"
}'
```

**Response**
```json
{
    "userId": "8b3f002d-3552-4f82-8a8e-6017a1b454d1",
    "username": "funkyuser"
}
```

### Getting the userId for an anonymous user

To get the associated `userId` for a user call `GET` `https://composer-api-dev.genies.com/{{stage}}/user?username={{username}}`

The `username` should be the same `username` provided when [creating the anonymous Avatar user](#creating-an-anonymous-avatar-user).

**Example**
```
curl --location --request GET 'https://composer-api-dev.genies.com/dev/user?username=funkyuser' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiI0MjQyNjMyOC0zMDU3LTQ1ZWUtYTZhYy0xNjhlZmNkNzhkNzEiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiY29nbml0bzpncm91cHMiOlsiYXBwIl0sImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJldmVudF9pZCI6ImE3Y2I5NmZlLWVkODAtNDQyYy04N2FlLWJiYjc1YzE2NmRhYiIsInRva2VuX3VzZSI6ImlkIiwiYXV0aF90aW1lIjoxNjEyNTMyMzkzLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb21cL3VzLXdlc3QtMl9FaGJ0ckZpRVYiLCJjb2duaXRvOnVzZXJuYW1lIjoiNDI0MjYzMjgtMzA1Ny00NWVlLWE2YWMtMTY4ZWZjZDc4ZDcxIiwiZXhwIjoxNjEyNjE4NzkzLCJpYXQiOjE2MTI1MzIzOTMsImVtYWlsIjoiYXBpK3Rlc3RpbmdAZ2VuaWVzLmNvbSJ9.ViDZy4oM0SeI1_bvjoPhE8JmZSxpV_zx6bJOq-F3FxPOriJaKH1PlhdeWrl8CT0bsgJDQzT-cd0YItxPF1Uk5W7_0yPDBBU-Ubo3YEspSY0Zy43SFXgK2FIMLSZxQH9hAJq38wijmJXM6PI6-c2alCD0fNrhbrjvgmT1MTs5pPHJnZEMOqEG9V_BUco448glXitBUlwOqmmplS8eH8uSbyao6MF_ZjsuRNXlpwxJFqhPLMYumwxoyo3wFajHaA-LTQI0D849WlCCj6rmXkUzvzFRoFBXq6lp_WCzaQdCSnpkIW7lScJOzm2RikuMdSEYlnysPk8iMTHIFVDrpp9zFA'
```

**Response**
```json
[
    {
        "userId": "8b3f002d-3552-4f82-8a8e-6017a1b454d1",
        "username": "funkyuser"
    }
]
```

### Deleting an anonymous user

**Deleting an anonymous user will destroy the user's ability to access their Avatar**

To delete an anonymous user call `DELETE` `https://composer-api-dev.genies.com/{{stage}}/user/{{userId}}`

The `{{userId}}` should be the `userId` provided when creating the anonymous user.

**Example**
```
curl --location --request DELETE 'https://composer-api-dev.genies.com/dev/user/8b3f002d-3552-4f82-8a8e-6017a1b454d1' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiI0MjQyNjMyOC0zMDU3LTQ1ZWUtYTZhYy0xNjhlZmNkNzhkNzEiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiY29nbml0bzpncm91cHMiOlsiYXBwIl0sImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJldmVudF9pZCI6ImE3Y2I5NmZlLWVkODAtNDQyYy04N2FlLWJiYjc1YzE2NmRhYiIsInRva2VuX3VzZSI6ImlkIiwiYXV0aF90aW1lIjoxNjEyNTMyMzkzLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb21cL3VzLXdlc3QtMl9FaGJ0ckZpRVYiLCJjb2duaXRvOnVzZXJuYW1lIjoiNDI0MjYzMjgtMzA1Ny00NWVlLWE2YWMtMTY4ZWZjZDc4ZDcxIiwiZXhwIjoxNjEyNjE4NzkzLCJpYXQiOjE2MTI1MzIzOTMsImVtYWlsIjoiYXBpK3Rlc3RpbmdAZ2VuaWVzLmNvbSJ9.ViDZy4oM0SeI1_bvjoPhE8JmZSxpV_zx6bJOq-F3FxPOriJaKH1PlhdeWrl8CT0bsgJDQzT-cd0YItxPF1Uk5W7_0yPDBBU-Ubo3YEspSY0Zy43SFXgK2FIMLSZxQH9hAJq38wijmJXM6PI6-c2alCD0fNrhbrjvgmT1MTs5pPHJnZEMOqEG9V_BUco448glXitBUlwOqmmplS8eH8uSbyao6MF_ZjsuRNXlpwxJFqhPLMYumwxoyo3wFajHaA-LTQI0D849WlCCj6rmXkUzvzFRoFBXq6lp_WCzaQdCSnpkIW7lScJOzm2RikuMdSEYlnysPk8iMTHIFVDrpp9zFA'
```

**Response**
The response is empty.

### Getting a list of users

You can get a list of list of anonymous users created on behalf of your Partner API Account. 

To get the user list call `GET` `https://composer-api-dev.genies.com/{{stage}}/user`


**Example**
```
curl --location --request GET 'https://composer-api-dev.genies.com/dev/user?username=funkyuser' \
--header 'x-api-key: defaultapikey-dev-0123456789' \
--header 'Authorization: Bearer eyJraWQiOiJGckhlSlBKS1pUeUo0SkR3em9zQTNjYUM0MUNnSkJxZ0FGdUw5N1MwV3hBPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiI0MjQyNjMyOC0zMDU3LTQ1ZWUtYTZhYy0xNjhlZmNkNzhkNzEiLCJhdWQiOiIybnE4OXI1YjRzdHVnNWxzc2dsZmJncG5tIiwiY29nbml0bzpncm91cHMiOlsiYXBwIl0sImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJldmVudF9pZCI6ImE3Y2I5NmZlLWVkODAtNDQyYy04N2FlLWJiYjc1YzE2NmRhYiIsInRva2VuX3VzZSI6ImlkIiwiYXV0aF90aW1lIjoxNjEyNTMyMzkzLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb21cL3VzLXdlc3QtMl9FaGJ0ckZpRVYiLCJjb2duaXRvOnVzZXJuYW1lIjoiNDI0MjYzMjgtMzA1Ny00NWVlLWE2YWMtMTY4ZWZjZDc4ZDcxIiwiZXhwIjoxNjEyNjE4NzkzLCJpYXQiOjE2MTI1MzIzOTMsImVtYWlsIjoiYXBpK3Rlc3RpbmdAZ2VuaWVzLmNvbSJ9.ViDZy4oM0SeI1_bvjoPhE8JmZSxpV_zx6bJOq-F3FxPOriJaKH1PlhdeWrl8CT0bsgJDQzT-cd0YItxPF1Uk5W7_0yPDBBU-Ubo3YEspSY0Zy43SFXgK2FIMLSZxQH9hAJq38wijmJXM6PI6-c2alCD0fNrhbrjvgmT1MTs5pPHJnZEMOqEG9V_BUco448glXitBUlwOqmmplS8eH8uSbyao6MF_ZjsuRNXlpwxJFqhPLMYumwxoyo3wFajHaA-LTQI0D849WlCCj6rmXkUzvzFRoFBXq6lp_WCzaQdCSnpkIW7lScJOzm2RikuMdSEYlnysPk8iMTHIFVDrpp9zFA'
```

**Response**

The first user in the list will always represent the **Partner API user account** and will be have the `"groups": ["app"]` property.

```json
[
    {
        "userId": "42426328-3057-45ee-a6ac-168efcd78d71",
        "groups": [
            "app"
        ],
        "emailVerified": false,
        "email": "demo_partner_api_account@genies.com"
    },
    {
        "userId": "6f3c6283-a102-4e33-8058-c1a0aa02feb6",
        "parentUserId": "42426328-3057-45ee-a6ac-168efcd78d71",
        "username": "funkyuser"
    },
    {
        "userId": "17af0ade-cb1c-42ad-8518-e5e94b701267",
        "parentUserId": "42426328-3057-45ee-a6ac-168efcd78d71",
        "username": "deadbeef"
    },
    ...
    ...
    ...
]
```
