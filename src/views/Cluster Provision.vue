<template>
    <div class="section">
      <div class="box has-text-left">
        <h2 class="subtitle">Cluster Provision</h2>
        Provision New or Use Existing
        <div class="block">
            <b-radio v-model="provision"
                name="name"
                native-value="Provision">
                Flint
            </b-radio>
            <b-radio v-model="provision"
                name="name"
                native-value="Existing">
                Silver
            </b-radio>
        </div>
        Cluster Type
        Cluster Version

        Cluster Name
        Cluster ID


    </div>

</template>

<script>
// @ is an alias to /src
import "vue-json-pretty/lib/styles.css";

export default {
  name: "Home",
  components: {  },
  data() {
    return { provision };
  },
  methods: {
  },
};
</script>
