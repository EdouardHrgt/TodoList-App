<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    TodoList,
  },
};
</script>

<style>
:root {
  --bright-blue: hsl(220, 98%, 61%);
  /* Dark theme : */
  --bg-dark-1: hsl(235, 24%, 19%);
  --bg-dark-2: hsl(235, 21%, 11%);
  --text-cl-1: hsl(234, 11%, 52%);
  --text-cl-2: hsl(234, 39%, 85%);
  --borders-cl: hsl(233, 14%, 35%);
  --shadow: hsl(235, 21%, 11%);
  --title: hsl(240, 39%, 95%);
  /* Font */
  --font-size: 18px;
  /* Width */
  --card-width: 550px;
  --margin-left: 24rem;
  --min-height: 4rem;
  /* Header image */
  --url: url(@/assets/bg-desktop-dark.jpg);
}
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@400;700&display=swap');
.lightMode {
  --bg-dark-1: hsl(0, 0%, 98%);
  --bg-dark-2: hsl(236, 33%, 92%);
  --text-cl-1: hsl(234, 11%, 52%);
  --text-cl-2: hsl(233, 14%, 35%);
  --borders-cl: hsl(235, 18%, 75%);
  --shadow: hsl(233, 15%, 66%);
  --url: url(@/assets/bg-desktop-light.jpg);
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  list-style: none;
  font-family: 'Josefin Sans', sans-serif;
  color: var(--text-cl-2);
}
</style>
