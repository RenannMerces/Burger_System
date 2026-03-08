<template>
  <div>
    <NavBar :logo="logo_src" :alt="app_name"/>
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
      }, data(){
        return{
          logo_src: "/img/logo.png",
          app_name: "Burger System"
        }
      }
    }
</script>

<style>

*{
  border: 0;
  margin: 0;
  padding: 0;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  box-sizing: border-box;
}

.main-container{
  margin: 50px;
  min-height: 250px;

}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}

</style>
