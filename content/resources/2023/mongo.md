+++
title = 'Mongodb'
date = 2023-12-03T09:44:50+05:30
draft = false
tag = ["database"]
+++
# MongoDB & Database



- **MongoDB Architecture  (internal)**
    1. [MongoDB internal architecture](https://www.notion.so/MongoDB-Database-44183e27e40d4cdca5795780fccc1125?pvs=21) 
    2. [MongoDB internal architecture](https://medium.com/@hnasr/mongodb-internal-architecture-9a32f1403d6f)
    3. [MongoDB Storage Engine](https://www.mongodb.com/docs/manual/core/storage-engines/)
- **MongoDB Patterns**
    1. [https://www.mongodb.com/blog/post/building-with-patterns-a-summary](https://www.mongodb.com/blog/post/building-with-patterns-a-summary)
    2. https://youtu.be/yuPjoC3jmPA
    3. [https://mongoing.com/moutianlei](https://mongoing.com/moutianlei)
    4. .https://www.mongodb.com/blog/post/building-with-patterns-a-summary 
    5. https://medium.com/@nirjalpaudel54312/mongodb-design-patterns-708100c07bcb
    
- **Indexes**
    1. [https://www.mongodb.com/docs/manual/indexes/](https://www.mongodb.com/docs/manual/indexes/)
    2. [https://emptysqua.re/blog/optimizing-mongodb-compound-indexes/](https://emptysqua.re/blog/optimizing-mongodb-compound-indexes/)
    3. [https://www.alexbevi.com/blog/2020/05/16/optimizing-mongodb-compound-indexes-the-equality-sort-range-esr-rule/](https://www.alexbevi.com/blog/2020/05/16/optimizing-mongodb-compound-indexes-the-equality-sort-range-esr-rule/)
    4. [https://medium.com/swlh/mongodb-indexes-deep-dive-understanding-indexes-9bcec6ed7aa6](https://medium.com/swlh/mongodb-indexes-deep-dive-understanding-indexes-9bcec6ed7aa6)
    5. https://emptysqua.re/blog/optimizing-mongodb-compound-indexes/
    6. [https://medium.com/@hnasr/index-seek-vs-index-scan-in-database-systems-641c2ac811fc](https://medium.com/@hnasr/index-seek-vs-index-scan-in-database-systems-641c2ac811fc) 
    7. [ESR Rules for creating an index](https://app.box.com/notes/724995085064?s=0d8o3cnd22jpa64x4tp2prmg3zzcgwxz)
    8. ****[MongoDB index principle](https://mongoing.com/archives/2789) ( a great article explain about indexes)**
    9. [Index fill factor](https://www.notion.so/MongoDB-Database-44183e27e40d4cdca5795780fccc1125?pvs=21) 
    10. [https://dzone.com/articles/effective-mongodb-indexing-part-1](https://dzone.com/articles/effective-mongodb-indexing-part-1) 
    11. ****[MongoDB Index Building on ReplicaSet and Shard Cluster](https://www.percona.com/blog/mongodb-index-building-on-replicaset-and-shard-cluster/)****
    12. [https://betterprogramming.pub/the-mystery-of-mongodb-indexing-af61766647dc](https://betterprogramming.pub/the-mystery-of-mongodb-indexing-af61766647dc) (tell about ESR rule and how to split combo index to increase performance)
    13. [https://betterprogramming.pub/the-mystery-of-mongodb-indexing-af61766647dc](https://betterprogramming.pub/the-mystery-of-mongodb-indexing-af61766647dc) (esr and index intersection)
    14. [https://medium.com/getpowerplay/understanding-ttl-indexes-in-mongodb-comprehensively-1251e5bd77bf](https://medium.com/getpowerplay/understanding-ttl-indexes-in-mongodb-comprehensively-1251e5bd77bf) [ TTL index how it works replica won't run TTL until it becomes primary]
    15. https://towardsdev.com/database-indexing-under-the-hood-5841ac77f7de
   
- **Blogs Series**
    1. [A collection of awesome MongoDB-related articles](https://mongoing.com/anspress?ap_s=&sort=newest) 
    2. [http://learnmongodbthehardway.com/article/](http://learnmongodbthehardway.com/article/)
    3. [https://www.kenwalger.com/blog/](https://www.kenwalger.com/blog/)
    4. [MongoDB's storage structure and its impact on space usage](https://mongoing.com/blog/file-storage)
    5.  [A series to learn about wired Tigger storage engine](https://mongoing.com/guoyuanwei)
- **Documentation**
    1. [MongoDB compass doc](https://www.mongodb.com/docs/compass/current/schema/)
    2. [MongoDB Atlas doc](https://www.mongodb.com/docs/atlas/tutorial/profile-database/)
    3. [Monitoring for MongoDB](https://www.mongodb.com/docs/manual/administration/monitoring/) 
    4. [Wiredtiger  doc](https://source.wiredtiger.com/develop/arch-index.html)
   
   
- **Database concepts**
    1. [B-tree visualization tool](https://www.notion.so/MongoDB-Database-44183e27e40d4cdca5795780fccc1125?pvs=21)
    2. [Database Pages — A deep dive](https://medium.com/@hnasr/database-pages-a-deep-dive-38cdb2c79eb5)
    3. [How databases store tables on disk](https://youtu.be/DbxddGtHl70) 
    4. [Database Performance](https://fizalihsan.github.io/technology/db-performance.html)
    5. [What every programmer needs to know about DB storage](https://youtu.be/e1wbQPbFZdk) 
    6. [How SQLite work in-depth series](https://jvns.ca/blog/2014/09/27/how-does-sqlite-work-part-1-pages/) 
    7. [Database Internals](https://medium.com/databasss) ( A collection of articles explaining DB internal concepts)
    8. [https://medium.com/@hnasr/what-is-wal-write-ahead-log-a-deep-dive-a2bc4dc91170](https://medium.com/@hnasr/what-is-wal-write-ahead-log-a-deep-dive-a2bc4dc91170)  (expain what WAL and checkpoint)
    9. [https://medium.com/databasss/on-disk-io-part-1-flavours-of-io-8e1ace1de017](https://medium.com/databasss/on-disk-io-part-1-flavours-of-io-8e1ace1de017)  [ A series explain about IO indepth ]
   
- **Tools**
    1.  mongodump →- the mongodump tool reads data from the MongoDB database and creates high-fidelity BSON files. It should be noted that if the amount of data is large, mongodump may take up a lot of resources, so, to alleviate this situation, this program should be run on the secondary server.
    2. https://www.mongodb.com/docs/database-tools/mongotop/. → (provides a method to track the amount of time a MongoDB instance mongod)
    3. [https://github.com/mongolab/dex](https://github.com/mongolab/dex) (Dex is a MongoDB performance tuning tool that compares queries to the available indexes in the queried collection(s) and generates index suggestions based on simple heuristics. Currently you must provide a connection URI for your database.) 
    4. [https://www.mongodb.com/docs/database-tools/mongostat/](https://www.mongodb.com/docs/database-tools/mongostat/)
    5. [https://www.mongodb.com/docs/v2.2/reference/mongoperf/](https://www.mongodb.com/docs/v2.2/reference/mongoperf/)  (Mongoperf is a powerful tool for measuring the performance of a MongoDB deployment and identifying potential bottlenecks or performance issues.)
    6. [https://www.dbkoda.com/](https://www.dbkoda.com/) (A perfect mongodb IDE have lot of feature) 
    7. [https://medium.com/dbkoda/dbkoda-tips-and-tricks-b00ec24fc189](https://medium.com/dbkoda/dbkoda-tips-and-tricks-b00ec24fc189)
    8. https://github.com/rueckstiess/mtools →A collection of scripts to set up MongoDB test environments and parse and visualize MongoDB log files.
    
    
- **MongoDB advanced concepts**
    1. [MongoDB replica set](https://mongoing.com/archives/2155) 
    2. [How to store time-series data in MongoDB, and why that’s a bad idea](https://www.timescale.com/blog/how-to-store-time-series-data-mongodb-vs-timescaledb-postgresql-a73939734016/)
    3. [Migration of a MongoDB Replica Set to a Sharded Cluster](https://www.percona.com/blog/migration-of-a-mongodb-replica-set-to-a-sharded-cluster/)
    4. [MongoDB – Converting Replica Set to Standalone](https://www.percona.com/blog/mongodb-converting-replica-set-to-standalone/)
    5. [basic-full-text-searches-in-mongodb](https://towardsdatascience.com/how-to-do-basic-full-text-searches-in-mongodb-48b17242676?gi=41f6006a8306) 
   
- **Others**
    1. [https://stackoverflow.com/questions/8749971/can-i-query-mongodb-objectid-by-date](https://stackoverflow.com/questions/8749971/can-i-query-mongodb-objectid-by-date) 
    2. https://medium.com/@hnasr/postgres-vs-mysql-5fa3c588a94e
    3. [https://www.mongodb.com/docs/manual/reference/command/compact/](https://www.mongodb.com/docs/manual/reference/command/compact/) →Rewrites and defragments all data and indexes in a collection. (remove the empty space between the doc)
    4. [https://www.mongodb.com/docs/manual/core/inmemory/](https://www.mongodb.com/docs/manual/core/inmemory/) (We can configure mongdb to user in memory for eveything )
    5. Mongodb data lake store data in S3 ([https://www.mongodb.com/developer/products/atlas/atlas-data-lake-setup/](https://www.mongodb.com/developer/products/atlas/atlas-data-lake-setup/))
    6. Mongodb data lake ([https://medium.com/mongodb-performance-tuning/optimizing-the-mongodb-datalake-pt-1-48b408db1179](https://medium.com/mongodb-performance-tuning/optimizing-the-mongodb-datalake-pt-1-48b408db1179))
    7. wlidcard index ([https://medium.com/guys-database-blog/mongodb-4-2-wildcard-indexes-b5c54544c749](https://medium.com/guys-database-blog/mongodb-4-2-wildcard-indexes-b5c54544c749)) 
    8. [https://dzone.com/articles/mongodb-cluster](https://dzone.com/articles/mongodb-cluster) (****MongoDB Tutorials and Articles: The Complete Collection)****
    9. Use moongose lean() to which will return json insetead of moongose which is faster [https://mongoosejs.com/docs/tutorials/lean.html](https://mongoosejs.com/docs/tutorials/lean.html)
    10. https://engineering.monday.com/nice-to-meet-you-mondaydb-architecture/

- **Wired tiger**
    1. [WiredTiger File Forensics Part 1: Building “wt”](https://www.percona.com/blog/wiredtiger-file-forensics-part-1-building-wt/)
    2. [WiredTiger File Forensics Part 2: wt dump](https://www.percona.com/blog/wiredtiger-file-forensics-part-2-wt-dump/)
    3. [WiredTiger File Forensics Part 3: Viewing all the MongoDB Data](https://www.percona.com/blog/wiredtiger-file-forensics-part-3-viewing-all-the-mongodb-data/)
    4. [https://scalegrid.io/blog/mongodb-storage-engines/](https://scalegrid.io/blog/mongodb-storage-engines/) → explain frgaments 
   
- **Blogs**
    1. https://thenewstack.io/how-discord-migrated-trillions-of-messages-to-scylladb/
    2. [https://www.instacart.com/company/how-its-made/from-postgres-to-amazon-dynamodb-￼/](https://www.instacart.com/company/how-its-made/from-postgres-to-amazon-dynamodb-%EF%BF%BC/) 
    3. [https://instagram-engineering.tumblr.com/post/10853187575/sharding-ids-at-instagram](https://instagram-engineering.tumblr.com/post/10853187575/sharding-ids-at-instagram)
    4. [https://engineering.monday.com/nice-to-meet-you-mondaydb-architecture/](https://engineering.monday.com/nice-to-meet-you-mondaydb-architecture/) [How they implement their own database architecture [lambda architecture] and how they make queries fast 
    5. [https://www.wikiwand.com/en/Lambda_architecture](https://www.wikiwand.com/en/Lambda_architecture) 
    6. [Monotonic Reads using proxy in between server and DB shopify](https://shopify.engineering/read-consistency-database-replicas)  
        <details>
           <summary>how they implements the Monotonic Reads using proxy in between server and DB [My Notes]</summary> 
           <br>
           <pre>
           - Monotonic Reads
                 Imagine you have a special notebook where you write down how many candies you have each day. 
                 You also have a friend, and both of you want to make sure your candy counts are always correct, 
                 even if you write them in your notebooks at different times.
            Without Monotonic Reads:
                1. Monday: You have 5 candies in your notebook.
                2. Tuesday: Your friend has 8 candies in their notebook.
                3. Wednesday: You look in your notebook again and see you have 6 candies now.
                Uh-oh! Without monotonic reads, you might think your candy count went from 5 to 6, 
                but it should be 8 because that's what your friend saw on Tuesday. 
                It's like time went backward for your candies!
            With Monotonic Reads:
                   1. Monday: You have 5 candies in your notebook.
                   2. Tuesday: Your friend has 8 candies in their notebook.
                   3. Wednesday: You look in your notebook again, and guess what? You see 8 candies too!
            With monotonic reads, you and your friend always make sure that the number of candies only 
            goes up or stays the same, never down. This way, 
            you both agree on the order of changes, and there are no surprises.
            </pre>
        </details>
- **Profiling and monitoring**
    1. https://www.mongodb.com/docs/manual/tutorial/manage-the-database-profiler/
    2. [https://gist.github.com/rantav/3433277](https://gist.github.com/rantav/3433277) [ tricks to find slow queries]
    3. [https://medium.com/mongodb-cowboys/troubleshooting-mongodb-100-cpu-load-and-slow-queries-da622c6e1339](https://medium.com/mongodb-cowboys/troubleshooting-mongodb-100-cpu-load-and-slow-queries-da622c6e1339) [how use current op]
    4. [https://medium.com/mongodb-cowboys/quick-scan-tool-mongodb-monalize-22888e41b1fa](https://medium.com/mongodb-cowboys/quick-scan-tool-mongodb-monalize-22888e41b1fa) 
    5. [MongoDB performance training course](https://university.mongodb.com/courses/M201/about). 
    6. [Performance Best Practices: MongoDB Query Patterns and Analysis](https://mongoing.com/archives/74637)
    7. [Performance Best Practices: MongoDB Data Modeling and Memory Sizing](https://mongoing.com/archives/73797)
    8. [How to improve the performance of MongoDB](https://kevcodez.medium.com/mongodb-performance-guide-9121dff56cd1)
    9. [MongoDB best practice](https://mongoing.com/archives/3895) 
    10. [MongoDB CPU usage is high/requests are slow, how can it be broken?](https://mongoing.com/archives/3998)
    11. https://www.percona.com/blog/2021/05/24/mongodb-tuning-anti-patterns-how-tuning-memory-can-make-things-much-worse/
    12. [Getting started with MongoDB explain()](https://medium.com/mongodb-performance-tuning/getting-started-with-mongodb-explain-8a3d0c6c7e68)
    13. [https://medium.com/mongodb-performance-tuning/explaining-aggregation-pipelines-2d1edd46a341](https://medium.com/mongodb-performance-tuning/explaining-aggregation-pipelines-2d1edd46a341)  (this blog has util function to print the explain outpuit in readable format)
    14. [MongoDB 101: 5 Configuration Options That Impact Performance and How to Set Them](https://www.percona.com/blog/mongodb-101-5-configuration-options-that-impact-performance-and-how-to-set-them/)
    15. [https://medium.com/mongodb-performance-tuning](https://medium.com/mongodb-performance-tuning) (A collection blog regarding performance)
    16. [https://medium.com/shelf-io-engineering/how-to-update-63-million-records-in-mongodb-50-faster-a6dbf23ec9b3](https://medium.com/shelf-io-engineering/how-to-update-63-million-records-in-mongodb-50-faster-a6dbf23ec9b3) (by running paralley using [radsh](https://radash-docs.vercel.app/docs/getting-started) lib similar to loadash lib)
    17. https://github.com/harazdovskiy/mongo-performance ( a script to test mongodb from pervoius articel)
    18. [https://medium.com/globant/mongodb-mongoose-query-optimizations-63cfc6def9d9](https://medium.com/globant/mongodb-mongoose-query-optimizations-63cfc6def9d9)  (lean,projection,indexing and script to test the speed )
