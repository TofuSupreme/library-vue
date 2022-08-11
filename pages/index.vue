<template>
  <div class="">
  <h1> Welcome to your personal library</h1>
  <div>
  <div class="create-new-book">
  <label> Title: </label>
    <input type="text" v-model="newBook" @keydown.enter="addBook" placeholder="Add a title">
    <button @click="addBook">Add</button>
  </div>
    <!-- <form @submit="onSubmit" class="add-form">
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
export default {
  name: 'IndexPage'
}
</script>
