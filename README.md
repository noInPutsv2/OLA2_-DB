# OLA2_-DB
Our frontend is displayed with HTML and JS.
To view the content of the database in the frontend, the backend (Node.js) must be runned.
The command for running the backend is "node server.mjs". (Must be runned inside frontend_v2 folder)

## Instruction for setting up DB
### Create Docker Network
```docker network create mongoCluster```

### Starting the MongoDB Instances
```docker run -d --rm -p 27017:27017 --name mongo1 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --shardsvr --bind_ip localhost,mongo1```

```docker run -d --rm -p 27018:27017 --name mongo2 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --shardsvr --bind_ip localhost,mongo2```

```docker run -d --rm -p 27019:27017 --name mongo3 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --shardsvr --bind_ip localhost,mongo3```


### Initiate the Replica Set
```
docker exec -it mongo1 mongosh --eval "rs.initiate({
 _id: \"myReplicaSet\",
 members: [
   {_id: 0, host: \"mongo1\"},
   {_id: 1, host: \"mongo2\"},
   {_id: 2, host: \"mongo3\"}
 ]
})"
```
### Populating data
We are using MongoDB Compass to populate our data into our database. This is done by connection to primary replica, and uploading the documents. 
