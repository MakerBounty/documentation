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

## Bounties
Posted bounty, start of thread
```sql
CREATE TABLE bountyThread (
    bountyThreadId BIGINT UNSIGNED PRIMARY KEY,
    title VARCHAR(256) NOT NULL,                -- task name
    tags JSON DEFAULT '[]',                     -- relevant skills involved
    specBody MEDIUM TEXT NOT NULL,              -- markdown body
    views BIGINT UNSIGNED DEFAULT 0 NOT NULL,
    score BIGINT DEFAULT 0 NOT NULL,            -- votes
    ts BIGINT UNSIGNED NOT NULL                      -- when was it posted
);
```

## Solutions
```sql
CREATE TABLE bountySolutions (
    solutionId BIGINT UNSIGNED NOT NULL,
    bountyThreadId BIGINT UNSIGNED REFERENCES bountyThread,
    authorUserId BIGINT UNSIGNED REFERENCES users,
    body TEXT NOT NULL,
    review ENUM("accepted", "rejected"), -- OP review
    ts BIGINT UNSIGNED NOT NULL
);
```

## Solution comments
for example, questions
```sql
CREATE TABLE bountyComments (
    commentId BIGINT UNSIGNED NOT NULL,
    bountyThreadId BIGINT UNSIGNED REFERENCES bountyThread,
    authorUserId BIGINT UNSIGNED REFERENCES users,
    body TEXT NOT NULL,
    ts BIGINT UNSIGNED NOT NULL
);
```

## Votes

### Bounty Votes
Rating for a bounty
```sql
CREATE TABLE bountyVotes (
    bountyThreadId BIGINT UNSIGNED NOT NULL REFERENCES bountyThread,
    userId BIGINT UNSGINED NOT NULL REFERENCS users,
    direction TINYINT(2) NOT NULL,
    ts BIGINT UNSIGNED NOT NULL -- maybe logging in future
);
```
### Solution Votes
Rating for a solution
```sql
CREATE TABLE solutionVotes (
    solutionId BIGINT UNSGINED NOT NULL REFERENCES bountySolutions,
    userId BIGINT UNSIGNED NOT NULL REFERENCES users,
    direction TINYINT(2) NOT NULL,
    ts BIGINT UNSIGNED NOT NULL -- maybe logging in future
);
```


## Bounty Watch
I want people to be able to see if someone else started on it without making them think they should try something else
```sql
CREATE TABLE bountyWatch (
    bountyThreadId BIGINT UNSIGNED NOT NULL REFERENCES BountyThread,
    userId BIGINT UNSIGNED NOT NULL REFERENCES users,
    ts BIGINT UNSIGNED NOT NULL
);
```

## Bounty Transactions
```sql
CREATE TABLE bountyTransactions (
    transactionId BIGINT UNSIGNED PRIMARY KEY,
    userId BIGINT UNSIGNED REFERENCES users,
    value DECIMAL(12,2) NOT NULL,
    direction ENUM("add", "take") NOT NULL
);
```


## User Moderation


## Notifications
