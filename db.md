# Database Documentation

## Users
everyone is a user
```sql
CREATE TABLE users (
    userId BIGINT UNSIGNED PRIMARY KEY,
    email VARCHAR(128) UNIQUE NOT NULL,
    username VARCHAR(64) UNIQUE NOT NULL,
    hashedPassword CHAR(128) NOT NULL,
    bio TEXT,                                   -- profile summary (md)
    createdTs BIGINT UNSIGNED NOT NULL,
);
```
## Auth tokens
```sql
CREATE TABLE authTokens (
    authToken VARCHAR(64) PRIMARY KEY,
    userId BIGINT UNSIGNED REFERENCES users,
    authTokenExpiration DATETIME NOT NULL
);
```

## Bounties
Posted bounty, start of thread
```sql
CREATE TABLE bountyThreads (
    bountyThreadId BIGINT UNSIGNED PRIMARY KEY,
    title VARCHAR(256) NOT NULL,                -- task name
    tags JSON DEFAULT '[]',                     -- relevant skills involved
    specBody MEDIUMTEXT NOT NULL,              -- markdown body
    opUserId BIGINT UNSIGNED REFERENCES users,
    views BIGINT UNSIGNED DEFAULT 0 NOT NULL,
    ts BIGINT UNSIGNED NOT NULL                      -- when was it posted
);
```

## Threads Items
```sql
CREATE TABLE bountyComments (
    bountyCommentId BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    bountyThreadId BIGINT UNSIGNED REFERENCES bountyThreads,
    authorUserId BIGINT UNSIGNED REFERENCES users,
    body TEXT NOT NULL,
    review ENUM("comment", "pending-solution", "accepted", "rejected"), -- OP review
    ts BIGINT UNSIGNED NOT NULL
);
```

## Votes

### Bounty Votes
Rating for a bounty
```sql
CREATE TABLE bountyVotes (
    bountyThreadId BIGINT UNSIGNED NOT NULL REFERENCES bountyThreads,
    userId BIGINT UNSIGNED NOT NULL REFERENCES users,
    direction TINYINT(2) NOT NULL,
    ts BIGINT UNSIGNED NOT NULL -- maybe logging in future
);
```

### Solution Votes
Rating for a solution
```sql
CREATE TABLE commentVotes (
    bountyCommentId BIGINT UNSIGNED NOT NULL REFERENCES bountyComments,
    userId BIGINT UNSIGNED NOT NULL REFERENCES users,
    direction TINYINT(2) NOT NULL,
    ts BIGINT UNSIGNED NOT NULL -- maybe logging in future
);
```


## Bounty Watch
I want people to be able to see if someone else started on it without making them think they should try something else
```sql
CREATE TABLE bountyWatch (
    bountyThreadId BIGINT UNSIGNED NOT NULL REFERENCES bountyThreads,
    userId BIGINT UNSIGNED NOT NULL REFERENCES users,
    ts BIGINT UNSIGNED NOT NULL
);
```

## Bounty Transactions
```sql
CREATE TABLE bountyTransactions (
    transactionId BIGINT UNSIGNED PRIMARY KEY,
    bountyThreadId BIGINT UNSIGNED NOT NULL REFERENCES bountyThreads,
    userId BIGINT UNSIGNED REFERENCES users,
    bounty DECIMAL(12,2) NOT NULL, -- +:add to bounty  -: take reward
    cashed BOOL DEFAULT 0,
    UNIQUE(userId, bountyThreadId) -- prevents double compensation
);
```


## User Moderation
- not important rn

## Notifications
- deal with it later

## Reccomendations
- todo: learn data science