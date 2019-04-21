<template>
    <form action="">
        <div class="modal-card" style="width: auto">
            <header class="modal-card-head">
                <p class="modal-card-title">Create New Task</p>
            </header>
            <section class="modal-card-body">
                <b-field label="Title">
                    <b-input
                        type="text"
                        v-model="title"
                        placeholder="Title here..."
                        required>
                    </b-input>
                </b-field>

                <b-field label="Description">
                    <b-input
                        v-model="description"
                        maxlength="200"
                        type="textarea"
                        placeholder="Description here...">

                    </b-input>
                </b-field>

                <b-field label="Assigned to">
                    <b-input
                        type="text"
                        v-model="pic"
                        password-reveal
                        placeholder="Person in charged..."
                        required>
                    </b-input>
                </b-field>
            </section>
            <footer class="modal-card-foot" >
                <button class="button" type="button" @click="$parent.close()">Close</button>
                <button class="button is-primary" @click.prevent="addTask">Add New Task</button>
            </footer>
        </div>

    </form>
</template>

<script>
import db from '@/api/firestore.js'

export default {
  props: [],
  data () {
    return {
      title: '',
      description: '',
      pic: ''
    }
  },
  methods: {
    addTask () {
      db
        .collection('tasks')
        .add({
          title: this.title,
          description: this.description,
          pic: this.pic,
          status: 'back-log',
          createdAt: new Date(),
          updatedAt: new Date()
        })
        .then(data => {
          this.$parent.close()
          this.title = ''
          this.description = ''
          this.pic = ''
          this.$snackbar.open({
            duration: 6000,
            message: 'Successfully added taks',
            type: 'is-success',
            position: 'is-top',
            actionText: 'Ok',
            queue: false
          })
        })
        .catch(err => {
          console.log(err)
        })
    }
  }

}
</script>

<style>

</style>
