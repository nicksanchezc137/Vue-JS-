<template>
  <div id="app">
    <Header v-on:errors-present="onErrors" />
    <div v-if="errors.length" class="alert alert-danger alert-dismissible" role="alert">
      <strong>File Errors:</strong> <p  v-for="error in errors" v-bind:key="error.index">
       Error on row number {{Number(error.index+2)}} on column {{error.column}}: {{error.description}}
      </p>
      <button
        type="button"
        class="close"
        data-dismiss="alert"
        aria-label="Close"
      >
        <span aria-hidden="true">&times;</span>
      </button>
    </div>
    <router-view />
  </div>
</template>

<script>
import Header from "./components/Header";
export default {
  components: {
    Header
  },
  methods: {
    onErrors(errors) {
      this.errors = errors;
    }
  },
  data() {
    return {
      errors: []
    };
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
