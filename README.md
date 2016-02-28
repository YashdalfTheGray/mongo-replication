# Mongo replication test

## The most important links
[Building and breaking replica sets in MongoDB](https://www.compose.io/articles/building-and-breaking-replica-sets-in-mongodb/)

## How do I do things?

First and foremost, read the most important link above!! This step is VERY IMPORTANT. 

* Pull the repo down
* Run `npm run init` to create mongo folders
* Run `npm run start-mongo1` in terminal window 1 to start the primary server
* Run `npm run import-data` another terminal window (terminal window 2) to put some data in there

### Setting up replication

In terminal window 2, run `mongo` and run the command `db.restaurants.find({}, { _id: 1 }).limit(20).forEach(printjson)`. This should print out a set of 20 object IDs.

A replica set can be set up using `rs.initiate()`. Once this is done, run `rs.status()` to check the status of the replica set. You should see `<your_replica_set_name>:SECONDARY>` at the prompt. Once a query is run, like the one above, you should see the prompt change to `<your_replica_set_name>:PRIMARY>`.

In another terminal window (terminal window 3), run `npm run start-mongo2` to start the secondary server. Back in terminal window 2, run `mongo` if you quit mongo earlier and then run `rs.add('computer_name:27018')`. Replace `computer_name` with your computer name. You should get a message saying `{ ok: 1 }`. This means that the primary knows about the secondary mongo server BUT they are still not synced.

In terminal window 4, run `mongo` and try to run a query like the one above. It should fail with an error. After the error, run `rs.slaveOk()` to sync the two mongo databases. Once the sync happens, running a query should yield the same results on both servers.
