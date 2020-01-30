
<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import axios from "axios/dist/axios";
import List from "./components/List";
import Domain from "./components/Domain";
import ChangeTitleInput from "./components/ChangeTitleInput";

export default {
  name: "app",
  components: {
    List,
    Domain,
    ChangeTitleInput
  },
  data: function() {
    return {
      prefixes: [],
      sufixes: [],
      title: 'Name Generator'
    };
  },
  methods: {
    addPrefix(prefix) {
      // this.prefixes.push(prefix);
      axios({
        url: "http://localhost:4000",
        method: "post",
        data: {
          query: `
            mutation ($item: ItemInput) {
             newPrefix: saveItem(item: $item) {
               id
               type
               description
             }
           }
        `,
          variables: {
            item: {
              type: "prefix",
              description: prefix
            }
          }
        }
      });
    },
    addSufix(sufix) {
      this.sufixes.push(sufix);
    },
    deletePrefix(prefix) {
      this.prefixes.splice(this.prefixes.indexOf(prefix), 1);
    },
    deleteSufix(sufix) {
      this.sufixes.splice(this.sufixes.indexOf(sufix), 1);
    },
    setTitle(msg) {
      this.title = msg;
    }
  },
  computed: {
    domains() {
      console.log("generating domains...");
      const domains = [];
      for (const prefix of this.prefixes) {
        for (const sufix of this.sufixes) {
          const name = prefix + sufix;
          const url = name.toLowerCase();
          domains.push({
            name,
            link: `https://checkout.hostgator.com.br/?a=add&sld=${url}&tld=.com.br`
          });
        }
      }
      return domains;
    }
  },
  created() {
    axios({
      url: "http://localhost:4000",
      method: "post",
      data: {
        query: `
          {
            prefixes: items(type:"prefix"){
              id
              type
              description
            }
            sufixes: items(type:"sufix"){
              id
              type
              description
            }
          }
        `
      }
    }).then(response => {
      const query = response.data;
      this.prefixes = query.data.prefixes.map(prefix => prefix.description);
      this.sufixes = query.data.sufixes.map(sufix => sufix.description);
    });
  }
};
</script>



<template>
  <div>
    <div id="slogan" class="text-center">
      <h1>{{title}}</h1>
      <br />
      <h6 class="text-secondary">Name generator made in Vue.js</h6>
      <ChangeTitleInput @changeTitle="setTitle" />
    </div>
    <div id="main">
      <div class="container">
        <div class="row">
          <List
            v-bind:items="prefixes"
            name="Prefixes"
            v-on:addItem="addPrefix"
            v-on:deleteItem="deletePrefix"
          />
          <List
            v-bind:items="sufixes"
            name="Sufixes"
            v-on:addItem="addSufix"
            v-on:deleteItem="deleteSufix"
          />
        </div>
        <Domain v-bind:domains="domains" />
      </div>
    </div>
  </div>
</template>



<style>
#slogan {
  margin-top: 30px;
  margin-bottom: 30px;
}
#main {
  background-color: #f1f1f1;
  padding-top: 30px;
  padding-bottom: 30px;
}
</style>
