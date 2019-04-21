<template>
<div>
    <nav class="navbar" role="navigation" aria-label="main navigation">
        <div class="navbar-brand">
            <a class="navbar-item" href="#">
            HACKBAN
            </a>

      </div>

      <div id="navbarBasicExample" class="navbar-menu">
        <div class="navbar-end">
          <div class="navbar-item">
              <div class="buttons">
                <button class="button is-small is-primary has-background-grey-dark"
                @click="isComponentModalActive = true">
                NEW TASK
                </button>
              </div>
          </div>
        </div>
      </div>
    </nav>
    <b-modal :active.sync="isComponentModalActive" class="is-medium" has-modal-card>
        <taskForm ></taskForm>
    </b-modal>
</div>
</template>

<script>
import taskForm from '@/components/taskForm.vue'

export default {
  components: {
    taskForm
  },
  data () {
    return {
      isComponentModalActive: false,
      token: ''
    }
  },
  methods: {
    successLogedIn (token) {
      this.token = token
    }
  }
}
</script>

<style>

</style>
