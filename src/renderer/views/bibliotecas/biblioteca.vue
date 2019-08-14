<template>

  <biblioteca-list></biblioteca-list>
</template>

<script>
import BibliotecaList from "@components/bibliotecas/biblioteca-list";
export default {
  name: "Biblioteca",
  data() {
    return {};
  },
  metaInfo: {
    title: "Biblioteca",
    meta: [{ name: "description", content: "Page description goes here." }]
  },
  methods: {},
  components: {
    "biblioteca-list": BibliotecaList
  },
  beforeUpdate() {},
  updated() {},
  beforeMount() {}
};
</script>

