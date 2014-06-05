learningMongo
=============


References:
MongoDB In Action (book)
http://docs.mongodb.org/manual/
http://api.mongodb.org/python/2.7rc0/tutorial.html
http://techblog.garethdwyer.co.za/2013/10/a-basic-web-app-using-flask-and-mongodb.html
http://hidekiitakura.wordpress.com/2013/04/29/flask-rest-api-with-mongodb/
getbootstrap.com
https://github.com (weicht)



Download mongodb for osx:  mongodb-osx-x86_64-2.4.9
Unzip and put in a safe place (ie. /Applications/)

Put the mongo executables in your PATH by symlinking /Applications/mongodb…/bin/* to /usr/local/sbin
  - Note this worked for me as i have /usr/local/sbin in my PATH already.  Could do this other ways.

Create the default location for the mongo db data directory:
sudo mkdir -p /data/db/
sudo chown `id -u` /data/db


To start mongoDB server:
mongod
- This will use the default database named “test”

#################
Chriss-MacBook-Pro:~ chrisweicht$ mongod
mongod --help for help and startup options
Tue Apr 15 18:31:53.994 [initandlisten] MongoDB starting : pid=1071 port=27017 dbpath=/data/db/ 64-bit host=Chriss-MacBook-Pro.local
Tue Apr 15 18:31:53.995 [initandlisten] 
Tue Apr 15 18:31:53.995 [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
Tue Apr 15 18:31:53.995 [initandlisten] db version v2.4.9
Tue Apr 15 18:31:53.995 [initandlisten] git version: 52fe0d21959e32a5bdbecdc62057db386e4e029c
Tue Apr 15 18:31:53.995 [initandlisten] build info: Darwin bs-osx-106-x86-64-2.10gen.cc 10.8.0 Darwin Kernel Version 10.8.0: Tue Jun  7 16:32:41 PDT 2011; root:xnu-1504.15.3~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
Tue Apr 15 18:31:53.995 [initandlisten] allocator: system
Tue Apr 15 18:31:53.995 [initandlisten] options: {}
Tue Apr 15 18:31:53.995 [initandlisten] journal dir=/data/db/journal
Tue Apr 15 18:31:53.995 [initandlisten] recover : no journal files present, no recovery needed
Tue Apr 15 18:31:54.009 [websvr] admin web console waiting for connections on port 28017
Tue Apr 15 18:31:54.009 [initandlisten] waiting for connections on port 27017
#################


Note port above is 27017.  This is the native driver port.  localhost:27017 will confirm this.

To see diagnostics, localhost:27017 says to add 1000 to the port.  localhost:28017 gives info re Mongo running on your machine.

You use the mongoDB shell to interact with mongo.  This uses javascript.

Start the mongo shell:
mongo

#####################
Chriss-MacBook-Pro:~ chrisweicht$ mongo
MongoDB shell version: 2.4.9
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
Tue Apr 15 18:31:53.995 [initandlisten] 
Tue Apr 15 18:31:53.995 [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
> 
#####################

Show list of databases
show dbs

Switch to a database named “tutorial”
use tutorial

Databases have Collections within them.
Show list of collections
show collections

Insert a user into the “users” collection:
db.users.insert({username: "smith"})

Update or Insert a user into the “users” collection with the save command:
db.users.save({username: "smith"})
- had we passed in an _id field, then it would look for that doc and do an update instead of insert

Find all docs in the Collection:
db.users.find()

determine how many docs in the Collection:
db.users.count()

Find a given doc by using the “query selector” which is also a document:
db.users.find({username: "smith"})

Update a property in a document with the $set command:
db.users.update({username: "smith"}, {$set: {country: "Canada"}})

Remove a property:
db.users.update({username: “smith”}, {$unset: {country: 1}})
- Note that the query selector must match only one document for this to work.  O/w, it does nothing to any of them.

- To apply to all username smith’s:
db.users.update({username: "smith"}, {$unset: {country: 1}}, false, true)
  - ‘false’ 3rd arg (ignore for now)
  - ‘true’ 4th arg means this is a multi-update

Add elements to a list:
$push (adds to a list)
$addtoSet (adds to a list but uniquely)


To delete an entire Collection:
db.foo.remove()
- will remove all docs in the foo Collection

To remove specific docs, use the query selector:
db.users.remove({"username": "jones"})


To remove an entire collection entirely:
db.users.drop()






