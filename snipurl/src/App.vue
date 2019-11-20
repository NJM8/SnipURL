<template>
  <div id="app">
    <h1>SnipURL</h1>
    <p>A simple url shortener, enter any complete valid url below to recieve an easy to remember shortened version!</p>
    <input type="text" v-model="originalUrl">
    <button @click="submitUrl">Submit</button>
    <p>Shortened URL: {{ this.shortUrl }}</p>
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

      axios.get(`http://localhost:8000/new/${this.originalUrl}`).then(response => {
        if (response.data.urlOriginal === "Invalid Url") {
          alert("please enter a valid url. ex: https://www.google.com");
        }
        this.shortUrl = response.data.urlShortened;
      })
    }
  }
}
</script>
