<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6"> Vuetify TODO </v-list-item-title>
          <v-list-item-subtitle> Your Todo List </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list density="compact" nav>
        <v-list-item
          to="/"
          prepend-icon="mdi-view-dashboard"
          title="Todo"
          value="myfiles"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      color="primary"
      dark
      prominent
      src="https://picsum.photos/200/300?grayscale"
      scroll-target="#scrolling-techniques-4"
    >
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Todo</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-text-field
        v-model="newTaskTitle"
        @click:append-inner="addTask"
        @keyup.enter="addTask"
        class="pa-3"
        outlined
        label="Add Task"
        append-inner-icon="mdi-plus"
        hide-details
        clearable=""
      ></v-text-field>
      <div v-for="task in tasks" :key="task.id">
        <v-list class="pt-0" lines="three" select-strategy="multiple">
          <v-list-item
            @click="doneTask(task.id)"
            :class="{ 'bg-blue lighten-3': task.done }"
            value="notifications"
          >
            <template v-slot:prepend>
              <v-list-item-action>
                <v-checkbox-btn
                  :model-value="task.done"
                  color="primary"
                ></v-checkbox-btn>
              </v-list-item-action>
              <v-list-item-content>
                <div>
                  <v-btn @click.stop="deleteTask(task.id)" icon class="edit">
                    <v-icon color="blue">mdi-delete</v-icon>
                  </v-btn>
                </div>
                <v-list-item-title
                  class="edt"
                  :class="{ 'text-decoration-line-through': task.done }"
                  >{{ task.title }}
                </v-list-item-title>
              </v-list-item-content>
            </template>
          </v-list-item>
        </v-list>

        <v-divider></v-divider>
      </div>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawer: null,
    newTaskTitle: "",
    tasks: [
      // {
      //   id: 1,
      //   title: "wake Up",
      //   done: true,
      // },
      // {
      //   id: 2,
      //   title: "Get Fruits",
      //   done: false,
      // },
      // {
      //   id: 3,
      //   title: "Eat healthy",
      //   done: false,
      // },
      // {
      //   id: 4,
      //   title: "Do Workout",
      //   done: false,
      // },
      // {
      //   id: 5,
      //   title: "GO Gym",
      //   done: false,
      // },
    ],
  }),
  mounted() {
    if (localStorage.tasks) {
      this.tasks = JSON.parse(localStorage.tasks);
    }
  },
  watch: {
    tasks: {
     handler (newTasks) {
      localStorage.tasks = JSON.stringify(newTasks);
    },
    deep: true
    }
  },
  methods: {
    doneTask(id) {
      let task = this.tasks.filter((task) => task.id == id)[0];
      task.done = !task.done;
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    addTask() {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false,
      };
      this.tasks.push(newTask);
      this.newTaskTitle = "";
    },
  },
};
</script>

<style>
.edt {
  margin-top: -40px;
}
.edit {
  margin-left: 940px;
}

@media only screen and (max-width: 600px) {
  .edit {
    margin-left: 270px;
  }
}
</style>
