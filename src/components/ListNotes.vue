<template>
     <div class="listNotes">
          <ul>
               <li v-for="(row,index) in notes" :key="index">
                    <button class="btn-note" @click="editNote(row.id)">
                         <label>{{ row.title }}</label>
                         <span>{{ row.description }}</span>
                    </button>
               </li>
          </ul>
     </div>
</template>

<script>
     import axios from 'axios';
export default {
     name: 'ListNotes',
     data: function(){
          return{
               notes : []
          }
     },
     props: {
         propEditNote : {
              type : Function
         }
     },
     methods: {
          editNote(id){
               // console.log("App id: "+ id);
               let dataForm = this.notes.find(note => note.id === id);
               dataForm.mode = 'update';
               this.$root.$emit('emitForm', dataForm);
          },
          createNewId(){
               let newId = 0;
               
               if(this.notes.length === 0){
                    newId = 1;
               }else{
                    newId = this.notes[this.notes.length - 1].id + 1;
               }

               return newId;
          },
          getData(){
               axios.get('http://localhost/bigthang-notes/note').then(response => {
                    console.log(response);
                    this.notes = response.data;
               })
          }
     },
     mounted(){
          this.getData();

          this.$root.$on('emitRemoveNote', data => {
               let noteIndex = this.notes.findIndex(note => note.id === data.id);
               this.notes.splice(noteIndex, 1);
          });
          this.$root.$on('emitUpdateNote', data => {
               let noteIndex = this.notes.findIndex(note => note.id === data.id);
               
               this.notes[noteIndex].title = data.title;
               this.notes[noteIndex].description = data.description;
          });
          this.$root.$on('emitSaveNote', data => {
               let newId = this.createNewId();

               let newNote = {id : newId, 'title' : data.title, 'description' : data.description }
               this.notes.push(newNote);
               this.editNote(newId);
          })
     }
}
</script>

<style>
     ul{
          list-style: none;
          padding: 0;
          margin: 0;
     }
     .btn-note{
          border: none;
          text-align: left;
          border-bottom: 1px solid gainsboro;
          padding: 0px 15px;
          outline: none;
          cursor: pointer;
          background: #f7f7f7;
          height: 150px;
          width: 100%;
          
     }
     .btn-note:hover {
          background:#eaeaea;
     }
     .btn-note label{
          display:block;
          color: #444444;
          font-size: 1.5em;
          margin-bottom: 15px;
     }
     .btn-note span{
          color: #545454;
     }   
</style>