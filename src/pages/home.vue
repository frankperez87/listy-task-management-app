<template>
   <div id="app">
      <f7-views navbar-through="">
          <f7-view main="" url="/" :dynamic-navbar="true">
              <f7-navbar>
                  <f7-nav-left></f7-nav-left>
                  <f7-nav-center sliding="">Listy - Simple Task Management</f7-nav-center>
                  <f7-nav-right></f7-nav-right>
              </f7-navbar>
              <f7-pages id="pages">
                  <f7-page class="navbar-fixed">
                    <div class="list-block">
                      <ul>
                        <li>
                          <div class="item-content">
                            <div class="item-inner">
                              <div class="item-title label">Write</div>
                              <div class="item-input">
                                  <input type="text" v-model="newTask" @keypress.enter="add">
                              </div>
                            </div>
                          </div>
                        </li>
                      </ul>
                    </div>   
                      
                      <template v-if="notCompletedTasks.length > 0">
                        <div align="center"><strong>Pending Tasks</strong></div>

                        <f7-list>
                            <f7-list-button :title="task.description" v-for="task in notCompletedTasks" @click="markCompleted(task)"></f7-list-button>
                        </f7-list>
                      </template>

                      <template v-if="completedTasks.length > 0">
                        <div align="center"><strong>Completed Tasks</strong></div>

                        <f7-list>
                            <f7-list-item :title="task.description" v-for="task in completedTasks"></f7-list-item>
                        </f7-list>
                      </template>
                  </f7-page>
              </f7-pages>
          </f7-view>
      </f7-views>
  </div>
</template>

<script>
let taskStorage = localStorage.getItem('tasks');

if(taskStorage == null) {
  localStorage.setItem('tasks', JSON.stringify([]));
  taskStorage = localStorage.getItem('tasks');
}

export default {
  name: 'app',
  data () {
    return {
      newTask: '',
      tasks: []
    }
  },

  created() {
    this.tasks = JSON.parse(taskStorage);
  },

  computed: {
    completedTasks() {
      return this.tasks.filter(task => task.completed);
    },
    notCompletedTasks() {
      return this.tasks.filter(task => !task.completed);
    }
  },

  methods: {
    add() {
      this.tasks.push({ description: this.newTask , completed: false });
      this.newTask = '';
      this.save();
    },

    markCompleted(task) {
      task.completed = true
      this.save();
    },

    save() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
}
</script>

<style>

</style>
