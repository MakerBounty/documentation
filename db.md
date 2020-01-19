# Database Documentation


## Users

```sql
CREATE TABLE users (
    userId BIGINT UNSIGNED PRIMARY KEY,         -- 
    email VARCHAR(128) UNIQUE NOT NULL,         --
    username VARCHAR(64) UNIQUE NOT NULL,       --
    hashedPassword CHAR(128) NOT NULL,          -- 
    bio TEXT                                    -- profile summary
);
```

## Bounties
Posting
```sql
CREATE TABLE bountyThread (
    bountyThreadId BIGINT UNSIGNED PRIMARY KEY, --
    title VARCHAR(256) NOT NULL,                -- task name
    tags JSON,                                  -- relevant skills involved
    specBody MEDIUM TEXT NOT NULL,              -- markdown body
    views BIGINT UNSIGNED DEFAULT 0,            -- 
    score BIGINT DEFAULT 0,                     -- votes
    usdValue decimal(12, 2) DEFAULT 0,
    created DATETIME,
);
```

## Bounty comments
comments on the original bounty posting
```sql
CREATE TABLE bountyComments (
    bountyCommentId BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    bountyThreadId BIGINT UNSIGNED REFERENCES bountyThread,
    created DATETIME,
    body TEXT NOT NULL,
    score INT DEFAULT 0,
);
```

## Solutions

## Solution comments

## Transactions

## Reports

## Notificaitons



