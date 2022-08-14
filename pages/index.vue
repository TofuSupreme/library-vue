<template>
  <main>
    <h1> Welcome to your personal library</h1>
    <FormulateForm v-model="bookValues" :key="bookFormKey" type="form" @submit="addBook">
      <FormulateInput v-model="bookValues.bookTitle" type="text" name="bookTitle" label="Title"
        placeholder="Enter the title of the book" validation="required|min:3" error-behavior="blur" outer-class=""
        label-class="" inner-class="" input-class="" help-class="" error-class="" errors-class="" />
      <FormulateInput v-model="bookValues.bookAuthor" type="text" name="bookAuthor" label="Author"
        placeholder="Enter Author Name" validation="required|min:3" error-behavior="blur" outer-class="" label-class=""
        inner-class="" input-class="" help-class="" error-class="" errors-class="" />
      <FormulateInput type="date" name="bookDate" label="Date Completed" placeholder="Enter Date Completed"
        validation="optional" error-behavior="blur" outer-class="" />

      <FormulateInput type="submit" label="Submit" outer-class=" my-1.5 border p-2 hover:bg-green-500 cursor-pointer" />
    </FormulateForm>

    <div v-for="book in books" :key="book.id">
      <ul>
        <li class="py-3 px-3 mb-1 bg-white text-black rounded-lg shadow-lg space-y-2 flex-row">
          {{book.bookTitle}} by {{book.bookAuthor}} - {{book.bookDate}}
          <button
            class="px-4 py-1 text-sm text-purple-600 font-semibold rounded-full border border-purple-200 hover:text-black hover:bg-purple-600 hover:border-transparent focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-offset-2"
            @click="removeBook(book.id)">X</button>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
//using uuid for book id's
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
async created(){
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
        method: 'post',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newBook),
  })
      this.books.push(newBook)
      this.bookFormKey = this.bookFormKey + 1
      console.log(newBook)
    },

    async removeBook(bookId) {
      await fetch('https://og3ufes8l5.execute-api.ap-northeast-1.amazonaws.com/books', {
        method: 'delete',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({id: bookId}),
      })

      this.books = this.books.filter(book => book.id !== bookId)
    }
  }
}
</script>
