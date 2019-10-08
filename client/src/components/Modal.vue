<template>
  <div class="modal" id="modal">
  <div class="modal-background"></div>
  <div class="modal-card">
    
    <header class="modal-card-head">
      <p class="modal-card-title">Modal title</p>
      <button class="delete" aria-label="close" @click="close"></button>
    </header>
    
    <section class="modal-card-body">

      <div class="field">
        <label class="label">{{'Title'}}</label>
        <div class="control">
          <input class="input" type="text" placeholder="e.g Alex Smith" v-model="book.title">
        </div>
      </div>

      <div class="field">
        <label class="label">{{'Author'}}</label>
        <div class="control">
          <input class="input" type="text" placeholder="e.g. alexsmith@gmail.com" v-model="book.author">
        </div>
      </div>

      <div class="field">
        <label class="checkbox">
          <input type="checkbox" v-model="book.read">
          {{'Read?'}}
        </label>
      </div>


    </section>
    
    <footer class="modal-card-foot">
      <button class="button is-success" @click="saveOrUpdate">{{show_operation}}</button>
      <button class="button" @click="close">{{"Cancel"}}</button>
    </footer>
  
  </div>

</div>

</template>

<script>
export default {

  name: 'Modal',

  props:['book', 'operation'],

  data() {

    return {
        
        operations: {

          0: 'save',
          
          1: 'update'
        
        }
    
    }

  },
  
  methods:{
    
    close() {

        const modal = document.getElementById('modal');

        modal.classList.remove('is-active');

    },

    saveOrUpdate() {

      if(this.book.id)

        this.$emit('update', this.book);
      
      else 

        this.$emit('save', this.book);

    }

  },

  computed:{

    show_operation(){
      return this.operations[this.operation];
    }

  }

}
</script>

<style>

</style>