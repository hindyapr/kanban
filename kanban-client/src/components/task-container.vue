<template>
    <section>
        <article :class="{message: true, 'is-danger': title === 'Back-Log', 'is-success': title === 'Done', 'is-info': title === 'To-Do', 'is-warning': title === 'In Progress'}">
            <div class="message-header">
                <p>{{ title }}</p>
            </div>
            <div class="message-body">
               <task v-for="task in data" :key="task.id" :task="task"/>
            </div>
        </article>
    </section>
</template>


<script>
import task from '@/components/task.vue'

export default {
    props : ['title', 'data'],
    components : {task}
}
</script>
