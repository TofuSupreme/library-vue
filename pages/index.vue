<!-- Using Vue-uuid for unique IDs-->
<template>
  <main>
    <h1> Welcome to your personal library</h1>
    <FormulateForm
    v-model="bookValues"
    :key="bookFormKey"
    type="form"
    @submit="addBook"
    >
    <FormulateInput
    v-model="bookValues.bookTitle"
    type="text"
    name="bookTitle"
    label="Title"
    placeholder="Enter the title of the book"
    validation="required|min:3"
    error-behavior="blur"
      outer-class=""
      label-class=""
      inner-class=""
      input-class=""
      help-class=""
      error-class=""
      errors-class=""
    />
    <FormulateInput
    v-model="bookValues.bookAuthor"
    type="text"
    name="bookAuthor"
    label="Author"
    placeholder="Enter Author Name"
    validation="required|min:3"
    error-behavior="blur"
      outer-class=""
      label-class=""
      inner-class=""
      input-class=""
      help-class=""
      error-class=""
      errors-class=""
    />
  <FormulateInput
  type="date"
  name="bookDate"
  label="Date Completed"
  placeholder="Enter Date Completed"
  validation="optional"
  error-behavior="blur"
      outer-class=""
      label-class=""
      inner-class=""
      input-class=""
      help-class=""
      error-class=""
      errors-class=""
  />

<FormulateInput
  type="submit"
  label="Submit"
  outer-class="border p-2 hover:bg-green-500 cursor-pointer"
  />
</FormulateForm>

<ul>
  <li v-for="book in books" :key="book.id">
  {{book.bookTitle}} by {{book.bookAuthor}} - {{book.bookDate}}

    <span class="cursor-pointer hover:border" @click="removeBook(book.id)">X</span>
  </li>
  </ul>
</main>
</template>

<script>
import { uuid } from 'vue-uuid';

export default {
  name:'IndexPage',

  data() {
    return {
      books: [],
      bookValues: {
        bookTitle: '',
        bookAuthor: '',
        bookDate: '',
      },
      bookFormKey: 0,
    }
  },
async getAll(){
  const response = await fetch('https://og3ufes8l5.execute-api.ap-northeast-1.amazonaws.com/books');
  this.books = await response.json()
},
  methods: {
    async addBook(data) {
      let newBook = {
        id: uuid.v4(),
        bookTitle: data.bookTitle,
        bookAuthor: data.bookAuthor,
        bookDate: data.bookDate,

      }
      await fetch('https://og3ufes8l5.execute-api.ap-northeast-1.amazonaws.com/books', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newBook),
  })
      this.books.push(newBook)
      this.bookFormKey = this.bookFormKey + 1
      console.log(newBook)
    },

    removeBook(bookId) {
      this.books = this.books.filter(book => book.id !== bookId)
    }
  }
}
</script>
