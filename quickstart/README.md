# from official docs

## quickstart

docker pull cassandra:latest
docker network create cassandra
docker run --rm -d --name cassandra --hostname cassandra --network cassandra cassandra

create data.cql
---BEGIN
CREATE KEYSPACE IF NOT EXISTS store WITH REPLICATION =
{ 'class' : 'SimpleStrategy',
'replication_factor' : '1'
};

CREATE TABLE IF NOT EXISTS store.shopping_cart (
userid text PRIMARY KEY,
item_count int,
last_update_timestamp timestamp
);

INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('9876', 2, toTimeStamp(now()));
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('1234', 5, toTimeStamp(now()));
---END

docker run --rm --network cassandra \
-v "$(pwd)/data.cql:/scripts/data.cql" \
-e CQLSH_HOST=cassandra -e CQLSH_PORT=9042 \
-e CQLVERSION=3.4.6 nuvo/docker-cqlsh

for windows
docker run --rm --network cassandra -v "$(pwd)/data.cql:/scripts/data.cql" -e CQLSH_HOST=cassandra -e CQLSH_PORT=9042 -e CQLVERSION=3.4.6 nuvo/docker-cqlsh