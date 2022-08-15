<template>
  <main class="scroll-smooth flex-column">
    <h1> Welcome to your personal library</h1>
    <FormulateForm v-model="bookValues" :key="bookFormKey" type="form" @submit="addBook">
      <FormulateInput v-model="bookValues.bookTitle" type="text" name="bookTitle" label="Title" placeholder="Book title"
        validation="required|min:3" error-behavior="blur" outer-class="mb-5" label-class="block mb-1 font-bold text-sm"
        inner-class="max-w-md border border-gray-400 rounded-lg mb-1 overflow-hidden focus-within:border-blue-500 focus:outline-none focus:shadow-outline"
        input-class="w-full h-10 px-3 border-none text-base text-gray-700 placeholder-gray-400"
        help-class="text-xs text-gray-500" error-class="text-red-500" errors-class="text-red-500" />
      <FormulateInput v-model="bookValues.bookAuthor" type="text" name="bookAuthor" label="Author"
        placeholder="Author Name" validation="required|min:3" error-behavior="blur" outer-class="mb-5"
        label-class="block mb-1 font-bold text-sm"
        inner-class="max-w-md border border-gray-400 rounded-lg mb-1 overflow-hidden focus-within:border-blue-500 focus:outline-none focus:shadow-outline"
        input-class="w-full h-10 px-3 border-none text-base text-gray-700 placeholder-gray-400"
        help-class="text-xs text-gray-500" error-class="text-red-500" errors-class="text-red-500" />
      <FormulateInput v-model="bookValues.bookDate" type="date" name="bookDate" label="Date" placeholder="Date"
        validation="required" error-behavior="blur" outer-class="mb-5" label-class="block mb-1 font-bold text-sm"
        inner-class="max-w-md border border-gray-400 rounded-lg mb-1 overflow-hidden focus-within:border-blue-500 focus:outline-none focus:shadow-outline"
        input-class="w-full h-10 px-3 border-none text-base text-gray-700 placeholder-gray-400"
        help-class="text-xs text-gray-500" error-class="text-red-500" errors-class="text-red-500" />

      <FormulateInput type="submit" label="Submit" outer-class=" my-2 border p-2 hover:bg-green-500 cursor-pointer" />
    </FormulateForm>

    <div class="bg-gray-400 border-white py-2 px-2">
      <p> List of your books </p>
      <ul v-for="book in books" :key="book.id">
        <li class="py-3 px-3 mb-1 bg-white text-black rounded-lg shadow-lg space-y-2 flex-row hover:bg-green-200">
          {{book.bookTitle}} by {{book.bookAuthor}} - {{book.bookDate}}
          <button
            class="px-4 py-1 justify-end text-sm text-purple-600 font-semibold rounded-full border border-purple-200 hover:text-black hover:bg-purple-600 hover:border-transparent focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-offset-2"
            @click="removeBook(book.id)">X</button>
        </li>
      </ul>
    </div>
    <footer class="py-4"> &copy; TofuSupreme 2022 </footer>
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
