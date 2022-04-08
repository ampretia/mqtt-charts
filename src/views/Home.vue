<template>
  <div class="home">
    <h1 class="title">Marvin Configurator</h1>
    <div class="section">
      <div class="box has-text-left">
        <h2 class="subtitle">Chaincode</h2>
        <div
          v-for="(versions, modules) in versions('chaincode')"
          v-bind:key="modules"
        >
          For module written to <b>{{ modules }} </b>
          <div
            v-for="(versionchoices, modulename) in versions"
            v-bind:key="versionchoices"
          >
            <version-select
              class="pb-3"
              :versions="versionchoices"
              :title="modulename"
              @update="update($event, 'chaincode', modules, modulename)"
            />
          </div>
        </div>
        <h2 class="subtitle">SDK</h2>
        <div
          v-for="(versions, modules) in versions('sdk')"
          v-bind:key="modules"
        >
          For module written to <b>{{ modules }} </b>
          <div
            v-for="(versionchoices, modulename) in versions"
            v-bind:key="versionchoices"
          >
            <version-select
              class="pb-3"
              :versions="versionchoices"
              :title="modulename"
              @update="update($event, 'chaincode', modules, modulename)"
            />
          </div>
        </div>
      </div>
      <div class="box has-text-left">
        <pre classs="has-text-left">{{ tocopy }}</pre>
      </div>
      <div class="box has-text-left">
        <VueJsonPretty v-bind:data="updatedversions" :key="componentKey">
        </VueJsonPretty>
      </div>
      <!-- <div class="buttons">
        <b-button type="is-primary" @click="submit">SUBMIT!!</b-button>
      </div> -->
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
    return { updatedversions: {}, componentKey: 0, tocopy: "" };
  },
  methods: {
    versions(comp) {
      return json[comp];
    },
    update(e, comp, modules, modulename) {
      if (!this.updatedversions[comp]) {
        this.updatedversions[comp] = {};
      }
      if (!this.updatedversions[comp][modules]) {
        this.updatedversions[comp][modules] = {};
      }
      this.updatedversions[comp][modules][modulename] = e.e.id;
      this.componentKey += 1;

      this.tocopy = `curl -H "Content-Type: application/json" --request POST  \\
   -H "Accept: application/json" \\
   -H "marvin: fb7f354c24a3974dc4432fb4ca6738f50db17d55a07c931f" \\
   -d '${JSON.stringify(this.updatedversions)}' \\
   https://devops-api.eu-gb.devops.cloud.ibm.com/v1/tekton-webhook/3648c0a9-a5da-478b-ab5b-d3712ec2eca6/run/94d17fdd-b50f-4a35-ae5f-5ea200f44811 `;
    },
    async submit() {
      let TOKEN = "fb7f354c24a3974dc4432fb4ca6738f50db17d55a07c931f";
      let HOST =
        "https://devops-api.eu-gb.devops.cloud.ibm.com/v1/tekton-webhook/3648c0a9-a5da-478b-ab5b-d3712ec2eca6/run/94d17fdd-b50f-4a35-ae5f-5ea200f44811";
      var headers = {
        Accept: "application/json",
        "Content-Type": "application/json",
        marvin: TOKEN,
        "Access-Control-Allow-Origin": "https://pages.github.ibm.com",
        crossorigin: true,
      };

      var options = {
        url: HOST,
        method: "POST",
        headers: headers,
        data: {
          versions: this.updatedversions,
        },
      };

      let result = await this.$http(options);
      if (result.status != 200) {
        console.log(result.response.statusText);
      } else {
        console.log(JSON.stringify(result.data));
      }

      //
      console.log();
    },
  },
};
</script>
