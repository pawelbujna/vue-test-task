<template>
  <div id="app">

  </div>
</template>

<script>
  import _ from 'lodash';

  export default {
    name: 'app',
    data() {
      return {
        users: ['john', 'paul', 'jacub'],
        admins: ['jacob', 'paul', 'tom']
      }
    },
    created() {
      this.takeSimilar();
    },
    methods: {
      takeSimilar() {
        console.log(_.difference(this.users, this.admins));
      }

    }
  }
</script>

<style>

</style>