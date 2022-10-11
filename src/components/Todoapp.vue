<template>
  <div class="container mt-5 shadow p-3 bg-primary mb-5  rounded">
    <h2 class="text-center text-light m-3"> <img src="../assets/app-icon.png" width="80"/> Todo App <br/> <strong>Today is :{{new Date().toLocaleString()}}</strong></h2>
    <p></p>

    <!-- input -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter Text" class="form-control ">
      <button @click="submittask" class="btn btn-warning rounded-5 m-2">SUBMIT</button>
    </div>

    <!-- TASK TABLE -->
    <table class="table  table-light  table-hover mt-5">
      <thead class="table-dark">
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
   
    <tr v-for="(task,index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status==='finished'}">{{ task.name }}</span>
      </td>
      <td style="width:120px">
        <span @click="changestatus(index)" class="pointer"
        :class="{'text-danger': task.status ==='to-do',
      'text-warning': task.status ==='in-progress',
      'text-success': task.status ==='finished'}">
          {{firstCharUpper(task.status) }}</span></td>
      <td>
        <div class="pointer text-center" @click="edittask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="pointer text-center" @click="deletetask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>

  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },
  data(){
    return {
      task:'',
      editedtask:null,
      availablestatuses:['to-do', 'in-progress', 'finished'],
      tasks:[
        {
        name:'steal banana from the store',
        status:'to-do'
        },
        {
        name:'Eat My Sandwitch',
        status:'to-do'
        },{
        name:'Meet My Friend',
        status:'To do'
        },
      ]
    }
  },
  methods:{
    submittask()
    {
      if(this.task.length==0) return;

      if(this.editedtask==null)
      {
        this.tasks.push({
          name:this.task,
          status:'to do'
        });
        
      }
      else
      {
        this.tasks[this.editedtask].name=this.task;
        this.editedtask=null;

      }
      this.task='';
    },
    deletetask(index) 
    {
      this.tasks.splice(index,1);
    },
    edittask(index)
    {
      this.task=this.tasks[index].name;
      this.editedtask=index;
    },
    changestatus(index){
      let newindex=this.availablestatuses.indexOf(this.tasks[index].status);
      if(++newindex>2) newindex=0;
      this.tasks[index].status=this.availablestatuses[newindex];
    },
    firstCharUpper(str)
    {
      return str.charAt(0).toUpperCase()+str.slice(1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pointer{
    cursor: pointer;
  }
  .finished{
    text-decoration: line-through;
  }

</style>
