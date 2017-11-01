sudo service mongodb [start|stop|restart|status] // Linux
mongod --dbpath "d:\set up\mongodb\data" // windows
mongod --dbpath /path/to/your/db --shutdown
mongodump --host <HOST=127.0.0.1> --port <PORT=27017>
--dbpath <DB_PATH> --out <BACKUP_DIR=./dump>
--dbname <DB_NAME> --collection <COLL_NAME>
mongorestore
mongostat
mongotop <UPDATE_INTERVAL_SECONDS=1>
mongo
mongo --eval "db.getSiblingDB('admin').shutdownServer()" --port 27017 

db.help()
db.stats()
db // show current db name
show dbs // list dbs
use <DB_NAME> // use db (if doesn't exits then will be created after first insert)
db.dropDatabase() // delete currently used db

show collections
db.createCollection(<COLL_NAME>, [<OPTS>])
// OPTS: { autoindexId: false, capped: false, size: <NUM_BYTES>, max: <NUM_ENTRIES> }
db.<COLL_NAME>.drop() //delete collection
db.<COLL_NAME>.insert(<JSON_DOC>|<ARR_DOC>) // insert document to collection
db.<COLL_NAME>.save(<JSON_DOC>) // rewrite doc if _id specified, insert otherwise
db.<COLL_NAME>.find(<SEL>, [<PROJ>]) // list found docs
db.<COLL_NAME>.findOne(<SEL>) // return the first element found
// SEL: {<key>: <value>} | {<key>: {$<ne|lt|lte|gt|gte>:<value>}} |
// { <key1:val1, $<and|or>: [{key2:val2}, {key3:val3}] }
// PROJ: {<key1>: <1|0>, <key2>: <1|0>} // 0 - hide key, 1 - display
db.<COLL_NAME.update(<SEL>, {$set: {<upd_key>: <upd_val>}}, [{multi:false}])
db.<COLL_NAME.remove([<SEL>], [<JUST_ONE=false>])

db.<COLL_NAME>.find().pretty()
db.<COLL_NAME>.find().skip(<NUM_DOCS>)
db.<COLL_NAME>.find().limit(<NUM_DOCS>)
db.<COLL_NAME>.find().sort(SORT)
// SORT: {<key1>:<1|-1>, <key2>:<1|-1>}
db.<COLL_NAME>.ensureIndex(SORT, [<OPTS>])
// OPTS: { name: <IND_NAME="SORT">, background: false, unique: false, dropDupd: false, sparse: false, expireAfterSeconds: <NUM_SECONDS>, v: <IND_VER>, weights: <DOC{1..99999}>, default_language: <LANG=english>, language_override: <LANG> }
db.<COLL_NAME>.aggregate(<AGGR_OP>)
// AGGR_OP: [{ $group : {_id : "$by_user", total_likes : {$sum : "$likes"}} }]
// AGGR_STAGE: $project, $match, $group, $sort, $skip, $limit, $unwind 
// AGGR_EXPR: $sum, $avg, $min, $max, $push, $addToSet, $firts, $last


mongod --port 27017 --dbpath "D:\set up\mongodb\data" --replSet rs0
rs.status()
rs.conf()
rs.initiate()
rs.add(<HOST_NAME>:<PORT>) // only if master, max 12 nodes
db.isMaster()
//     App ---> Query router (mongos)
//              Query router (mongos)
//               /           \
//  Shard (replica set)____Config server
//  Shard (replica set)    Config server


// db1
// |-collection1
// | |-document1
// | |-document2
// |-collection2
// db2
// |-collection
