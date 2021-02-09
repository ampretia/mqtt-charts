<template>
  <div class="home">
    <h1 class="title">Marvin Configurator</h1>
    <div class="section">
      <div class="box has-text-left">
        <version-select
          class="pb-6"
          :versions="versions('chaincode', 'node')"
          title="Node Chaincode (release14)"
          @update="update($event, 'chaincode', 'node', 'release14')"
        />

        <version-select
          class="pb-6"
          :versions="versions('chaincode', 'node')"
          title="Node Chaincode (release20)"
          @update="update($event, 'chaincode', 'node', 'release20')"
        />

        <version-select
          class="pb-6"
          :versions="versions('sdk', 'node')"
          title="Node SDK (release14)"
          @update="update($event, 'sdk', 'node', 'release14')"
        />
      </div>
      <div class="box has-text-left">
        <VueJsonPretty v-bind:data="updatedversions" :key="componentKey">
        </VueJsonPretty>
      </div>
      <div class="buttons">
        <b-button type="is-primary" @click="submit">SUBMIT!!</b-button>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import VersionSelect from "@/components/VersionSelect";
import json from "@/json/candidate-versions.json";
import VueJsonPretty from "vue-json-pretty";
import "vue-json-pretty/lib/styles.css";

export default {
  name: "Home",
  components: { VersionSelect, VueJsonPretty },
  data() {
    return { updatedversions: {}, componentKey: 0 };
  },
  methods: {
    versions(comp, lang) {
      return json[comp][lang];
    },
    update(e, comp, lang, release) {
      if (!this.updatedversions[comp]) {
        this.updatedversions[comp] = {};
      }
      if (!this.updatedversions[comp][lang]) {
        this.updatedversions[comp][lang] = {};
      }
      if (comp === "chaincode") {
        if (lang === "node") {
          this.updatedversions[comp][lang][release] = {
            "fabric-shim": e.e.id,
            "fabric-contract-api": e.e.id,
          };
        } else if (lang === "java") {
          console.log();
        }
      } else if (comp === "sdk") {
        console.log();
      }

      this.componentKey += 1;
    },
    async submit() {
      let TOKEN = "fb7f354c24a3974dc4432fb4ca6738f50db17d55a07c931f";
      let HOST =
        "https://devops-api.eu-gb.devops.cloud.ibm.com/v1/tekton-webhook/3648c0a9-a5da-478b-ab5b-d3712ec2eca6/run/94d17fdd-b50f-4a35-ae5f-5ea200f44811";
      var headers = {
        Accept: "application/json",
        "Content-Type": "application/json",
        marvin: TOKEN,
        "Access-Control-Allow-Origin":"*",
        crossorigin:true
      };

      var options = {
        url: HOST,
        method: "POST",
        headers: headers,
        data: {
          versions:this.updatedversions,
        },
      };


      let result = await this.$http(options);
      if (result.status != 200) {
        console.log(result.response.statusText)
      } else {
        console.log(JSON.stringify(result.data));
      }

      //
      console.log();
    },
  },
};
</script>
