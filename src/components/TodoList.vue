<template>
  <main v-bind:class="{ lightMode: dark }">
    <section class="page_wrapper">
      <header>
        <div class="banner_wrapper">
          <h1>todo</h1>
          <img
            src="../assets/icon-sun.svg"
            alt="light theme"
            class="logo light"
            v-show="!dark"
            @click="dark = !dark"
          />
          <img
            src="../assets/icon-moon.svg"
            alt="dark theme"
            class="logo dark"
            v-show="dark"
            @click="dark = !dark"
          />
        </div>
      </header>
      <section class="global_wrapper">
        <div class="new_todo">
          <form @submit.prevent="addNewTodo()">
            <div class="checkboxes"></div>
            <input type="text" required placeholder="Create a new todo..." v-model="userInput" />
          </form>
        </div>
        <div class="list_wrapper">
          <draggable v-model="filteredList" ghost-class="ghost" @end="onEnd">
            <transition-group type="transition" name="flip-list" tag="div">
              <div class="todo_list sortable" v-for="todo in filteredList" :key="todo.text">
                <ul>
                  <div
                    class="checkboxes"
                    @click="toggleCheckbox(todo.text)"
                    v-show="!todo.isComplete"
                  ></div>
                  <div
                    class="checkboxes check_checkboxes"
                    @click="toggleCheckbox(todo.text)"
                    v-show="todo.isComplete"
                  ></div>

                  <li class="to_complete_li" v-show="!todo.isComplete">{{ todo.text }}</li>
                  <li class="complete_li" v-show="todo.isComplete">{{ todo.text }}</li>
                </ul>
              </div>
            </transition-group>
          </draggable>
          <ul class="mobile_filters">
            <li>{{ countTodos }} items left</li>
            <li @click="clearCompleted()">Clear Completed</li>
          </ul>
          <ul class="filters">
            <li>{{ countTodos }} items left</li>
            <li @click="showAll()">All</li>
            <li @click="showActive()">Active</li>
            <li @click="showCompleted()">Completed</li>
            <li @click="clearCompleted()">Clear Completed</li>
          </ul>
        </div>
      </section>
      <footer>
        <p>Drag and drop to reorder list</p>
      </footer>
    </section>
  </main>
</template>

<script>
import draggable from 'vuedraggable';
export default {
  name: 'TodoList',
  components: {
    draggable,
  },
  data() {
    return {
      dark: false,
      userInput: '',
      /* Drag & drop */
      oldIndex: '',
      newIndex: '',

      /* Local storage list */
      todoList: [],
      /* Displayed list */
      filteredList: [],
    };
  },
  computed: {
    countTodos() {
      return Object.values(this.todoList).filter((n) => !n.isComplete).length;
    },
  },
  methods: {
    setStorage(arr) {
      localStorage.setItem('userList', JSON.stringify(arr));
    },
    addNewTodo() {
      this.todoList.unshift({ text: this.userInput, isComplete: false });
      this.setStorage(this.todoList);
      this.filteredList = this.todoList;
      this.userInput = '';
    },

    toggleCheckbox(str) {
      const index = this.todoList.findIndex((x) => x.text === str);
      this.todoList[index].isComplete = !this.todoList[index].isComplete;
      this.setStorage(this.todoList);
    },

    /* Filters functions */
    clearCompleted() {
      const newList = this.todoList.filter((obj) => {
        return !obj.isComplete;
      });
      this.filteredList = newList;
      this.todoList = newList;
      if (newList.length <= 0) {
        localStorage.removeItem('userList');
      } else {
        this.setStorage(newList);
      }
    },

    showAll() {
      if (this.filteredList.length != this.todoList.length) {
        this.filteredList = this.todoList;
      }
    },
    showActive() {
      this.filteredList = this.todoList.filter((obj) => {
        return !obj.isComplete;
      });
    },
    showCompleted() {
      this.filteredList = this.todoList.filter((obj) => {
        return obj.isComplete;
      });
    },
    /* Drag and drop */
    onEnd(e) {
      this.oldIndex = e.oldIndex;
      this.newIndex = e.newIndex;
      this.todoList = this.filteredList
      // this.setStorage(this.todoList);
    },
  },

  /* Get list from local storage  on page load */
  mounted() {
    if (!localStorage.getItem('userList')) {
      const demoArray = [{ text: 'Add todos in my list !', isComplete: false }];
      localStorage.setItem('userList', JSON.stringify(demoArray));
    }
    this.todoList = JSON.parse(localStorage.getItem('userList'));
    this.filteredList = this.todoList;
  },
};
</script>

<style scoped>
main {
  background-color: var(--bg-dark-2);
  min-height: 100vh;
  width: 100%;
}
.page_wrapper {
  width: 1440px;
  margin: auto;
  display: flex;
  flex-direction: column;
}
header {
  width: inherit;
  height: 300px;
  background-image: var(--url);
}
header .banner_wrapper {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin: auto;
  margin-top: 4.5rem;
  width: var(--card-width);
}
header h1 {
  font-weight: 700;
  color: var(--title);
  text-transform: uppercase;
  font-size: 2.5rem;
  letter-spacing: 17px;
}
header .logo {
  width: 27px;
  height: 27px;
  cursor: pointer;
}
.global_wrapper {
  width: var(--card-width);
  margin: -9rem auto 0 auto;
  font-size: var(--font-size);
}
.new_todo form {
  display: flex;
  align-items: center;
  min-height: var(--min-height);
  background-color: var(--bg-dark-1);
  margin-bottom: 1.5rem;
  border-radius: 5px;
  padding: 0 1.5rem;
  box-shadow: 0px 5px 20px 1px var(--shadow);
}
.checkboxes {
  width: 23px;
  height: 23px;
  min-height: 23px;
  min-width: 23px;
  border: 1.5px solid var(--borders-cl);
  border-radius: 50%;
  cursor: pointer;
  margin-right: 1.5rem;
  position: relative;
}
.check_checkboxes {
  background: linear-gradient(130deg, rgba(87, 221, 255, 1) 18%, rgba(192, 88, 243, 1) 85%);
}
input[type='text'] {
  border: none;
  background-color: var(--bg-dark-1);
  color: var(--text-cl-1);
  font-size: var(--font-size);
  width: 90%;
}
input::placeholder {
  color: var(--text-cl-1);
}
/* The list */
.list_wrapper {
  background-color: var(--bg-dark-1);
  border-radius: 5px;
  box-shadow: 0px 5px 20px 1px var(--shadow);
}
.todo_list ul {
  display: flex;
  align-items: center;
  min-height: var(--min-height);
  border-bottom: 1px solid var(--borders-cl);
  cursor: grab;
  padding: 0 1.5rem;
}
.todo_list ul .complete_li {
  text-decoration: line-through;
}
/* Drag & drop styling */
.page_wrapper .sortable-drag {
  opacity: 0;
}
.flip-list-move {
  transition: transform 0.5s ease;
}
.ghost {
  box-shadow: 0px 2px 20px 1px var(--shadow), -12px 0px 0px 0px var(--bright-blue);
}
@keyframes rotating {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
/* bottom filters options */
.filters,
.mobile_filters {
  display: flex;
  align-items: center;
  height: var(--min-height);
  padding: 0 1rem;
  justify-content: space-evenly;
}
.filters li,
.mobile_filters li {
  cursor: pointer;
  color: var(--text-cl-1);
  font-size: 15px;
  font-weight: bolder;
}
.filters li:first-child {
  color: var(--bright-blue);
}
.filters li:hover,
.mobile_filters li:hover {
  color: var(--bright-blue);
}
.mobile_filters {
  border-bottom: 1px solid var(--borders-cl);
  justify-content: space-between;
  padding: 0 2rem;
}
.mobile_filters {
  display: none;
}
/* Footer */
footer {
  text-align: center;
  margin-top: 5rem;
}
footer p {
  color: var(--text-cl-1);
  font-size: 15px;
  font-weight: bolder;
}

/* Responsive */
@media screen and (max-width: 1440px) {
  .page_wrapper {
    width: 90%;
  }
  header {
    width: 90%;
    margin: 0 auto;
  }
  header .banner_wrapper {
    width: 55%;
  }
  .global_wrapper {
    width: 55%;
  }
}

@media screen and (max-width: 1124px) {
  .page_wrapper {
    width: 100%;
  }
  header {
    width: 100%;
    margin: 0 auto;
  }
  header .banner_wrapper {
    width: 85%;
  }
  .global_wrapper {
    width: 85%;
  }
  .filters li:first-child,
  .filters li:last-child {
    display: none;
  }
  .mobile_filters {
    display: flex;
  }
}
</style>
