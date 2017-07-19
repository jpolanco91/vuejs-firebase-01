<template>
  <div id="app" class="container">
    <div class="panel-body">
      <!-- Vue Material table -->
      <md-table v-once>
          <md-table-header>
              <md-table-row>
                  <md-table-head>Title</md-table-head>
                  <md-table-head>Author</md-table-head>
              </md-table-row>
          </md-table-header>

          <md-table-body>
              <md-table-row v-for="book in books" :key="book.title">
                  <md-table-cell><a v-bind:href="book.url">{{book.title}}</a></md-table-cell>
                  <md-table-cell>{{book.author}}</md-table-cell>
              </md-table-row>
          </md-table-body>
      </md-table>
      <!-- Vue Material table -->
    </div>
    <!-- Input secton title -->
    <md-toolbar>
        <h3 class="md-title">Add Books</h3>
    </md-toolbar>

    <!-- Input fields -->
    <form novalidate v-on:submit.prevent="addBook">
        <md-input-container>
            <label>Title</label>
            <md-input v-model="newBook.title"></md-input>
        </md-input-container>
        <md-input-container>
            <label>Author</label>
            <md-input v-model="newBook.author"></md-input>
        </md-input-container>
        <md-input-container>
            <label>URL</label>
            <md-input v-model="newBook.url"></md-input>
        </md-input-container>
        <md-button type="submit" class="md-raised md-primary">Add Book</md-button>
    </form>
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
