# Database Documentation


## Users
everyone is a user
```sql
CREATE TABLE users (
    userId BIGINT UNSIGNED PRIMARY KEY,         -- 
    email VARCHAR(128) UNIQUE NOT NULL,         --
    username VARCHAR(64) UNIQUE NOT NULL,       --
    hashedPassword CHAR(128) NOT NULL,          -- 
    bio TEXT                                    -- profile summary (md)
);
```

## Bounties
Posted bounty, start of thread
```sql
CREATE TABLE bountyThread (
    bountyThreadId BIGINT UNSIGNED PRIMARY KEY, --
    title VARCHAR(256) NOT NULL,                -- task name
    tags JSON DEFAULT '[]',                     -- relevant skills involved
    specBody MEDIUM TEXT NOT NULL,              -- markdown body
    views BIGINT UNSIGNED DEFAULT 0 NOT NULL,   -- 
    score BIGINT DEFAULT 0 NOT NULL,            -- votes
    bountyValue decimal(12, 2) DEFAULT 0 NOT NULL, -- bounty
    created DATETIME NOT NULL,                  -- when was it posted
);
```


## Solutions
```sql
CREATE TABLE bountySolutions (
    solutionId BIGINT UNSIGNED PRIMARY KEY,
    bountyThreadId BIGINT UNSIGNED REFERENCES bountyThread,
    body TEXT NOT NULL,
    created DATETIME NOT NULL,
    
);
```


## Solution comments

## Votes

## WiP Solution
user says they've started working on a solution


## Transactions
record-keeping for money

## Reports
useful to mods for banning users and stuff


## Notificaitons



