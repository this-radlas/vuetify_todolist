<template>
  <div class="home">
 <v-list
      flat
    >

          <v-text-field
            v-model="newTaskTitle"
            @click:append="addTask"
            @keyup.enter="addTask"
            class="pa-3"
            outlined
            label="Add task"
            append-icon="mdi-plus"
            hide-details
            clearable
            
          ></v-text-field>

    

      <v-list-item-group
        multiple
        v-for="task in tasks"
        :key = "task.id"
      >
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'blue lighten-5' : task.done }"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="task.done"
                color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
              :class="{ 'text-decoration-line-through' : task.done }"
              >{{ task.title }}</v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn 
               icon
               @click.stop="deleteTask(task.id)"
              >
                <v-icon 
                color="primary"
                >
                mdi-trash-can</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>

        <v-divider></v-divider>

      </v-list-item-group>
    </v-list>
    <div class="pa-3">
    <v-alert
      elevation="2"
      type="success"
      :value="alert"
      transition="fade-transition"
    >
      Task added!
    </v-alert>
</div>
    <div v-if="!this.tasks.length" class="text-center">
         <v-icon 
                color="primary lighten-3"
                size="80"
                >
                mdi-check</v-icon>
        <p>No task</p>
    </div>
  </div>



</template>


<script>
  export default {
    data: () => ({ 
      alert: false,
      newTaskTitle: '',
      tasks: [],
    }),
    methods: {
        doneTask(id) {
          let task = this.tasks.filter(task => task.id === id)[0];
          task.done = !task.done;
        },
        deleteTask(id) {
            this.tasks = this.tasks.filter(task => task.id !== id);
        },
        addTask() {
          let newTask = {
            id: Date.now(),
            title: this.newTaskTitle,
            done: false
          }
          this.tasks.push(newTask)
          this.newTaskTitle = ''

          this.alert = true
        },
        hide_alert: function (event) {
          window.setInterval(() => {
            this.alert = false;
          }, 3000)    
        }
     },
      mounted: function () {
        if(alert){
          this.hide_alert();
        }
      }
  }
</script>