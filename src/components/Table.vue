<template>
  <div>
    <div class="container">
      <div class="table-responsive">
        <table v-if="rows.length" class="table table-striped table-bordered table-hover">
          <thead>
            <th v-for="column in columns" v-bind:key="column._id">{{column}}</th>
          </thead>

          <tbody>
            <tr v-for="row in rows" v-bind:key="row._id">
              <td v-for="column in columns" v-bind:key="column._id">{{row[column]}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <Footer v-on:back-clicked="onBackClicked" v-on:next-clicked="onNextClicked" />
  </div>
</template>

<script>
import axios from "axios";
import Footer from "../components/Footer";
export default {
  name: "Table",
  methods: {
    onBackClicked() {
      if (this.page > 0) {
        this.page -= 1;
        this.fetchData();
      }
    },
    onNextClicked() {
      this.page += 1;
      this.fetchData();
    },
    fetchData() {
          this.rows = [];
          this.columns = [];
        console.log("refreshing data---",this.page);
      axios
        .get("http://193.46.198.162:5700/entities/getData/15/" + this.page)
        .then(res => {
          console.log(res.data);
         this.columns = Object.keys(res.data[0]).filter(x => x != "_id");
          this.rows = res.data;
        })
        .catch(err => console.error(err));
    }
  },
  components: {
    Footer
  },

  created() {
    this.$root.$on("refreshData", () => {
        setTimeout(()=>{
           this.fetchData();
        },2000)
     
    });
    axios
      .get("http://193.46.198.162:5700/entities/getData/15/1")
      .then(res => {
        console.log(res.data);
        if (res.data.length) {
          this.columns = Object.keys(res.data[0]).filter(x => x != "_id");
          this.rows = res.data;
        }
      })
      .catch(err => console.error(err));
  },

  data() {
    return { rows: [], columns: [], page: 1 };
  }
};
</script>


<style scoped>
#nav {
  padding: 30px;
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
}

#nav a.router-link-exact-active {
  color: #fff;
}

table {
  margin-bottom: 100px;
}
</style>