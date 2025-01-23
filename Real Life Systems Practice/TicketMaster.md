
Source :  https://www.hellointerview.com/learn/system-design/problem-breakdowns/ticketmaster

Major things to note

1) Node query caching in elasticsearch
2) Locks in redis for 10 min reservation window
3) Map become real time for most popular events. (server side events and virtual waiting queue) Redis sorted queue we can use
Things this system covered is

1) Queuing users in case of big event
2) Distributed lock using Redis
3) Searching of events using ElasticSearch

Also watch Arpit's video on flash sale.