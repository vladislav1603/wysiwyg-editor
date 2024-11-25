<template>
  <div id="app">
    <TextEditor />
  </div>
</template>

<script>
import TextEditor from './components/TextEditor.vue';

export default {
  name: 'App',
  components: {
    TextEditor
  }
};
</script>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #121212;
  color: #ffffff;
}

#app {
  padding: 20px;
}
</style>
