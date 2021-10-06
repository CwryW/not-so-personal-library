/*
*
*
*       Complete the API routing below
*       
*       
*/

'use strict';
const {MongoClient} = require('mongodb');
const ObjectId = require('mongodb').ObjectID;
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  title: String,
  comments: {Array, default: []},
commentcount: {Number, default: 0}
});

const Book = new mongoose.model('Book', bookSchema);

mongoose.connect(process.env.DB, {useNewUrlParser: true, useUnifiedTopology: true, useFindAndModify: false});
//.then((doc)=>{
//  console.log('mongoose connection made')});


module.exports = function (app) {

  app.route('/api/books')
    .get(function (req, res){
      //response will be array of book objects
let DB = process.env.DB;
let client = new MongoClient(DB, {useNewUrlParser: true, useUnifiedTopology: true});
client.connect(function(err){ 
       let db = client.db('db1');
       //console.log('connection made')
       let collection = db.collection('books');
    collection.find().toArray(function(error, data){
      if(error) return;

data.forEach((book)=>{
  if(!book.comments.length){
book.commentcount = 0;  
}else{
book.commentcount = book.comments.length;   
}
}) //adding commentcount to array ends
      res.json(data); 
    }) //find() ends
  }); // connect ends
  }) //.get an array of everything ends
    
    
 .post(function (req, res){
    /*
    {"result":{"ok":1,"n":1,"opTime":{"ts":"6647448311403905025","t":1}},"ops":[{"_id":"5c407b0557c5a04362f8fe21","title":"tufc","comments":[]}],"insertedCount":1,"insertedIds":["5c407b0557c5a04362f8fe21"]}
    */
        var title = req.body.title;
    var bookid = new ObjectId(req.body._id);
      var comment = req.body.comment; 
  
      //response will contain new book object including atleast _id and title
     
   if(!title){
     res.send('missing required field title');
   }else{  
     let DB = process.env.DB
    let client = new MongoClient(DB, {useNewUrlParser: true, useUnifiedTopology: true}) 
     client.connect(function(err){
       let dbName = 'db1';
       let db = client.db(dbName)
       
  var collection = db.collection('books');
    collection.insertOne({title: title, comments: comment||[]},function(err, data){       
   res.json({_id: data.ops[0]._id, title: data.ops[0].title})
    })
  }); 
  }
  })
    
  

    
 .delete(function(req, res){
      //if successful response will be 'complete delete successful'
    let DB = process.env.DB;
    let client = new MongoClient(DB, {useNewUrlParser: true, useUnifiedTopology: true})
  client.connect(function(err){
    let db = client.db('db1')
    
    var collection = db.collection('books');
      collection.deleteMany({}, function(err, doc){      
          res.send('complete delete successful');
   });
    });
    
  }); // delete all ends



   app.route('/api/books/:id')
    .get(function (req, res){
       var bookid = req.params.id;
      var obid = new ObjectId(bookid);  
    let DB = process.env.DB;
    let client = new MongoClient(DB, {useNewUrlParser: true, useUnifiedTopology: true})
     client.connect(function(err) {
       let dbName = 'db1';
       let db = client.db(dbName);
       
           var collection = db.collection('books');
      collection.find({_id: obid}).toArray(function(err, data){
     (data[0])?res.json(data[0]):
     res.send('no book exists'); //if ends    
    })//find ends
    }) // connect ends     
  }) //get id ends
  

    
    .post(function(req, res){
      let bookid = req.params.id;
      let comment = req.body.comment;
      //json res format same as .get    
          if(!comment){
      res.send('missing required field comment')  
      }else{  
Book.findByIdAndUpdate({_id: bookid},{$push:{'comments': comment}, $inc:{'commentcount': 1}}, {new: 1}, (err, doc)=>{
  (doc)?res.json(doc):res.send('no book exists');
})//findoneandupdate ends 
}//!comment if/else ends
    })// post id ends

    
    .delete(function(req, res){
      let bookid = req.params.id;
      //if successful response will be 'delete successful'
      let obid = new ObjectId(bookid);
let DB = process.env.DB;
let client = new MongoClient(DB, {useNewUrlParser: true, useUnifiedTopology: true});

client.connect((err)=>{
  let db = client.db('db1')
  let collection = db.collection('books');

  collection.findOneAndDelete({_id: obid},(err, doc)=>{
    (doc.value)?res.send('delete successful'):res.send('no book exists');
  })
})
    });//delete w/ id ends
  
};
