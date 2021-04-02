<template>
    <header>
        <h1>{{ title }}</h1>
        <Button
          v-show="homePage"
          @btn-click="$emit('toggle-add-task')"
          :text="showAddTask ? 'Close' : 'Add Task' "
          :color="showAddTask ? 'grey' : 'green' "
        />
    </header>
</template>

<script>
import Button from './Button.vue';

export default {
  name: 'Header',
  components: {
    Button,
  },
  computed: {
    homePage() {
      return (this.$route.path === '/');
    },
  },
  props: {
    title: String,
    showAddTask: Boolean,
  },
  emits: [
    'toggle-add-task',
  ],
};
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 1.5em;
    }
</style>
