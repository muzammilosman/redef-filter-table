<template>
  <div class="md-layout">
    <div class="md-layout-item md-size-100" v-if="conditions.length > 0">
      <md-button class="md-raised md-accent" @click="submitConditions">SUBMIT</md-button>
    </div>
    <div class="md-layout-item">
      <md-table>
        <md-table-row>
          <md-table-head>Name</md-table-head>
          <md-table-head>Screen Name</md-table-head>
          <md-table-head>Followers Count</md-table-head>
          <md-table-head>Following Count</md-table-head>
          <md-table-head>Location</md-table-head>
          <md-table-head>Verified</md-table-head>
        </md-table-row>

        <md-table-row v-for="user in matchedUsers" :key="user.name">
          <md-table-cell>{{ user.name }}</md-table-cell>
          <md-table-cell>{{ user.screen_name }}</md-table-cell>
          <md-table-cell>{{ user.followers_count }}</md-table-cell>
          <md-table-cell>{{ user.following_count }}</md-table-cell>
          <md-table-cell>{{ user.location }}</md-table-cell>
          <md-table-cell>{{ user.verified }}</md-table-cell>
        </md-table-row>
      </md-table>
    </div>
  </div>
</template>

<script>
import userJson from "../assets/users.json";
export default {
  name: "UserTable",
  props: ["conditions"],
  data() {
    return {
      users: userJson,
      previousMatches: [],
      currentMatches: [],
      matchedUsers: []
    };
  },

  methods: {
    submitConditions() {
      this.matchedUsers = [];
      this.conditions.forEach((criteria, i) => {
        switch (criteria.operator) {
          case "contains":
            this.users.forEach((user, userIndex) => {
              if (user[criteria.id].includes(criteria.value)) {
                this.currentMatches.push(userIndex);
              }
            });
            break;
          case "gte":
            this.users.forEach((user, userIndex) => {
              if (user[criteria.id] >= criteria.value) {
                console.log(`entry: ${user[criteria.id]}`)
                console.log(`value: ${criteria.value}`)
                this.currentMatches.push(userIndex);
              }
            });
            break;
          case "lte":
            this.users.forEach((user, userIndex) => {
              if (user[criteria.id] <= criteria.value) {
                this.currentMatches.push(userIndex);
              }
            });
            break;
          case "eq":
            this.users.forEach((user, userIndex) => {
              if (user[criteria.id] == criteria.value) {
                this.currentMatches.push(userIndex);
              }
            });
            break;
        }
        if (i != 0) {
          switch (criteria.linkLogic) {
            case "&&":
              this.currentMatches = this.previousMatches.filter(item =>
                this.currentMatches.includes(item)
              );
              break;
            case "||":
              this.currentMatches = [
                ...new Set([...this.currentMatches, ...this.previousMatches])
              ];
              break;
          }
        }
        this.previousMatches = this.currentMatches;
        this.currentMatches = [];
      });

      console.log("Entry indexes:", this.previousMatches);
      this.previousMatches.forEach(el => {
        this.matchedUsers.push(this.users[el]);
      });
    }
  },

  created() {
    this.matchedUsers = this.users;
  }
};
</script>

<style></style>
