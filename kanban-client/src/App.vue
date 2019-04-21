<template>
  <div id="app">
    <b-loading :is-full-page="true" :active.sync="isLoading" :can-cancel="false"></b-loading>
    <navbar/>
    <div class="columns" style="margin-top: 48px;">
      <taskContainer class="column" :title=" 'Back-Log' "  :data="backLog"/>
      <taskContainer class="column" :title=" 'To-Do' " :data="toDo"/>
      <taskContainer class="column" :title=" 'In Progress' " :data="inProgress"/>
      <taskContainer class="column" :title=" 'Done' " :data="done"/>
    </div>

  </div>
</template>

<script>
import navbar from '@/components/navbar.vue'
import taskContainer from '@/components/task-container.vue'
import db from '@/api/firestore.js'

export default {
  components: { navbar, taskContainer },
  data () {
    return {
      tasks: [],
      isLoading: true
    }
  },
  created () {
    db
      .collection('tasks')
      .orderBy('createdAt')
      .onSnapshot((querySnapshot) => {
        this.tasks = []
        querySnapshot.forEach((doc) => {
          console.log(doc)
          this.tasks.push({ id: doc.id, ...doc.data() })
        })
        this.isLoading = false
      })
  },
  computed: {
    backLog () {
      return this.tasks.filter(task => task.status === 'back-log')
    },
    toDo () {
      return this.tasks.filter(task => task.status === 'to-do')
    },
    inProgress () {
      return this.tasks.filter(task => task.status === 'in-progress')
    },
    done () {
      return this.tasks.filter(task => task.status === 'done')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
