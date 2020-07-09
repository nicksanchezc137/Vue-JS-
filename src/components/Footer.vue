<template>
  <div id="footer">
    <a href="javascript:void(0)" v-on:click = "$emit('back-clicked')" >{{special_back}}</a>
     <a href="javascript:void(0)"  v-on:click = "$emit('next-clicked')" >></a>
  </div>
</template>

<script>
export default {
  data() {
    return {
      FILE: null,
       special_back:"<"
    };
  },
  methods: {
     
    },
  name: "Header",

};
</script>


<style scoped>
#footer {
  bottom:0;
  width:100%;
  position: fixed;
  padding: 15px;
  background: #2f495e;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content:center;
}

#footer a {
  background: #00c58e;
  color: #fff !important;
  padding: 0.6em;
  border-radius: 3px;
   margin-left: 1em;
   text-decoration: none;
}


</style>