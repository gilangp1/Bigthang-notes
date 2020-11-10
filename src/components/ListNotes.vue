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
export default {
     name: 'ListNotes',
     data: function(){
          return{
               notes : [
                    { id: 1, title: 'Wegodev', description: 'Ini adalah jalan ninjaku' },
                    { id: 2, title: 'BigThang', description: 'Bersabarlah giliranmu akan datang'}
               ]
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

          this.$root.$emit('emitForm', dataForm);
        }
     },
     mounted(){
          this.$root.$on('emitRemoveNote', data => {
               let noteIndex = this.notes.findIndex(note => note.id === data.id);
               this.notes.splice(noteIndex, 1);
          });
          this.$root.$on('emitUpdateNote', data => {
               let noteIndex = this.notes.findIndex(note => note.id === data.id);
               this.notes[noteIndex].title = data.title;
               this.notes[noteIndex].description = data.description;
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