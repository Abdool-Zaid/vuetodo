<template>
  <v-list-item-group v-model="settings" multiple>
    <div v-for="(task, index) in tasks" :key="index">
      <v-list-item
        @click="executedTask(task.id)"
        @contextmenu.prevent="reorderTasks(task.id)"
        :class="{ 'blue lighten-4': task.status }"
        :id="task.id"
      >
        <template v-slot:default="{}">
          <v-list-item-action>
            <v-checkbox :input-value="task.status" color="primary"></v-checkbox>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title
              :class="{ 'text-decoration-line-through': task.status }"
            >
              {{ task.title }}</v-list-item-title
            >
            <v-list-item-subtitle
              :class="{ 'text-decoration-line-through': task.status }"
            >
              {{ task.subtitle }}</v-list-item-subtitle
            >
          </v-list-item-content>
          <v-list-item-action>
            <div class="text-center">
              <v-dialog v-model="dialog" width="500">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="red lighten-2"
                    dark
                    v-bind="attrs"
                    v-on="on"
                    icon
                  >
                    <v-icon color="primary lighten-4"
                      >mdi-pencil-box-multiple-outline
                    </v-icon>
                  </v-btn>
                </template>

                <v-card>
                  <v-card-title class="text-h5 grey lighten-2">
                    edit task
                  </v-card-title>

                  <v-card-text>
                    <v-row justify="center" class="pt-6">
                      <v-col cols="12" sm="10" md="8" lg="6">
                        <v-card ref="form" hide-details>
                          <v-card-text>
                            <v-text-field
                              ref="alteredTitle"
                              v-model="alteredTitle"
                              label="task"
                              required
                              clearable
                            ></v-text-field>
                            <v-text-field
                              ref="alteredsubtitle"
                              v-model="alteredsubtitle"
                              clearable
                              label="subtitle"
                            ></v-text-field>
                          </v-card-text>
                          <v-divider class="mt-12"></v-divider>
                          <v-card-actions>
                            <v-btn
                              color="primary"
                              text
                              @click="alterStep(task.id)"
                            >
                              Submit
                            </v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-col>
                    </v-row>
                  </v-card-text>

                  <v-divider></v-divider>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      id="closeButton"
                      color="primary"
                      text
                      @click="dialog = false"
                    >
                      close
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </div>
            <v-btn icon @click.stop="nextStep(task.id)">
              <v-icon color="primary lighten-4">mdi-delete</v-icon>
            </v-btn>
          </v-list-item-action>
        </template>
      </v-list-item>

      <v-divider></v-divider>
    </div>

    <v-divider></v-divider>

    <v-row justify="center" class="pt-6">
      <v-col cols="12" sm="10" md="8" lg="6">
        <v-card ref="form" hide-details>
          <v-card-text>
            <v-text-field
              ref="title"
              v-model="title"
              label="task"
              required
              clearable
            ></v-text-field>
            <v-text-field
              ref="subtitle"
              v-model="subtitle"
              clearable
              label="subtitle"
            ></v-text-field>
          </v-card-text>
          <v-divider class="mt-12"></v-divider>
          <v-card-actions>
            <v-btn color="primary" text @click="progressForward()">
              Submit
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-list-item-group>
</template>

<script>
export default {
  name: "Home",
  data: () => ({
    dialog: false,
    title: "",
    alteredTitle: "",
    subtitle: "",
    settings: [],
    tasks: JSON.parse(localStorage.tasks),
    orderStack: [],
  }),
  mounted() {
    this.loadFunc();
  },
  methods: {
    executedTask(id) {
      let task = this.tasks.filter((task) => task.id == id)[0];
      task.status = !task.status;
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    nextStep(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    progressForward() {
      let newTask = {
        id: `t` + JSON.stringify(Date.now()),
        title: this.title,
        subtitle: this.subtitle,
        status: false,
      };
      this.tasks.push(newTask);
      this.title = "";
      this.subtitle = "";
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    alterStep(id) {
      let task = this.tasks.filter((task) => task.id == id)[0];
      (task.title = this.alteredTitle), (task.subtitle = this.alteredsubtitle);
      this.alteredTitle = "";
      this.alteredsubtitle = "";
      localStorage.setItem("tasks", JSON.stringify(this.tasks));

      this.dismisModal();
    },
    loadFunc() {
      let tasks = JSON.parse(localStorage.getItem("tasks"))
        ? JSON.parse(localStorage.getItem("tasks"))
        : localStorage.setItem(
            "tasks",
            JSON.stringify([
              {
                id: "t0",
                title: "stay hungry",
                subtitle: "stay foolish",
                status: false,
              },
            ])
          );
    },
    dismisModal() {
      let clickEvent = new Event("click");
      document.querySelector("#closeButton").dispatchEvent(clickEvent);
    },
    reorderTasks(id) {
      if (this.orderStack.length < 2) {
        this.orderStack.push(id);
      } else if ((this.orderStack.length = 2)) {
        let task1 = this.tasks.findIndex(
          (item) => item.id == this.orderStack[0]
        );
        let task2 = this.tasks.findIndex(
          (item) => item.id == this.orderStack[1]
        );
        [this.tasks[task1], this.tasks[task2]] = [
          this.tasks[task2],
          this.tasks[task1],
        ];
        this.orderStack = [];
        this.tasks.push('mlem')
        this.tasks.pop()
        localStorage.setItem("tasks", JSON.stringify(this.tasks));

      }
    },
  },
};
</script>
