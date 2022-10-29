<template>
  <div class="container">
    <section class="vh-100">
      <div class="container py-5 h-100">
        <div class="row d-flex justify-content-center align-items-center h-100">
          <div class="col col-lg-9 col-xl-7">
            <div class="card rounded-3">
              <div class="card-body p-4">
                <h4 class="text-center my-3 pb-3">To Do App</h4>
                <form
                  class="mb-4"
                  @submit.prevent="selectedIndex == null ? store() : update()"
                >
                  <div class="input-group mb-1">
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Add a task...."
                      autocomplete="off"
                      v-model="todo"
                    />
                    <button type="submit" class="btn btn-primary ml-2">
                      {{ selectedIndex == null ? "Save" : "Update" }}
                    </button>
                  </div>
                  <div class="text-danger" v-if="todoError != ''">
                    <b>{{ todoError }}</b>
                  </div>
                </form>

                <table class="table mb-4">
                  <thead>
                    <tr>
                      <th scope="col">No.</th>
                      <th scope="col">Todo item</th>
                      <th class="text-center" scope="col">Actions</th>
                    </tr>
                  </thead>
                  <tbody id="todo-render">
                    <tr v-if="todos.length < 1">
                      <td colspan="3" class="text-center">No data found</td>
                    </tr>
                    <tr v-else v-for="(todo, i) in todos" :key="i">
                      <th scope="row">{{ i + 1 }}</th>
                      <td>{{ todo.todo }}</td>
                      <td class="text-center">
                        <a
                          href="javascript:void(0)"
                          class="text-success complete-todo"
                          v-if="todo.status == 'In Progress'"
                          @click.prevent="updateStatus(i, 'Complete')"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            class="feather feather-check-circle text-success"
                          >
                            <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                            <polyline points="22 4 12 14.01 9 11.01"></polyline>
                          </svg>
                        </a>
                        <a
                          href="javascript:void(0)"
                          class="text-success complete-todo"
                          v-else
                          @click.prevent="updateStatus(i, 'In Progress')"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            class="feather feather-slash text-primary"
                          >
                            <circle cx="12" cy="12" r="10"></circle>
                            <line
                              x1="4.93"
                              y1="4.93"
                              x2="19.07"
                              y2="19.07"
                            ></line>
                          </svg>
                        </a>
                        <a
                          href="javascript:void(0)"
                          class="text-success complete-todo"
                          @click.prevent="editTodo(i)"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            class="feather feather-edit-3 text-warning ml-2"
                          >
                            <path d="M12 20h9"></path>
                            <path
                              d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"
                            ></path>
                          </svg>
                        </a>
                        <a
                          href="javascript:void(0)"
                          class="text-success complete-todo"
                          @click.prevent="deleteTodo(i)"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            class="feather feather-trash-2 text-danger ml-2"
                          >
                            <polyline points="3 6 5 6 21 6"></polyline>
                            <path
                              d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                            ></path>
                            <line x1="10" y1="11" x2="10" y2="17"></line>
                            <line x1="14" y1="11" x2="14" y2="17"></line>
                          </svg>
                        </a>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    todo: "",
    todoError: "",
    todos: [],
    selectedIndex: null,
  }),
  methods: {
    store() {
      if (this.validation()) {
        this.todos.push({
          todo: this.todo,
          status: "In Progress",
        });
        this.todo = "";
      }
    },
    update() {
      if (this.validation()) {
        this.todos[this.selectedIndex].todo = this.todo;
        this.selectedIndex = null;
        this.todo = "";
      }
    },
    editTodo(index) {
      this.todo = this.todos[index].todo;
      this.selectedIndex = index;
    },
    updateStatus(index, status) {
      this.todos[index].status = status;
    },
    deleteTodo(index) {
      const c = confirm("Are sure to delete?");
      if (c) {
        this.todos.splice(index, 1);
      }
    },
    validation() {
      if (this.todo.trim().length < 1) {
        this.todoError = "This field is required";
        return false;
      } else if (
        this.todos.filter((t) => t.todo == this.todo).length > 0 &&
        this.selectedIndex == null
      ) {
        this.todoError = "Todos already exists";
        return false;
      } else if (
        this.todos.findIndex((t) => t.todo == this.todo) != -1 &&
        this.todos.findIndex((t) => t.todo == this.todo) !=
          this.selectedIndex &&
        this.selectedIndex != null
      ) {
        this.todoError = "Todos already exists";
        return false;
      } else {
        this.todoError = "";
        return true;
      }
    },
  },
};
</script>
