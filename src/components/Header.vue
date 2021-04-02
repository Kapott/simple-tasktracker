<template>
    <header>
        <h1>{{ title }}</h1>
        <Button text="Add task" color="#e00" />
    </header>
</template>

<script>
import Button from './Button.vue';

export default {
  name: 'Header',
  components: {
    Button,
  },
  props: {
    title: String,
  },
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
