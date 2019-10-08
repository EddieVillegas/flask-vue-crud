<template>
  
  <div class="container is-fluid">
    
    <table class="table">
        
        <thead>

           <button class="button is-success" @click="add(book)">{{"ADD"}}</button>

          <tr>
            
            <th>{{"Title"}}</th>
            
            <th>{{"Author"}}</th>
            
            <th>{{"Read"}}</th>
            
            <th></th>
          
          </tr>
        
        </thead>

        <tbody class="tbody">

          <tr v-for="book in books" :key="book.id">
            
            <td>{{book.title}}</td>
            
            <td>{{book.author}}</td>
            
            <td>{{book.read}}</td>
            
            <td>
              
              <div class="field is-grouped">
                
                <p class="control">
                  
                  <button class="button is-danger" @click="remove(book)">{{"DELETE"}}</button>
                
                </p>

                <p class="control">
                  
                  <button class="button is-warning" @click="edit(book)">{{"EDIT"}}</button>
                
                </p>
              
              </div>
            
            </td>
          
          </tr>

        </tbody>

    </table>

    <modal 
      :book="book"
      @save="save"
      @update="update"
      :operation="operation"
    />

 </div>

</template>

<script>

  import axios from 'axios';

  import modal from './Modal.vue'
  
  const URL = `http://127.0.0.1:5000`

  export default {

    name: 'Books',

    components:{
      
      modal
    
    },

    data() {

      return {
        
        book:{
          id: 0,
          title: '',
          author:'',
          read: false
        },

        books:[],

        operation: 0
      
      }
    
    },

    methods:{

      async getBooks(){

        try {
          
          const {data:{books}} = await axios.get(`${URL}/books`);
          
          this.books = books;

        } catch (error) {

          console.log(error);
        
        }

      },

      add(book) {

        this.book.title = '';

        this.book.author = '';

        this.book.read = false;
        
        const modal = document.getElementById('modal');

        this.operation = 0;
        
        modal.classList.add('is-active');
      
      },

      edit(book){

        const modal = document.getElementById('modal'); 
        
        modal.classList.add('is-active');

        this.operation = 1;

        this.book = {...book};

      },

      async save(book){
        
        const modal = document.getElementById('modal'); 
        
        const path = 'http://localhost:5000/books';
        
        try {
          
          await axios.post(path, this.book);
          
          modal.classList.remove('is-active');
        
        } catch (error) {
          
          console.log(error);
        
        }
        
        this.getBooks();
      },

      async remove({id}){

        const path = `http://localhost:5000/books/${id}`;

        try {
          
          await axios.delete(path);

        } catch (error) {
          
          console.log(error);

        }

        this.getBooks();
        
      },

      async update(book) {

        const {id} = book;

        const modal = document.getElementById('modal');
        
        const path = `http://localhost:5000/books/${id}`;

        try {
          
          await axios.put(path, book);

          modal.classList.remove('is-active');

        } catch (error) {
          
          console.log(error);

        }

        this.getBooks();
        
      }

    },

    created(){
      this.getBooks();
    }
  
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "~bulma/css/bulma.css";
</style>
