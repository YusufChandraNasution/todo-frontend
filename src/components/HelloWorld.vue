<template>
  <div>
    <div>
      <h1>Makanan Favorit !</h1>
    </div>
    <ul>
      <li v-for="item in todos" :key="item.id">
        {{item.deskripsi}} 
        <button @click="hapus(item.id)"> X </button>
      </li>
    </ul>
    <input v-model="myText" />
    <button @click="tambah">Add</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  created: function() {
    axios.get('http://localhost:8090/todo')
    .then((result)=>{
      this.todos = result.data
    })
  },
  data : function() {
    return {
      todos : [],
      myText : '',
      lastId : null
    }
  },
  methods : {
    tambah: function () { 
      let newItem = {deskripsi: this.myText} 
      axios.post('http://localhost:8090/todo', newItem)
      .then(() => {
        this.todos.push(newItem)
        this.myText = ""
      })
    },
    hapus: function (id) {
      axios.delete(`http://localhost:8090/todo/${id}`)
      .then(()=>{
        var filter = this.todos.filter((item) => item.id != id)
        this.todos = filter
      })
    }
  }
}
</script>