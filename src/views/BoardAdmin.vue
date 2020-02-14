<template>
  <div id="app">
    {{todos}}
    <ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
      <li class="nav-item">
        <a
          class="nav-link active"
          id="pills-home-tab"
          data-toggle="pill"
          href="#pills-home"
          role="tab"
          aria-controls="pills-home"
          aria-selected="true"
        >User Statistics</a>
      </li>
      <li class="nav-item">
        <a
          class="nav-link"
          id="pills-profile-tab"
          data-toggle="pill"
          href="#pills-profile"
          role="tab"
          aria-controls="pills-profile"
          aria-selected="false"
        >User Choice</a>
      </li>
    </ul>
    <div class="tab-content" id="pills-tabContent">
      <div
        class="tab-pane fade show active"
        id="pills-home"
        role="tabpanel"
        aria-labelledby="pills-home-tab"
      >
        <div class="input-group mb-3">
          <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon3">username</span>
          </div>
          <input
            type="text"
            class="form-control"
            id="basic-url"
            aria-describedby="basic-addon3"
          />
          <div class="input-group-append">
            <button
              class="btn btn-outline-secondary"
              type="button"
              id="button-addon2"
              @change="findUser()"
            >search</button>
          </div>
        </div>

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
      <div
        class="tab-pane fade"
        id="pills-profile"
        role="tabpanel"
        aria-labelledby="pills-profile-tab"
      >
        <table class="table table-hover">
          <thead class="thead-dark">
            <tr>
              <th>#</th>
              <th>Name Activity</th>
              <th>Action</th>
              <th>Yes</th>
              <th>No</th>
            </tr>
          </thead>
          <tbody v-for="user of todos" :key="user.userName">
            <tr v-if="user.activities.length!=0">
              <td align="center" colspan="5">
                <h3>{{user.userName}}</h3>
              </td>
            </tr>
            <tr v-for="activity of user.activities" :key="activity.nameAct">
              <td>1</td>
              <td>{{activity.nameAct}}</td>
              <td>{{activity.action}}</td>
              <td>
                <button type="button" @click="AgreeAction(activity)" class="btn btn-success">Success</button>
              </td>
              <td>
                <button
                  type="button"
                  @click="DisagreeAction(activity)"
                  class="btn btn-danger"
                >Danger</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import UserService from "../services/user.service";
const API_URL = "http://localhost:5050/api/test/";
//import userService from '../services/user.service';
export default {
  name: "user",
  data() {
    return {
      userN: "",
      todos: [],
      info: [],
      todoName: ""
    };
  },

  async created() {
    try {
      const res = await axios.get(API_URL + "getUserPerActivity");
      this.todos = res.data;
    } catch (e) {
      UserService.getPublicContent.then(error => {
        this.todoName = error.response.data.message;
      });
    }
  },
  methods: {
    async AgreeAction(actn) {
      try {
        const res2 = await axios.post(API_URL + "addUserActForAdmin", {
          userName: this.currentUser.username,
          nameActivity: actn.nameAct,
          action: actn.action,
          id: actn.id,
          isAgree: true
        });
        this.todos = res2.data;
      } catch (e) {
        UserService.getPublicContent.then(error => {
          this.todoName = error.response.data.message;
        });
      }
    },
    async DisagreeAction(actn) {
      try {
        const res3 = await axios.post(API_URL + "addUserActForAdmin", {
          userName: this.currentUser.username,
          nameActivity: actn.nameAct,
          action: actn.action,
          id: actn.id,
          isAgree: false
        });
        this.todos = res3.data;
      } catch (e) {
        UserService.getPublicContent.then(error => {
          this.todoName = error.response.data.message;
        });
      }
    },
     async findUser() {
      try {
        const res4 = await axios.post(API_URL + "getUserActForAdmin", {
          userName: document.getElementById("basic-url").value
        });
        this.info = res4.data;
      } catch (e) {
        UserService.getPublicContent.then(error => {
          this.todoName = error.response.data.message;
        });
      }
    }
  }
};
</script>