This is a simple design which I saw the video during my way to work
https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly


The major things to note here was

1) Generation of keys base62 encoding and all

we will use redis `INCR` command 

Just refresh this before interview maybe useful in any system when we need to generate unique keys

There was twitter's snowflake algo which was introduced 
