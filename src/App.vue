<template>
  <div id="app" class="container">
    <img src="./assets/logo.png">
    <router-view></router-view>
    <div class="panel-body">
      <table class="table table-stripped">
        <thead>
          <tr>
            <th>Title</th>
            <th>Author</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="book in books">
            <td><a v-bind:href="book.url">{{book.title}}</a></td>
            <td>{{book.author}}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Add New Books</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <!-- Title -->
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <!-- Author -->
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <!-- URL -->
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import FireBase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: 'AIzaSyA5Bn7sjvs29EqnjbztqSWRWd7Regrq5o0',
  authDomain: 'vuejs-firebase-01-5ab50.firebaseapp.com',
  databaseURL: 'https://vuejs-firebase-01-5ab50.firebaseio.com/',
  storageBucket: 'vuejs-firebase-01-5ab50.appspot.com',
  messagingSenderId: '346193167287',
  projectId: 'vuejs-firebase-01-5ab50'
}

let app = FireBase.initializeApp(config)
let db = app.database()

let booksRef = db.ref('books')

export default {
  name: 'app',

  firebase: {
    books: booksRef
  },

  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'http://'
      }
    }
  },
  methods: {
    addBook: function () {
      booksRef.push(this.newBook)
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = 'http://'
    },

    removeBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Book removed sucessfully')
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
