<template>
  <body>
    <h1>ToDoリスト</h1>
      <input type="radio" name="list" v-model="filter" value="all" checked="checked" >すべて
      <input type="radio" name="list" v-model="filter" value="work" >作業中
      <input type="radio" name="list" v-model="filter" value="finish" >完了
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo,index) in filteredTodos" :key="index">
            <td>{{ index+1 }}</td>
            <td>{{ todo.task }}</td>
            <td><input type="button" @click="changeStatus(todo);" :value="todo.status"></td>
            <td><input type="button" @click="deleteTodo(index);" value="削除"></td>
        </tr>
      </tbody>
    </table>
    <p>{{todos}}</p>
   
    <h1>新規タスクの追加</h1>
    <input v-model="newTask" type="text">
    <input type="button" value="追加" @click="addTasks">
  </body>
</template>

<script>

export default {

  data() {
    return{
      id: 0,
      status:'',
      newTask:'',
      todos:[ ],
      filter:'all',
 
    }
  },
  methods: {
    addTasks() {
      this.todos.push(
        { id: this.id++,task: this.newTask, status: '作業中' }
      )
      this.newTask = '';
    },
    changeStatus(todo){
      if(todo.status === '作業中'){
        const finish = {id: todo.id, task: todo.task, status:'完了'};
        this.todos.splice(todo.id,1,finish);
      }else{
        const work = {id: todo.id, task: todo.task, status:'作業中'};
        this.todos.splice(todo.id,1,work);

      }
    },
    deleteTodo(index){
      this.todos.splice(index,1)
    }
    },
  computed: {
    filteredTodos(){
    if(this.filter === 'all'){
      return this.todos
    }else if(this.filter === 'finish'){
      return this.todos.filter((todo) => {
        return todo.status === '完了' 
      });
    }else{
      return this.todos.filter((todo)=>{
        return todo.status === '作業中'
      });
    }
    }
  }
  
};
</script>
