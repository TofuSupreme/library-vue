<!-- Using Vue-uuid for unique IDs-->
<template>
  <div class="">
  <h1> Welcome to your personal library</h1>
  <div>
  <div class="create-new-book">
  <label> Title: </label>
    <input type="text" v-model="newBookInput" @keydown.enter="addBook" placeholder="Add a title">
    <button @click="addBook">Add Book</button>
  </div>
  <div class="book" v-for="book in books" :key="book.id">
    <span>{{book.name}}</span>
    <!-- <span> {{book.author}}</span>
    <span> {{book.complete}}</span> -->
    <span class="delete-book" @click="deleteBook(book.id)">X</span>
  </div>
    <!-- <form @submit="onSu
    bmit" class="add-form">
      <label> Book Name</label>
      <input type="text" v-model="addNewBook" @keydown.enter="addBook" name="book" placeholder="Enter a book title"/>
      <label> Author</label>
      <input type="text" v-model="author" name="author" placeholder="Enter the author"/>
      <label> Completed</label>
      <input type="checkbox" v-model="completed" name="completed" placeholder="Completed"/>
      <button type="submit">Add Book</button>
    </form> -->
  </div>

  </div>
</template>

<script>
import {uuid} from 'vue-uuid';

export default {
  name: 'App',
  data() {
    return {
      books: [],
      newBookInput: '',
    }
  },
  methods: {
    addBook() {
      let newBook = {
        id: uuid.v4(),
        name: this.newBookInput
      }
      this.books.push(newBook)
console.log(newBook)
    },

    deleteBook(bookId) {

    }
  }
}
</script>
