# API documentation

## Authentication
Pretty straightforward bearer token authentication
- need to set `Authentication` header to `Bearer b64-token-from-signin`
- not all endpoints require authentication
    - like github/stackoverflow, website should work normally even if user isn't logged in

## User Endpoints

### **POST** `/api/user/signin`
Sends a login token so user can authenticate other endpoints
#### Body
```json
{
    "login" : "dansk99@outlook.com",
    "password" : "dansk99"
}
```
- login: email/username
- password: plaintext password
    - could use a hash but has to be consistent
#### Return
- **200:** Gives user their bearer token (see [#Authentication](#Authentication)
- **400:** Something is missing or invalid, should show user the response message

### **POST** `/api/user/signup`
Given relevant information, create a new user account
#### Body
```json
{
    "username" : "dvtate",
    "password" : "dansk99",
    "email" : "dansk99@outlook.com"
}
```
#### Expected Response
> `done`

#### Status 
- **200:** should redirect them to login page, not sure if we wanna add email verification phase eventually
- **400:** Invalid form data

### **GET** `/api/user/describe/:username`
Gives some relevant information about specified user
- in future will likely have activity data and usage statistics (gameification)
#### Params
- `:username`: identification for target user
#### Expected Response
`/api/user/describe/dvtate`
>    ```json
>    {
>        "userId": 2671706909569302,
>        "email": "dansk99@outlook.com",
>        "username": "dvtate",
>        "bio": "**OMG** _this_ *is* `Markdown`",
>        "createdTs": 1581126916165
>    }
>    ```
- `userId`: will likely remove this field eventually
- `email`: might hide this field too
- `username`: same as param
- `bio`: if not `null` it's a user-defined markdown text about them
    - should be rendered appropriately
- `createdTs`: join date as an epoch ms

#### Status
- **200:** relevant information as json
- **400:** invalid username

### **POST** `/api/user/edit`
Change user information/settings
#### Body
Field to edit and new value 
```json
{
    "username" : "dvtate2"
}
````
- username: change username
- password: set a new password
- bio: change bio md
- email: change email
#### Expected Response
> done

#### Status
- **200:** success
- **400:** 
- **401:**





## Thread Endpoints 
Bounties/Tasks are posted as "threads" and they have comments and other things assocaitedwith them.

### **POST** `/api/thread/create`
Post a new bounty/task
#### Body
```json
{
    "title" : "Make a game",
    "tags" : ["C++", "SDL", "Linux", "game"],
    "spec" : "need to be able to drive car on the moon"
}
```
- `title`:  quick name for bounty (limit 140 chars)
- `tags`: list of relevant tags
- `spec`: markdown specification for deliverables

#### Expected Response
On success, responds with bountyThreadId
> 23421431242131

#### Status
- 200:
- 400:
- 401:


### **GET** `/api/thread/describe/short/:bountyThreadId`
Get a quick description of bountyThread. 
- if we were stackoverflow this would be used to generate list of questions

#### Expected Response
> ```json
> {
>    "title": "Make a game",
>    "tags": "[\"C++\",\"SDL\",\"Linux\",\"game\"]",
>    "specBody": "need to be able to drive car on moon",
>    "opUserId": 7171715303648065,
>    "ts": 1581482876109,
>    "views": 8,
>    "comments": 1,
>    "solutions": 0,
>    "watching": 0,
>    "score": 1,
>    "userVote": 1
>}
>```

#### Status
- 200
- 400


### **GET** `/api/thread/describe/long/:bountyThreadId`
Get everything we have about given thread
- if we were stackoverflow this would be used to generate actual page for question

#### Expected Response
> ```json
> {
>     "title": "Make a game",
>    "tags": "[\"C++\",\"SDL\",\"Linux\",\"game\"]",
>    "specBody": "Guns and bombs and space ships. Teleport with buttons. Arrow keys for movement. 3d",
>    "opUserId": 7171715303648065,
>    "views": 8,
>    "ts": 1581482876109,
>    "score": 1,
>    "watching": [],
>    "comments": [
>        {
>            "bountyCommentId": 1,
>            "authorUserId": 7171715303648065,
>            "body": "this is a bad question, but ig it doens't matter bc it's just a test...",
>            "review": "comment",
>            "ts": 1581486140843
>        }
>    ],
>    "userVote": 1
> }
> ```

#### Status 
- 200,400


### **POST** `/api/thread/vote/:bountyThreadId/:direction`
Vote a post up/down or remove your vote
#### Params
- `bountyThreadId` : relevant bountyThread
- `direction` : `up` (+1), `down` (-1), or `null` (0)

#### Expected Return
> done

#### Status
- 200, 401, 400

### **POST** `/api/thread/watch/:bountyThreadId`
Equivalent to "I'm working on this"

#### Expected Response
> done

#### Status
- 200, 401, 400

### **POST** `/api/thread/comment/create/:bountyThreadId`
Comment/Solve the task

#### Body
```json
{
    "type" : "solution",
    "body" : "[my solution](http://github.com/dvtate/yoda)"
}
```
- type: either
    - "solution" if user wants to solve task
    - "comment" perhapse user wants clarification from OP
- body: markdown body

#### Expected Response
> done

#### Status
- 200, 401, 400
