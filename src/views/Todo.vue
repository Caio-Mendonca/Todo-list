<template>
  <div class="home ">
    <v-text-field
    @click:append="addTask"
    @keyup.enter="addTask"
    v-model="NewTaskTitulo"
    class="pa-4"
    outlined
    label="Adicionar Afazer"
    append-icon="mdi-plus"
    hide-details
    clearable>
    </v-text-field>
    <v-list
      class="pt-0"
      flat
    >
    <div
    v-for="task in tasks"
    :key="task.id">
        <v-list-item
        @click='doneTask(task.id)'
        :class="{'blue lighten-5': task.done}">
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="task.done"
                color="primary"
              ></v-checkbox>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title
              :class="{'text-decoration-line-through' : task.done}">
                {{task.title}}
              </v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
          <v-btn 
          @click.stop="DeleteTask(task.id)"
          icon>
            <v-icon color="primary lighten-1">mdi-delete</v-icon>
          </v-btn>
        </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
    </div>  
    </v-list>
  </div>
</template>

<script>
  export default {
    name: 'Home',
    data(){
      return{
        NewTaskTitulo:'',
        tasks:[
          {
            id: 1,
            title: 'Acordar',
            done: false
          },
          {
            id: 2,
            title: 'Escovar os dentes',
            done: false
          },
          {
            id: 3,
            title: 'Tomar café',
            done: false
          },
          {
            id: 4,
            title: 'Ir trabalhar',
            done: false
          },
           {
            id: 5,
            title: 'Academia',
            done: false
          },
        ]
      }
    },
     mounted() {
      if (localStorage.getItem('tasks')) {
        
          this.tasks = JSON.parse(localStorage.getItem('tasks'))
        
      }
    },
    methods:{
      addTask(){
        let NewTask = {
          id: Date.now(),
          title: this.NewTaskTitulo,
          done : false
        }
        this.tasks.push(NewTask)
        this.NewTaskTitulo=''
        this.savetaks()
      },
      doneTask(id){
        let task = this.tasks.filter(task => task.id === id)[0]
        task.done = !task.done
      },
      DeleteTask(id){
        this.tasks = this.tasks.filter(task => task.id !== id)
      },
      savetaks() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem('tasks', parsed);
    }
    }
  }
</script>
