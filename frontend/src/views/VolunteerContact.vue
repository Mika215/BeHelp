<script>
export default {
  data() {
    return {
      result: undefined,
    };
  },
  mounted() {
    this.getUser();
  },
  methods: {
    async getUser() {
      try {
        const res = await fetch(
          `${import.meta.env.VITE_API}/users/${this.$route.params._id}`,
          {
            method: "GET",
            headers: {
              "Content-Type": "application/json",
            },
          }
        );
        this.result = await res.json();
        console.log(this.result.firstName);
      } catch (error) {
        console.log(error);
      }
    },
    getUserEmail() {
       fetch(
          `${import.meta.env.VITE_API}/users/contactinfo/${this.$route.params._id}`,
          {
            method: "GET",
            headers: {
              "Content-Type": "application/json",
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          }
        ).then(response => response.json())
        .then(data => {
          console.log(data);
        });
    },
  },
};
</script>

<template>
<div v-if="result" class="container">
    <div class="container__volunteer">
    <h2 class="container__volunteer-h2">{{ result.firstName }}</h2>
      <div class="container__volunteer-top">
        <img v-bind:src="result.photoURL" class="container__volunteer-photo" />
        <div class="container__volunteer-profile">
          <p><b>Nationality: </b>{{ result.nationality }}</p>
          <p><b>City:</b> {{ result.location[0]}}</p>
          <p><b>Languages: </b>
          <ul>
          <li v-for="lang in result.languages"> {{ lang }} </li>
          </ul></p>
        </div>
      </div>
      <div class="container__volunteer-text">
        <p><b>Skills: </b>
        <ul>
        <li v-for="skill in result.skills"> {{ skill }} </li>
        </ul></p>
        <p><b>Description: </b>{{ result.description }}</p>
      </div>
    </div>

    <div class="container__contact">
      <div class="container__contact-messagebox">
        <form class="container__contact-form" @submit.prevent="sendEmail">
          <h2 class="container__contact-h2">CONTACT THIS VOLUNTEER</h2>
            <div class="container__contact-logo">
              <img src="../assets/logos/Logo_small_blue.svg" alt="logo" />
            </div>
        
          <textarea
            class="container__contact-message"
            name="message"
            cols="30"
            rows="8"
            v-model="message"
            placeholder="Message"
          >
          </textarea>
        
          <input type="submit" value="Send" />
        </form>
      </div>
    </div>
  </div>
  </template>

<style scoped lang="scss">
@import "../components/styles/abstract/_base.scss";
@import "../components/styles/abstract/_variables.scss";
@import "../components/styles/layout/_volunteerContact.scss";
</style>
