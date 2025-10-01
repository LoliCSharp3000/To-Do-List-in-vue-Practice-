<template>
  <div>
    <h1>Proyecto</h1>
    <button @click="OpenModal()">Create</button>
    <div style="margin-top: 1rem;">
      <transition-group name="list" tag="div" style="margin-top: 1rem;">
        <button
        v-for="btn in orderedButtons"
        :key="btn.id"
        @click="actionButton(btn)"
        :class="{ tachado: btn.hecha}"
        >
          {{ btn.texto }}
        </button>
      </transition-group>
      
    </div>
  </div>
  <transition name="fade">
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
  </transition>
  
</template>

<style>
  button{
    transition: all 0.3s ease;
  }
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
    transform: scale(1.05);
  }
  .fade-enter-active, .fade-leave-active{
    transition: opacity 0.4s;
  }
  .fade-enter, .fade-leave-to{
    opacity: 0;
  }
  .list-enter-active, .list-leave-active{
    transition: all 0.3s ease; 
  }
  .list-enter-from{
    opacity: 0;
    transform: translateY(-10px);
  }
  .list-leave-to{
    opacity: 0;
    transform: translateY(10px);
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
    computed:{
      orderedButtons(){
        return [...this.Buttons].sort((a, b) => {
          return (b.hecha === true) - (a.hecha === true);
        });
      }
    },
    mounted(){
        this.loadLocalStorage();
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
          this.saveLocalStorage()
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
        this.saveLocalStorage();
        
      },
      saveLocalStorage(){
        localStorage.setItem("buttons", JSON.stringify(this.Buttons))
        localStorage.setItem("contadorID", this.contadorID) 
      },
      loadLocalStorage(){
        const savedButtons = localStorage.getItem("buttons")
        const savedContador = localStorage.getItem("contadorID")

        if(savedButtons){
          this.Buttons = JSON.parse(savedButtons)
        }
        if(savedContador){
          this.contadorID = parseInt(savedContador)
        }
      }
      
      
    }
    
  }
</script>