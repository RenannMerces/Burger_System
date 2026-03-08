<template>
    <div id="nav">
        <router-link id="logo-url" to="/"><img id="logo" :src="logo" :alt="alt" ></router-link> |
        <router-link to="/">Home</router-link>
        <router-link to="/pedidos">Pedidos</router-link>
    </div>
</template>

<script>
    export default {
        name: 'NavBar',
        props: ["logo", "alt"]

    }
</script>

<style scoped>

    #nav{
        background-color: #222;
        border-bottom: 4px solid #111;
        padding: 15px 50px;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }

    #nav a{
        color: #fcba26;
        text-decoration: none;
        margin-left: 20px;
        font-size: 18px;
        transition: .5s;
    }

    #nav a:hover{
        color: #fff;
        text-decoration: underline;
    }

    #logo-url{
        margin: auto;
        margin-left: 0;
    }

    #logo{
        width: 40px;
        height: 40px;
    }



</style>