<template>
    <footer id="footer">
        <p>Make Your Burger &copy; 2026</p>
    </footer>
</template>

<script>
    export default {
        name: 'FooterHome'
    }
</script>

<style scoped>

#footer{
    height: 200px;
    background-color: #222;
    border-top: 4px solid #111;
    padding: 15px 50px;
    color: #fcba26;
    display: flex;
    justify-content: center;
    align-items: center;
    bottom: 0;
    width: 100%;
    position: fixed;
}

#footer p{
    font-size: 18px;
}

</style>