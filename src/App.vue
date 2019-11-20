<template>
  <div id="app" class="container">
    <h1 class="title">SnipURL</h1>
    <p class="text">A simple url shortener, enter any complete valid url (ex: https://www.google.com) below to recieve an easy to remember shortened version!</p>
    <input type="text" v-model="originalUrl" class="input" @keyup.enter="submitUrl">
    <button @click="submitUrl" class="button submit">Submit</button>
    <div v-if="shortUrl.length > 0" >
      <p class="text">Shortened URL: <span class="urlText">{{ this.shortUrl }}</span></p>
      <button           
        v-clipboard:copy="shortUrl"
        v-clipboard:success="handleCopySuccess"
        v-clipboard:error="handleCopyFailure" class="button">Copy</button>
    </div>
  </div>
</template>

<style src="@/assets/styles.css" lang="css"></style>

<script>
import axios from "axios";

export default {
  name: 'app',
  data() {
    return {
      originalUrl: "",
      shortUrl: ""
    }
  },
  methods: {
    submitUrl() {
      if (this.originalUrl.length === 0) {
        alert("Please enter a url");
      }

      axios.get(`https://natethedevurlshortener.herokuapp.com/new/${this.originalUrl}`)
        .then(response => {
          if (response.data.urlOriginal === "Invalid Url") {
            alert("please enter a valid url. ex: https://www.google.com");
            return;
          }
          this.shortUrl = response.data.urlShortened;
          this.originalUrl = "";
        })
        .catch(() => {
          alert("There was an error with your request, please try again.")
        })
    },
    handleCopySuccess() {
      alert("Successfully copied url");
    },
    handleCopyFailure() {
      alert("Copy has failed, please try again");
    }
  }
}
</script>
