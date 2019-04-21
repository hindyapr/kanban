<template>
    <section>
        <b-collapse class="card" aria-id="contentIdForA11y3" :open=false >
            <div
                slot="trigger"
                slot-scope="props"
                class="card-header"
                role="button"
                aria-controls="contentIdForA11y3"
                >
                <p class="card-header-title">
                    {{ task.title }}
                </p>
                <a class="card-header-icon">
                    <b-icon
                        :icon="props.open ? 'menu-up' : 'menu-down'">
                    </b-icon>
                </a>
            </div>
            <div class="card-content">
                <div class="content">
                    <p>
                        {{ task.description }}
                    </p>
                    <p>
                        Person In Charge: {{ task.pic }}
                    </p>
                </div>
            </div>
            <footer class="card-footer" v-if="task.status === 'back-log'">
                <a class="card-footer-item" @click="deleteConfirm(task.id)">Delete</a>
                <a class="card-footer-item" @click="addTaskToToDo(task.id)">Add to To-Do</a>
            </footer>
            <footer class="card-footer" v-if="task.status === 'to-do'">
                <a class="card-footer-item" @click="addTaskToBackLog(task.id)">Remove to Back-Log</a>
                <a class="card-footer-item" @click="deleteConfirm(task.id)">Delete</a>
                <a class="card-footer-item" @click="addTaskToProgress(task.id)">Start Progress</a>
            </footer><footer class="card-footer" v-if="task.status === 'in-progress'">
                <a class="card-footer-item" @click="addTaskToToDo(task.id)">Revert Progress</a>
                <a class="card-footer-item" @click="deleteConfirm(task.id)">Delete</a>
                <a class="card-footer-item" @click="addTaskToDone(task.id)">Done Progress</a>
            </footer><footer class="card-footer" v-if="task.status === 'done'">
                <a class="card-footer-item" @click="addTaskToProgress(task.id)">Back to Progress</a>
                <a class="card-footer-item" @click="deleteConfirm(task.id)">Delete</a>
            </footer>
        </b-collapse>

    </section>
</template>

<script>
import db from '@/api/firestore.js'
export default {

  props: ['task'],
  methods: {
    deleteTask (id) {
      db
        .collection('tasks')
        .doc(id)
        .delete()
        .then(() => {
          this.$toast.open({
            message: 'Task is deleted!',
            type: 'is-success'
          })
        })
        .catch(err => {
          console.log(err)
        })
    },
    addTaskToToDo (id) {
      db
        .collection('tasks')
        .doc(id)
        .set({
          status: 'to-do',
          updatedAt: new Date()
        },
        {
          merge: true
        })
        .then(() => {

        })
        .catch(err => {
          console.log(err)
        })
    },
    addTaskToProgress (id) {
      db
        .collection('tasks')
        .doc(id)
        .set({
          status: 'in-progress',
          updatedAt: new Date()
        },
        {
          merge: true
        })
        .then(() => {

        })
        .catch(err => {
          console.log(err)
        })
    },
    addTaskToDone (id) {
      db
        .collection('tasks')
        .doc(id)
        .set({
          status: 'done',
          updatedAt: new Date()
        },
        {
          merge: true
        })
        .then(() => {

        })
        .catch(err => {
          console.log(err)
        })
    },
    addTaskToBackLog (id) {
      db
        .collection('tasks')
        .doc(id)
        .set({
          status: 'back-log',
          updatedAt: new Date()
        },
        {
          merge: true
        })
        .then(() => {

        })
        .catch(err => {
          console.log(err)
        })
    },
    deleteConfirm (id) {
      this.$dialog.confirm({
        title: 'Deleting task',
        message: 'Are you sure you want to <b>delete</b> this task? This action cannot be undone.',
        confirmText: 'Delete',
        type: 'is-danger',
        hasIcon: true,
        onConfirm: () => this.deleteTask(id)
      })
    }
  }

}
</script>
