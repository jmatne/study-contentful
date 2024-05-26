# Contentful Project

This project demonstrates the use of the Contentful platform for managing and delivering digital content. It includes setting up Contentful, testing APIs using Curl and Postman, and forked a react project to test Contentful's functionalities with my entries and assets.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Testing APIs](#testing-apis)
  - [GraphQL](#graphql)
  - [Testing React](#testing-react)
- [Troubleshooting](#troubleshooting)


## Installation

1. **Create a Contentful Account**
   - Sign up for a free account on Contentful.[here](https://www.contentful.com/).

2. **Set Up Contentful Space**
   - Create a content model and add entries and assets via the Contentful UI.

3. **API Keys**
   - Create API keys for different environments (Production and Sandbox) to test the Content Delivery API (CDA) and Content Management API (CMA).

4. **Set Up Development Environment**
   - Install VirtualBox from [here](https://www.virtualbox.org/wiki/Linux_Downloads).
   - Download an Ubuntu LTS image from [here](https://releases.ubuntu.com/focal/).
   - Install Node.js and npm.
   - Install the Contentful SDK as per the [Contentful JS SDK GitHub](https://github.com/contentful/contentful.js).

## Usage

### Testing APIs

#### Get Space with Authorization Header
```bash
curl --location 'https://cdn.contentful.com/spaces/c8vet3y9jhg9' \
--header 'Authorization: Bearer QrOkl7IlJwfFytXSg7fjkxVFJ376F7ptp4cGrH8kr-8'
```
#### Get Space with Authorization Header
```bash
curl --location 'https://cdn.contentful.com/spaces/c8vet3y9jhg9?access_token=QrOkl7IlJwfFytXSg7fjkxVFJ376F7ptp4cGrH8kr-8'
```
#### Post with CMA token to create new entry
```bash
curl --location 'https://api.contentful.com/spaces/c8vet3y9jhg9/environments/SandBox/entries' \
--header 'Content-Type: application/vnd.contentful.management.v1+json' \
--header 'X-Contentful-Content-Type: author' \
--header 'Authorization: Bearer blablabla' \
--data '{"fields": {
                "name": {
                    "en-US": "Juan"
                },
                "surName": {
                    "en-US": "Matne"
                }
            }
}'

```
#### UPDATE an entry (attention to the version header has to be the current version).
```bash
curl --location --request PUT 'https://api.contentful.com/spaces/c8vet3y9jhg9/environments/SandBox/entries/3w07i4RMcS8U85UiWVopGE' \
--header 'X-Contentful-Content-Type: author' \
--header 'Content-Type: pplication/vnd.contentful.management.v1+json' \
--header 'X-Contentful-Version: 3' \
--header 'Authorization: Bearer blablabla' \
--data '{"fields": {
                "name": {
                    "en-US": "Juan"
                },
                "surName": {
                    "en-US": "Updated from CMA"
                }
            }
}'

```
#### Tested Contentful JS Delivery Library.
```js
const contentful = require('contentful')
const client = contentful.createClient({
  // This is the space ID. A space is like a project folder in Contentful terms
  space: 'c8vet3y9jhg9',
  // This is the access token for this space. Normally you get both ID and the token in the Contentful web app
  accessToken: 'blablabla',
  environment: 'SandBox',
})
// This API call will request an entry with the specified ID from the space defined at the top, using a space-specific access token
client
  .getEntry('2R781ypsAmyeRm17ZBQ7TL')
  .then((entry) => console.log(entry))

```
```js
juan@Ub-ThinkPad-X390:~/Desktop/ContentfulProject$ node app.js 
{
  metadata: { tags: [] },
  sys: {
    space: { sys: [Object] },
    id: '2R781ypsAmyeRm17ZBQ7TL',
    type: 'Entry',
    createdAt: '2024-05-23T11:55:40.870Z',
    updatedAt: '2024-05-23T11:55:40.870Z',
    environment: { sys: [Object] },
    revision: 1,
    contentType: { sys: [Object] },
    locale: 'en-US'
  },
  fields: { name: 'Juan', surName: 'Matne' }
}
```
### GraphQL

#### Get all Assets
```bash
https://graphql.contentful.com/content/v1/spaces/c8vet3y9jhg9/environments/SandBox

query {
    assetCollection  {
        items {
            title
            description
            contentType
            fileName
            size
            url
            width
            height
        }
    }
}
)
```
#### Get a Single asset by ID
```bash
https://graphql.contentful.com/content/v1/spaces/c8vet3y9jhg9/environments/SandBox

query Asset {
    asset(id: "1u7Oi8I51Gt21YSz7SFyFr") {
        title
        description
        contentType
        fileName
        size
        url
        width
        height
    }
}
```

### Testing React

Forked the project shared in this email [here](https://codesandbox.io/s/hx0de). and got it working with my contentful account, tested some blog posts from my account to make sure it's working fine, and also tested the schedule.

[here](https://codesandbox.io/p/sandbox/react-contentful-forked-9twdlq)


## Troubleshooting 

*Ensure that the environment is set correctly in your API calls. The default environment is master.
*Verify that the space ID, access tokens, and content type IDs are correctly specified.
*Common error: NotFound - Ensure the entry ID and environment are correct.
