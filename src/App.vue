<template>
  <div>
    <NavBar/>
    <router-view/>
    <FooterHome/>
  </div>
</template>

<script>
  import NavBar from "@/components/NavBar.vue";
  import FooterHome from "@/components/FooterHome.vue";

      export default {
        name: 'App',
        components: {
          NavBar,
          FooterHome
      }, props: {
          
      }
    }
</script>

<style>

</style>
