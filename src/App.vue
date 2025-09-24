<template>
  <div>
    <h1>Proyecto</h1>
    <button @click="OpenModal()">Create</button>
    <div style="margin-top: 1rem;">
      <button
      v-for="btn in Buttons"
      :key="btn.id"
      @click="actionButton(btn)"
      :class="{ tachado: btn.hecha}"
      >
        {{ btn.texto }}
      </button>
    </div>
  </div>
  
  <div v-if="showModal" class="modal">
    <div class="modalContent">
      <h3>name a new button</h3>
      <input v-model="nameButton" placeholder="Type a name...">
      <div class="modalButtons">
        <button @click="CreateButton">Create</button>
        <button @click="cancel">Cancel</button>
      </div>
    </div>
  </div>
</template>

<style>
  .modal{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modalContent{
    background-color: white;
    padding: 20px;
    border-radius: 10px;
  }
  .modalButtons{
    margin-top: 10px;
    display: flex;
    gap: 10px;
  }
  .tachado{
    text-decoration: line-through;
    background-color: rgb(101, 235, 101);
  }
</style>

<script>
  export default {
    data(){
      return{
        Buttons: [],
        contadorID:0,
        showModal: false,
        nameButton: ""
      }
    },
    methods: {
      OpenModal(){
        this.nameButton = ""
        this.showModal = true
      },
      CreateButton(){
        if(this.nameButton.trim() != ""){
          this.contadorID++
          this.Buttons.push({
            id: this.contadorID,
            texto: this.nameButton,
            hecha: false
          })
        this.showModal = false
        }else{
          alert("type the name!")
        }
        
      },
      cancel(){
        this.showModal = false
      },
      actionButton(btn){
        const option = prompt("Escribe 'hecha' para marcar como hecha o 'eliminar' para borrar la tarea");
        if(!option)return;

        if(option.toLowerCase() === "hecha"){
          btn.hecha = !btn.hecha
        }else if(option.toLowerCase() === "eliminar"){
          this.Buttons = this.Buttons.filter(b => b.id !== btn.id)
        }else{
          alert("Opcion no valida!")
        }
        
      },
      /*orderedButtons(){
        return this.Buttons.slice().sort((a, b) =>{
          return (b.hecha === true) - (a.hecha === true);
        });
      }*/
    }
    
  }
</script>