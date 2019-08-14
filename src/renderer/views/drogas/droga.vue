<template>
  <droga-list></droga-list>
</template>

<script>
import DrogaList from "@components/drogas/droga-list";
export default {
  name: "Droga",
  data() {
    return {};
  },
  metaInfo: {
    title: "Droga",
    meta: [{ name: "description", content: "Page description goes here." }]
  },
  methods: {},
  components: {
    "droga-list": DrogaList
  },
  beforeUpdate() {},
  updated() {},
  beforeMount() {}
};
</script>

