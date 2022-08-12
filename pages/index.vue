<!-- Using Vue-uuid for unique IDs-->
<template>
  <main>
    <h1> Welcome to your personal library</h1>
    <FormulateForm
    :key="bookFormKey"
    v-model="bookValues"
    type="form"
    @submit="addBook"
    >
    <FormulateForm
    v-model="bookValues.title"
      type="text"
      name="title"
      label="Title"
      placeholder="Enter Book Title"
      validation="required|min:3"
      error-behavior="blur"
      outer-class="mb-5"
      label-class="block mb-1 font-bold text-sm"
      inner-class="max-w-md border border-gray-400 rounded-lg mb-1 overflow-hidden focus-within:border-blue-500"
      input-class="w-full h-10 px-3 border-none text-base text-grey-700 placeholder-gray-400"
      help-class="text-xs text-gray-500"
      error-class="text-red-500"
      errors-class="text-red-500"
    />
    <FormulateForm
    v-model="bookValues.author"
    type="text"
    name="author"
    label="Author"
    placeholder="Enter Author Name"
    validation="required|min:3"
    error-behavior="blur"
    outer-class="mb-5"
    label-class="block mb-1 font-bold text-sm"
    inner-class="max-w-md border border-gray-400 rounded-lg mb-1 overflow-hidden focus-within:border-blue-500"
    input-class="w-full h-10 px-3 border-none text-base text-grey-700 placeholder-gray-400"
    help-class="text-xs text-gray-500"
    error-class="text-red-500"
    errors-class="text-red-500"
    />
  <FormulateForm
  v-model="bookValues.dateCompleted"
  type="date"
  name="dateCompleted"
  label="Date Completed"
  placeholder="Enter Date Completed"
  outer-class="mb-5"
  label-class="block mb-1 font-bold text-sm"
  inner-class="max-w-md border border-gray-400 rounded-lg mb-1 overflow-hidden focus-within:border-blue-500"
  input-class="w-full h-10 px-3 border-none text-base text-grey-700 placeholder-gray-400"
  help-class="text-xs text-gray-500"
  />

<FormulateInput
  type="submit"
  label="Submit"
  outer-class="border p-2 hover:bg-green-500 cursor-pointer"
  />
</FormulateForm>

<ul>
  <li v-for="book in books" :key="book.id">
  {{book.title}} by {{book.author}} - {{book.dateCompleted}}

    <span class="cursor-pointer hover:border" @click="removeBook(book.id)">X</span>
  </li>
  </ul>
</main>
</template>

<script>
import { uuid } from 'vue-uuid';
import { z } from 'zod';

export default {
  buildModules: ['@nuxt/typescript-build'],
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
