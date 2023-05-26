<template>
    <div class="container">
        <Products :deletePath="deletePath" />
    </div>
</template>

<script>
import Products from '../pages/Products.vue';
import Employees from '../pages/Employees.vue';
export default {
    components: {
        Products,
        Employees
    },
    mounted() {
        this.deletePath();
    },
    methods: {
        deletePath() {
            const newUrl = `${window.location.pathname}`; /* pour supprimer le paramètre productId de l'Url */
            window.history.replaceState(null, '', newUrl);
        }
    }
}
</script>

<style lang="scss">
.container {
    width: calc(96% - 375px);
    margin-left: calc(375px + 2%);
    gap: 75px;
    display: flex;
    flex-direction: column;

    @media screen and (max-width: 1224px) {
        width: 96%;
        margin: 0 2%;
    }
}
</style>