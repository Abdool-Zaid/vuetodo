<template>
  <!-- <h1 class="pa-4">to do page</h1> -->
  

<v-list-item-group
  v-model="settings"
  multiple
> 
<div v-for="task in tasks" :key="task.id">

  <v-list-item
  @click="executedTask(task.id)"
  :class="{'blue lighten-4':task.status}"
  >
  <template v-slot:default="{}">
    <v-list-item-action>
      <v-checkbox
      :input-value="task.status"
      color="primary"

      ></v-checkbox>
    </v-list-item-action>
    
    <v-list-item-content>
      <v-list-item-title
      :class="{'text-decoration-line-through':task.status}"
      >
        {{task.title}}</v-list-item-title>
      <v-list-item-subtitle
      :class="{'text-decoration-line-through':task.status}"
      >
        {{task.subtitle}}</v-list-item-subtitle>
    </v-list-item-content>
    <v-list-item-action>
          <v-btn icon 
          @click.stop="nextStep(task.id)"
          >
            <v-icon color="primary lighten-4">mdi-delete</v-icon>
          </v-btn>
        </v-list-item-action>
  </template>
</v-list-item>

<v-divider></v-divider>
</div>

<v-divider></v-divider>

      <v-row justify="center" class="pt-6">
    <v-col
      cols="12"
      sm="10"
      md="8"
      lg="6"
    >
      <v-card ref="form" hide-details >
        <v-card-text>
          <v-text-field
            ref="title"
            v-model="title"
            label="task"
              required
              clearable
          ></v-text-field>
          <v-text-field
            ref="subtitle"
            v-model="subtitle"
            clearable
         
            label="subtitle"
          ></v-text-field>
     
        </v-card-text>
        <v-divider class="mt-12"></v-divider>
        <v-card-actions>
          
          <v-btn
          color="primary"
          text
          @click="progressForward()"
          >
          Submit
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-col>
</v-row> 
</v-list-item-group>
</template>

<script>

  export default {
    name: 'Home',
    data: () => ({
      title:'',
subtitle:'',
      settings: [],
      tasks:[
        {
          id:1,
          title:'stay hungry',
          subtitle:'stay foolish',
          status:false,
        },
      ],
    }),
methods:{
  executedTask(id){
let task= this.tasks.filter(task=>task.id==id)[0] 
task.status=!task.status
},
nextStep(id){
  this.tasks= this.tasks.filter(task=>task.id!==id) },
  progressForward(){
let newTask={
  id:this.tasks.length+1,
  title:this.title,
  subtitle:this.subtitle,
  status:false,

}
this.tasks.push(newTask)
this.title=''
this.subtitle=''
  },
},

  }
</script>
