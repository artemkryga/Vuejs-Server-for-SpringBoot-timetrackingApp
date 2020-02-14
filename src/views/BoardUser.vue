<template>
  <div id="app">
    <div class="row">
      <div class="col-3">
        <div
          class="nav flex-column nav-pills"
          id="v-pills-tab"
          role="tablist"
          aria-orientation="vertical"
        >
          <a
            v-for="todo of todos"
            :key="todo.nameAct"
            class="nav-link"
            data-toggle="pill"
            :href="'#' + todo.nameCategory"
            role="tab"
            aria-controls="v-pills-home"
            aria-selected="true"
          >{{todo.nameCategory}}</a>
        </div>
      </div>
      <div class="col-9">
        <div class="tab-content" id="v-pills-tabContent">
          <div
            v-for="todo of todos"
            :key="todo.nameCategory"
            class="tab-pane fade show"
            :id="todo.nameCategory"
            role="tabpanel"
            aria-labelledby="v-pills-home-tab"
          >
            <div class="card-group">
              <div v-for="activity of todo.activities" :key="activity.nameAct">
                <div
                  v-if="!activity.userHas"
                  class="card text-white bg-primary mb-3"
                  style="max-width: 9rem;"
                >
                  <div class="card-header">Can adding</div>
                  <div class="card-body">
                    <h5 class="card-title">{{activity.nameAct}}</h5>
                    <button type="button" @click="addActivity(activity)" class="btn btn-success">add</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <h1>My Activities</h1>
    <ul>
      <div v-for="todo of todos" :key="todo.nameCategory">
        <div class="alert alert-secondary" role="alert">{{todo.nameCategory}}</div>
        <div class="card-group">
          <div v-for="activity of todo.activities" :key="activity.nameAct">
            <div
              v-if="activity.userHas"
              class="card text-white bg-success mb-3"
              style="max-width: 9rem;"
            >
              <div class="card-header">adding</div>
              <div class="card-body">
                <h5 class="card-title">{{activity.nameAct}}</h5>
                <button type="button" class="btn btn-danger">Success</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </ul>

    <h1>Statistics</h1>

    <input class="btn btn-primary" type="button" value="Click me" @click="showStat()" />

    <table class="table">
      <tr>
        <th>Activities</th>
        <th>Date</th>
        <th>Time</th>
      </tr>
      <tr v-for="currency in info " v-bind:key="currency.nameAct">
        <td>
          <h5>{{currency.name}}</h5>
        </td>
        <td>
          <h5>{{currency.time}}</h5>
        </td>
        <td>
          <h5>{{currency.date}}</h5>
        </td>
      </tr>
    </table>
  </div>
</template>


<script>
import axios from "axios";
import UserService from "../services/user.service";
const API_URL = 'http://localhost:5050/api/test/';
export default {
  name: "user",
  data() {
    return {
      todos: [],
      info: [],
      todoName: ""
    };
  },

  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    }
  },

  async created() {
    try {
      const res = await axios.post(API_URL + "getAllActivityByUser", {
        userName: this.currentUser.username
      });

      this.todos = res.data;
    } catch (e) {
      UserService.getPublicContent.then(error => {
        this.todoName = error.response.data.message;
      });
    }
  },
  methods: {
    async showStat() {
      try {
        const res2 = await axios.post(API_URL + "getStatUser", {
          userName: this.currentUser.username
        });
        this.info = res2.data;
      } catch (e) {
        UserService.getPublicContent.then(error => {
          this.todoName = error.response.data.message;
        });
      }
    },
    async addActivity(currency) {
      try {
        axios.post(API_URL + "addActForUser", {
          action: "ADD",
          nameAct: currency.nameAct,
          userName: this.currentUser.username
        });
      } catch (e) {
        UserService.getPublicContent.then(error => {
          this.todoName = error.response.data.message;
        });
      }
    },
    async deleteActivity(currency) {
      try {
        axios.post(API_URL + "addActForUser", {
          action: "DELETE",
          nameAct: currency.nameAct,
          userName: this.currentUser.username
        });
      } catch (e) {
        UserService.getPublicContent.then(error => {
          this.todoName = error.response.data.message;
        });
      }
    }
  }
};
</script>