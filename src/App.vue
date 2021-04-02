<template>
  <div id="app">
    <div class="container">
    
    <h1>Vue with Firebase</h1>
    <div class="card">
      <div class="card-header">
        <h3>Add Link</h3>
      </div>
      <div class="card-body">
          <form v-on:submit.prevent="addLink" class="form-inline">
            <div class="form-group col-md-3">
              <label for="">Title</label>
                 <input type="text" name="" v-model="newLink.title" placeholder="Title" class="form-control">
            </div>
                 <div class="form-group col-md-3">
                   <label for="">Author</label>
                 <input type="text" name="" v-model="newLink.author" placeholder="Author" class="form-control">
            </div>
                 <div class="form-group col-md-3">
                   <label for="">Url</label>
                 <input type="text" name="" v-model="newLink.url" placeholder="Url" class="form-control">
            </div>
            
             <div class="form-group m-5">
            <input type="submit" value="Add Link" class="btn btn-success">
            </div>
          </form>
      </div>

    </div>
    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Links List</h3>
      </div>
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>id</th>
              <th>Title</th>
              <th>Author</th>
              <th>Options</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="link of links">
                <td>
                {{link.id}}
              </td>
              <td>
                <a target="_blank" v-bind:href="link.url">{{link.title}}</a>
              </td>
              <td>{{link.author}}</td>
              <td>
                <button v-on:click="editLink(link)"  class="btn btn-primary"><i class="fa fa-edit"></i></button>
                <button v-on:click="deleteLink(link)"  class="btn btn-danger"><i class="fa fa-trash-o" ></i></button>
                </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import * as firebase from "firebase"
import toastr from 'toastr'

let config={
    apiKey: "AIzaSyD2W12bKh5xv8wWnmTxHUea6dWK4LoY__k",
    authDomain: "dbvueapplinks.firebaseapp.com",
    databaseURL: "https://dbvueapplinks.firebaseio.com",
    projectId: "dbvueapplinks",
    storageBucket: "dbvueapplinks.appspot.com",
    messagingSenderId: "1005008704213",
    appId: "1:1005008704213:web:786be461a0fb2774c3885c",
    measurementId: "G-E98NGGBKN4"
}

firebase.initializeApp(config)

var db=firebase.firestore()

let linksRef=db.collection('links')
var linksRefs=[];

let LoadData=()=>{
 linksRefs=[]
 this.links=[]
linksRef.onSnapshot(snap=>{
  snap.forEach(element => {

    linksRefs.push({
      id:element.id,
      ...element.data()
    })
  })
});

};
LoadData();

export default {
  name: 'App',
  data(){
    return{
      links:{}, 
      newLink:{
        title:'',author:'',url:''
      }
    }
  },firestore(){
     return {
      links: db.collection('links').orderBy('title')
     }
  },created(){

  },methods:{
    addLink(){
        linksRef.add(this.newLink)
        this.newLink.title=''
        this.newLink.url=''
        this.newLink.author=''
        toastr.success('Link Add')
        
    },
    deleteLink(link){
      console.log(link);
      linksRef.doc(link.id).delete()
      toastr.success('Links Removed')
    }, editLink(link){
      this.newLink=link
      toastr.success('Links Edit')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
