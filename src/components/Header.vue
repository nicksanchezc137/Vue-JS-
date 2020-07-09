<template>
  <div id="nav">
    <router-link to="/">
      <span class="nav-link">Home</span>
    </router-link>|
    <router-link to="/about">About</router-link>

    <input class="upload-btn" type="file" @change="onFileUpload" accept=".xlsx, .xls, .csv" />
    <a href="javascript:void(0)" class="upload-btn" @click="getTemplate()">Download Excel Template</a>
    <a href="javascript:void(0)" class="upload-btn" @click="getFile('xls')">Export Excel File</a>
    <a href="javascript:void(0)" class="upload-btn" @click="getFile('csv')">Export CSV File</a>
    <a href="javascript:void(0)" class="upload-btn" v-on:click="clear">Clear</a>
  </div>
</template>

<script>
import axios from "axios";
import exportFromJSON from "export-from-json";
export default {
  data() {
    return {
      FILE: null,
      errors: []
    };
  },

  methods: {
    onFileUpload(event) {
      this.FILE = event.target.files[0];
      console.log(this.FILE);
      this.sumbitFile();
    },
    sumbitFile() {
      // upload file
      const formData = new FormData();
      formData.append("datafile", this.FILE, this.FILE.name);
      formData.append("entity", "stocktake");
      axios
        .post("http://193.46.198.162:5700/files/sendFile", formData, {})
        .then(res => {
          console.log(res);
          this.errors = res.data;
          this.$root.$emit("refreshData");
          if (this.errors.length) {
            this.$emit("errors-present", this.errors);
          }
        });
    },
    getFile(bookType) {
      axios
        .get("http://193.46.198.162:5700/files/getFile", {
          bookType,
          file_name: "test"
        })
        .then(res => {
          const fileName = "stocktake";
          const exportType = bookType;
          console.log('res',res);
          exportFromJSON({ data:res.data, fileName, exportType });
        });
    },
     getTemplate() {
      axios
        .get("http://193.46.198.162:5700/files/getTemplate")
        .then(res => {
          const fileName = "stocktake_template";
          const exportType = "csv";
          console.log('res',res);
          exportFromJSON({ data:res.data, fileName, exportType });
        });
    },
    clear() {
      axios
        .post("http://193.46.198.162:5700/entities/clear", { entity: "stocktake" })
        .then(res => {
          console.log(res);
          this.$root.$emit("refreshData");
        });
    }
  },
  name: "Header"
};
</script>


<style scoped>
#nav {
  padding: 15px;
  background: #2f495e;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-start;
}

#nav a {
  font-weight: bold;
  color: #939393;
  text-decoration: none;
  margin-left: 1em;
}
.upload-btn {
  background: #00c58e;
  color: #fff !important;
  padding: 0.6em;
  border-radius: 3px;
  margin-left: 1em;
}

#nav a.router-link-exact-active {
  color: #fff;
}
</style>