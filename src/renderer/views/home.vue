<template>
  <div class="page_container">
    <b-container>
      <div class="text-center">
        <img src="~@/assets/logo2.png" width="500px">
      </div>

      <!-- <h2 class="text-center m-t-80">{{ $t('homepage.title') }}</h2> -->
      <!-- <p class="text-center">{{ $t('homepage.description') }}</p> -->

      <h1 class="text-center m-t-80">SamINTELLI</h1>
      <h1 class="text-center m-t-80">{{ $t('homepage.version') }}: 1.0.0</h1>
    </b-container>
  </div>
</template>

<script>
import NProgress from "nprogress/nprogress";
export default {
  computed: {
    current_number() {
      return this.$store.getters["test/current_number"];
    }
  },
  methods: {
    start_top_progress: function() {
      NProgress.start();
    },
    stop_top_progress: function() {
      NProgress.done();
    },
    increaseNumber: function() {
      this.$store.dispatch("test/increase_number");
    },
    decreaseNumber: function() {
      this.$store.dispatch("test/decrease_number");
    }
  }
};
</script>

<style lang="scss" scoped>
ul.creative_list li {
  font-size: 20px;
  margin-bottom: 13px;
  text-align: center;
  list-style: none;

  p {
    display: inline-block;
    .badge {
      font-size: 17px;
    }
  }
}
</style>
