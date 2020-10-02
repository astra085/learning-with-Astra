
# Building my first set of tables using the CQL console in Astra #

After having attended recently the "Intro to Cassandra for Developers Workshop" I have experimented with what I have learnt in the workshop to further my learning journey. 
I have forked the existing repository shared in the workshop and have updated it with my  own examples.

## Explain your use case ##
```
Here I am trying to replicate the data model of a learning mmanagement system in Astra. I have created 3 tables as below:
1. curriculum_master
2. learner_master
3. learner_curriculum
```
## My  tables on Astra ##
```
Example tables that we used in the workshop:
```
## Astra Account Details ##
```
My login - aniruddha.bhattacharya.sap@gmail.com
Database User Name - ANIRUDDHAB85
Database - ABH85
```
# Connection Code #
```
Connected to caas-cluster at caas-cluster-dc-1-service:9042.
[cqlsh 6.8.0 | DSE 6.8.4.145 | CQL spec 3.4.5 | DSE protocol v2]
```
## Table: curriculum_master ##
```

ANIRUDDHAB85@cqlsh:ABHKS> CREATE TABLE IF NOT EXISTS curriculum_master (
         ...     currid uuid,
         ...     currtitle text,
         ...     currstat text,
         ...     currexpirydt timeuuid,
         ...     currsource text,
         ...     PRIMARY KEY (currid));
```
## Table: learner_master ##
```
ANIRUDDHAB85@cqlsh:ABHKS> CREATE TABLE IF NOT EXISTS learner_master (
         ...     learnerid uuid,
         ...     lfname text,
         ...     llname text,
         ...     lstat text,
         ...     lrole text,
         ...     PRIMARY KEY ((learnerid))
         ... );
```
## Table: learner_curriculum ##         
```
ANIRUDDHAB85@cqlsh:ABHKS> CREATE TABLE IF NOT EXISTS learner_curriculum (
         ...     currid uuid,
         ...     learnerid uuid,
         ...     currtitle text,
         ...     currassigndt timestamp,
         ...     currcomplstat text,
         ...     PRIMARY KEY ((currid),learnerid)
         ... ) WITH CLUSTERING ORDER BY (learnerid DESC);
```
## My tables ##
```
ANIRUDDHAB85@cqlsh:ABHKS> desc tables;

learner_curriculum  curriculum_master  users_by_city
learner_master      comments_by_user   comments_by_video
```
## Updated with my own table ##
```
learner_curriculum  curriculum_master learner_master
```
## My own data inserts, into my table curriculum_master: #
```
INSERT INTO curriculum_master (currid, currtitle, currstat, currexpirydt, currsource )
         ... VALUES ( 11111111-1111-1111-1111-111111111111, 'Introduction to Cassandra for Developers', 'active', now(),'Course Era');

INSERT INTO curriculum_master (currid, currtitle, currstat, currexpirydt, currsource )
         ... VALUES ( 21111111-1111-1111-1111-111111111111, 'Building CRUD applications with Python and NodeJS', 'active', now(),'Course Era' );

INSERT INTO curriculum_master (currid, currtitle, currstat, currexpirydt, currsource )
         ... VALUES ( 31111111-1111-1111-1111-111111111111, 'Building a Realtime Event Driven API with Kafka and Cassandra', 'active', now(),'Course Era' );

INSERT INTO curriculum_master (currid, currtitle, currstat, currexpirydt, currsource )
         ... VALUES ( 41111111-1111-1111-1111-111111111111, 'Building reactive Java applications with SPRING', 'active', now(),'Course Era' );
```
## My own data inserts, into my table learner_master: #
```
INSERT INTO learner_master ( learnerid , lfname , llname , lstat , lrole )
VALUES ( now(), 'SHERLOCK', 'HOLMES', 'active', 'manager');

INSERT INTO learner_master ( learnerid , lfname , llname , lstat , lrole )
VALUES ( now(), 'BRUCE', 'WAYNE', 'active', 'admin');

INSERT INTO learner_master ( learnerid , lfname , llname , lstat , lrole )
VALUES ( now(), 'PETER', 'PARKER', 'active', 'learner');

INSERT INTO learner_master ( learnerid , lfname , llname , lstat , lrole )
VALUES ( now(), 'TONY', 'STARK', 'inactive', 'learner');
```
## My own data inserts, into my table learner_curriculum: #
```
INSERT INTO learner_curriculum (currid , learnerid , currtitle ,currassigndt ,currcomplstat )
VALUES ( 11111111-1111-1111-1111-111111111111, cd5eb810-04c4-11eb-9480-1dee1c068ca3, 'Introduction to Cassandra for Developers', '2020-04-01 00:00+0000', 'IN PROGRESS');

INSERT INTO learner_curriculum (currid , learnerid , currtitle ,currassigndt ,currcomplstat )
VALUES ( 111111111-1111-1111-1111-111111111111, cd6e2160-04c4-11eb-9480-1dee1c068ca3, 'Building CRUD applications with Python and NodeJS', '2020-04-01 00:00+0000', 'IN PROGRESS');

INSERT INTO learner_curriculum (currid , learnerid , currtitle ,currassigndt ,currcomplstat )
VALUES ( 21111111-1111-1111-1111-111111111111, cd5eb810-04c4-11eb-9480-1dee1c068ca3, 'Introduction to Cassandra for Developers', '2020-04-01 00:00+0000', 'IN PROGRESS');

INSERT INTO learner_curriculum (currid , learnerid , currtitle ,currassigndt ,currcomplstat )
VALUES ( 21111111-1111-1111-1111-111111111111, cd6e2160-04c4-11eb-9480-1dee1c068ca3, 'Building CRUD applications with Python and NodeJS', '2020-04-01 00:00+0000', 'IN PROGRESS');
```
# curriculum_master output : #
```
 currid                               | currexpirydt                         | currsource | currstat | currtitle
--------------------------------------+--------------------------------------+------------+----------+---------------------------------------------------------------
 41111111-1111-1111-1111-111111111111 | 725a81c0-04c3-11eb-9480-1dee1c068ca3 | Course Era |   active |               Building reactive Java applications with SPRING
 21111111-1111-1111-1111-111111111111 | 6aa9d160-04c3-11eb-9480-1dee1c068ca3 | Course Era |   active |             Building CRUD applications with Python and NodeJS
 31111111-1111-1111-1111-111111111111 | 6e51cac0-04c3-11eb-9480-1dee1c068ca3 | Course Era |   active | Building a Realtime Event Driven API with Kafka and Cassandra
 11111111-1111-1111-1111-111111111111 | 65f54a00-04c3-11eb-9480-1dee1c068ca3 | Course Era |   active |                      Introduction to Cassandra for Developers
 ```
 # learner_master output : #
 ```
 learnerid                            | lfname   | llname | lrole   | lstat
--------------------------------------+----------+--------+---------+----------
 cd5eb810-04c4-11eb-9480-1dee1c068ca3 | SHERLOCK | HOLMES | manager |   active
 cd6e2160-04c4-11eb-9480-1dee1c068ca3 |    PETER | PARKER | learner |   active
 cee6deb0-04c4-11eb-9480-1dee1c068ca3 |     TONY |  STARK | learner | inactive
 cd5fc980-04c4-11eb-9480-1dee1c068ca3 |    BRUCE |  WAYNE |   admin |   active
```
# learner_curriculum : #
 ```
 currid                               | learnerid                            | currassigndt                    | currcomplstat | currtitle
--------------------------------------+--------------------------------------+---------------------------------+---------------+---------------------------------------------------
 21111111-1111-1111-1111-111111111111 | cd6e2160-04c4-11eb-9480-1dee1c068ca3 | 2020-04-01 00:00:00.000000+0000 |   IN PROGRESS | Building CRUD applications with Python and NodeJS
 21111111-1111-1111-1111-111111111111 | cd5eb810-04c4-11eb-9480-1dee1c068ca3 | 2020-04-01 00:00:00.000000+0000 |   IN PROGRESS |     Introduction to Cassandra for Developers
 11111111-1111-1111-1111-111111111111 | cd5eb810-04c4-11eb-9480-1dee1c068ca3 | 2020-04-01 00:00:00.000000+0000 |   IN PROGRESS |     Introduction to Cassandra for Developers
```
## selecting data statements ##
```
select * from curriculum_master;
select * from learber_master;
select * from learner_curriculum;

```
##  Experimenting with CRUD and showing the outputs - Updating table learer_curriculum: ##
```
UPDATE learner_master 
SET llname = 'Pettigrew' 
WHERE learnerid = cd6e2160-04c4-11eb-9480-1dee1c068ca3;

```
## Verifying updated record in table learer_curriculum: ##
```
select * from learner_master where learnerid = cd6e2160-04c4-11eb-9480-1dee1c068ca3;

learnerid                            | lfname | llname    | lrole   | lstat
--------------------------------------+--------+-----------+---------+--------
 cd6e2160-04c4-11eb-9480-1dee1c068ca3 |  PETER | Pettigrew | learner | active

```
## Deleting a record from table learer_curriculum: ##
```
DELETE FROM learner_curriculum  
WHERE currid = 11111111-1111-1111-1111-111111111111 and learnerid = cd5eb810-04c4-11eb-9480-1dee1c068ca3;

```
## verifying records of table learer_curriculum: ##
```
select * from learner_curriculum;
```
 currid                               | learnerid                            | currassigndt                    | currcomplstat | currtitle
--------------------------------------+--------------------------------------+---------------------------------+---------------+---------------------------------------------------
 21111111-1111-1111-1111-111111111111 | cd6e2160-04c4-11eb-9480-1dee1c068ca3 | 2020-04-01 00:00:00.000000+0000 |   IN PROGRESS | Building CRUD applications with Python and NodeJS
 21111111-1111-1111-1111-111111111111 | cd5eb810-04c4-11eb-9480-1dee1c068ca3 | 2020-04-01 00:00:00.000000+0000 |   IN PROGRESS |     Introduction to Cassandra for Developers

