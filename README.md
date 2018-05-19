# mongo-appbase-sync
```diff
- NOTE: This package is not maintained anymore.
- If you want to help, please reach out to dieynedrame@gmail.com
```

Meteor Mongo Appbase Sync
=========================

Automatically sync a Mongo collection to an Appbase index.

Installation  
------------

``` sh
npm install appbase-js

meteor add ddrame:mongo-appbase-sync
```

Methods
-------

**MongoCollection.syncAppBase(App, options)**  

Keeps track of all documents inserted / updated / removed within a Mongo collection, and performs the equivalent operations to an Appbase index.

```javascript
// Server-only
var Appbase = require("appbase-js")
var app = new Appbase({
  app: "myapp",
  url: "https://xxxxxxx:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@scalr.api.appbase.io",
});
function transformShop(doc){
  let doc1={};
    doc1['id']=doc._id;
    doc1['name']=doc.name;
    doc1['address']=doc.address;
    doc1['shopType']=doc.shopType;
    doc1['geoloc']=doc.geoloc;

  return doc1;
}
function transformProduct(doc){
  var shop=Shops.findOne({"_id":doc._id});
  var doc2={};
    doc2['id']=doc._id;
    doc2['name']=doc.name;
    doc2['shop']=shop.name;
  return doc2;
}
//Products and Shops are a Mongo collections
// e.g : import { Shops, Products} from "/lib/collections";

Shops.initAppBase(app,{type:"shops",transform:transformShop});
Shops.syncAppBase(app,{type:"shops",transform:transformShop});

Products.initAppBase(app,{type:"products",transform:transformProduct});
Products.syncAppBase(app,{type:"products",transform:transformProduct});
```

Options:
- transform (function): Allows to transform the documents that will be saved to Appbase .

**MongoCollection.initAppBase(App, options)**  

Performs the initial sync of all the documents in the given Mongo collection to the given Appbase index.

```javascript
// Server-only
MongoCollection.initAppBase(App, {
  transform: function(doc){
    // here transformation + return
  }
});
```
Options (same as above + the following):
- mongoSelector: Mongo selector for the cursor that will be synced to Appbase (```{}``` by default).
- mongoOptions: Mongo options for the cursor that will be synced to Appbase (```{}``` by default).

Notes
-----


