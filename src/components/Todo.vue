<template>
  <div id="todo" class="flex flex-col items-center p-4 font-mono text-lg h-screen">
    <h1 class="uppercase bold text-blue-900 text-2xl p-2">TODO LIST
      <div class="float-right bg-blue-900 text-blue-500 lowercase px-2 ml-2 mt-1 rounded-full">
        <svg class="fill-current" xmlns="http://www.w3.org/2000/svg" height="25" viewBox="5 7 12 12" width="10"><path d="M0 0h24v24H0z" fill="none"/><path d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z"/></svg>
      </div>
    </h1>
    <div class="flex flex-col items-start">
      <div> <!-- div used to keep consistent width -->
          <div class="border-8 border-blue-900 mt-4 p-2 rounded-lg shadow-lg">
              <input @keyup.enter="addTask" class="p-2 capitalize rounded-sm" placeholder="Add Todo" maxlength="30" v-model="task.name">
              <button @click="addTask" class="capitalize bg-red-500 text-red-900 ml-2 px-2 py-3 rounded-lg">Add Todo</button>
          </div>
          <ul class="mt-2 rounded-lg">
              <li class="border-8 border-blue-800 bg-white p-2 mt-4 capitalize rounded-lg" v-for="(value, key) in tasks" :key="key">{{ value.name }}
                <!-- <button @click="removeTask(key)" class="float-right bg-red-500 text-red-900 lowercase px-2 ml-2 mb-2 rounded-sm">x</button> -->
                <button @click="removeTask(key)" class="float-right bg-red-500 text-red-900 lowercase px-2 ml-2 mb-2 rounded-full">
                  <svg class="fill-current" xmlns="http://www.w3.org/2000/svg" height="27" viewBox="5 7 12 12" width="11"><path d="M0 0h24v24H0z" fill="none"/><path d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z"/></svg>
                </button>
              </li>
          </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data() {
    return {
      tasks: [], // array of task objects
      task: { name: '' },  // using a task object allows for additional properties to be added in the future
      whiteSpace : new RegExp("^\\s+$") // for whitespace detection in input field
    }
  },
  mounted() {
    if (localStorage.getItem("tasks")) {
      try {
        this.tasks = JSON.parse(localStorage.getItem("tasks"));
      }
      catch(e) {
        localStorage.removeItem("tasks");
      }
    }
  },
  methods: {
    addTask() {
        if (this.task.name != '' && !this.whiteSpace.test(this.task.name)) {
          this.tasks.push({...this.task}); // creates object with this property (shallow copy task) and stores it in array
        }
        this.task.name = ''; // reset text field

        this.saveTasks(); // updated local storage
    },

    removeTask(key) {
      this.tasks.splice(key, 1);
      this.$forceUpdate(); // shows deletion immediately

      this.saveTasks(); // updated local storage
    },

    saveTasks : function() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsed);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
