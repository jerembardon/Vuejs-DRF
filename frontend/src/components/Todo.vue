<template>
  <div class="hello">
    <h1 class="title"> Test for Toto </h1> 

    <hr>

    <div class="columns">
      <div class="column is-one-third is-offset-one-third"> 
        <form>
          <h2 class="subtitle">Add task</h2>

          <div class="field">
            <label class="label">Description</label>
            <div class="control">
              <input class="input" type="text" v-model="description">
            </div>
          </div>

          <div class="field">
            <label class="label">Status</label>
            <div class="control">
              <div class="select">
                <select v-model.number="status">
                  <option value=0>To do</option>
                  <option value=1>Done</option>
                </select>
              </div>
            </div>
          </div>

          <div class="field is-grouped">
            <div class="control">
              <button class="button is-link" @click.prevent="createTask($data)">Submit</button>
            </div>
          </div>
        </form>
      </div>
    </div>

    <hr>

    <div class="columns" >
      <div class="column is-half" > 
        <h2 class="subtitle">Todo</h2>

        <div class="todo" v-for="task in tasks" :key="task.id">
          <div class="card"  v-if="task.status === 0">
            <div class="card-content">
              <div class="content">
                {{task.description}}
              </div>
            </div>

            <footer class="card-footer">
              <a class="card-footer-item" @click.prevent="taskStatus(task.id, task.description, task.status)">Done</a> 
              <a class="card-footer-item" @click.prevent="deleteTask(task.id)">Delete</a> 
            </footer>
          </div>
        </div>
      </div>

      <div class="column is-half"> 
        <h2 class="subtitle">Done</h2>

        <div class="done" v-for="task in tasks" :key="task.id">
          <div class="card" v-if="task.status === 1">
            <div class="card-content">
              <div class="content">
                {{task.description}}
              </div>
            </div>

            <footer class="card-footer">
              <a class="card-footer-item" @click.prevent="taskStatus(task.id, task.description, task.status)">Undone</a> 
              <a class="card-footer-item" @click.prevent="deleteTask(task.id)">Delete</a> 
            </footer>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Todo',
  data () {
    return {
      tasks: [],
      status: 0,
      description: ''
    }
  },

  mounted () {
    this.getTasks(); 
  },

  methods: {
    getTasks() {
        axios({
            method:'get',
            url: 'http://127.0.0.1:8000/api/1.0/tasks/',
            auth: {
                username: 'test',
                password: 'test$2012'
            }
        }).then(response => this.tasks = response.data);
    },

    createTask({status, description}) {
      axios({
        method: 'post',
        url: 'http://127.0.0.1:8000/api/1.0/tasks/',
        data: {
          description,
          status
        },
        auth: {
          username: 'test',
          password: 'test$2012'
        }
      }).then((response) => {
        let newTask = {id: response.data.id, description, status};
        this.tasks.push(newTask);

        this.description = '' 
        this.status = 0;
      }).catch((error) => {
        console.log(error);
      });
    },

    taskStatus(taskId, description, status) {
      let statusTaskChanged = status === 1 ? 0 : 1;

      axios({
        method: 'patch',
        url: `http://127.0.0.1:8000/api/1.0/tasks/${taskId}/`,
        data: {
          description,
          status: statusTaskChanged
        },
        auth: {
          username: 'test',
          password: 'test$2012'
        }
      }).then((response) => {
        //this.getTasks(); 
      }).catch((error) => {
        console.log(error);
      });
    },

    deleteTask(taskId) {
      axios({
        method: 'delete',
        url: `http://127.0.0.1:8000/api/1.0/tasks/${taskId}/`,
        auth: {
          username: 'test',
          password: 'test$2012'
        }
      }).then((response) => {
        let newTask = this.tasks.filter(task => taskId !== task.id);
        this.tasks = newTask;
      }).catch((error) => {
        console.log(error);
      });
    },
  }
}
</script>

<style scoped>
.select, select { /* 100% width for the select */
  width: 100%;
}

.card { /* Adding some air under the tasks */
  margin-bottom: 25px;
}

.done { /* Make the done tasks a little bit transparent */
  opacity: 0.3;
}

</style>