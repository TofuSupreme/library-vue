<!-- Using Vue-uuid for unique IDs-->
<template>
  <div class="main">
    <h1> Welcome to your personal library</h1>

    <!-- <label> Title: </label>
      <input type="text" v-model="newBookInput" @keydown.enter="addBook" placeholder="Add a title">
      <button class="create-new-book" @click="addBook">Add Book</button>
    </div> -->
    <form @submit="onSubmit" class="add-form">
      <label>Book Title</label>
      <input type="text" v-model="bookTitle.value" name="bookTitle" :ref="bookTitle.ref"
        placeholder="Enter a book title" />
      <p v-if="errors.bookTitle">{{errors.bookTitle.message}} </p>
      <label>Author</label>
      <input type="text" v-model="bookAuthor.value" name="bookAuthor" :ref="bookAuthor.ref"
        placeholder="Enter the author" />
      <p v-if="errors.bookAuthor">{{errors.bookAuthor.message}} </p>
      <label>Date</label>
      <input type="date" v-model="bookDate.value" name="bookDate" :ref="bookDate.ref" placeholder="Date Completed" />
      <p v-if="errors.bookDate">{{errors.bookDate.message}} </p>
      <button type="submit">Add Book</button>
    </form>
    <div class="books" v-for="book in books" :key="book.id">
      <span>{{book.bookTitle}}</span>
      <span> {{book.bookAuthor}}</span>
    <span> {{book.bookDate}}</span>
      <button class="delete-book" @click="deleteBook(book.id)">X</button>
    </div>
  </div>
</template>

<script>
import { uuid } from 'vue-uuid';
import { useForm } from 'vue-hooks-form';

export default {
  setup() {
    const { useField, handleSubmit, errors } = useForm({
      defaultValues: {},
    })
    const bookTitle = useField('bookTitle', {
      rule:
        {
          required: true,
          message: 'Please enter a book title',
        },
    })
    const bookAuthor = useField('bookAuthor', {
      rule:
        {
          required: true,
          message: 'Please enter the author',
        },
    })
    const bookDate = useField('bookDate', {
      rule:
        {
          required: true,
          message: 'Please enter the date',
        },
    })
    const onSubmit = (values) => {
      console.log(values)
      this.addBook(values)
    }

    return {
      bookTitle,
      bookAuthor,
      bookDate,
      onSubmit: handleSubmit(onSubmit),
      errors,
    }
  },
  name: 'App',
  data() {
    return {
      books: [],
      newBookInput: '',
    }
  },
  methods: {
    addBook(data) {
      let newBook = {
        id: uuid.v4(),
        bookTitle: data.bookTitle,
        bookAuthor: data.bookAuthor,
        bookDate: data.bookDate,

      }
      this.books.push(newBook)
      console.log(newBook)
    },

    deleteBook(bookId) {
      this.books = this.books.filter(book => book.id !== bookId)
    }
  }
}
</script>
