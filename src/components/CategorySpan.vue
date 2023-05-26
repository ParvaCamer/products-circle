<template>
    <p>
        <span :class="category.replace(/[' ]/g, '')">{{ category }}</span>
    </p>
</template>

<script>
export default {
    props: ['category']
}
</script>

<style lang="scss" scoped>
span {
    font-size: .95rem;
    font-weight: 300;
    color: #ffffff;
    background: #564AFF;
    padding: 8px 12px;
    border-radius: 18px;

    &.mensclothing {
        background: #FFA846;
    }

    &.jewelery {
        background: #32B436;
    }

    &.electronics {
        background: #f1e024;
    }

    &.womensclothing {
        background: #564AFF;
    }

    @media screen and (max-width: 1224px) {

        font-size: .575rem;

    }
}
</style>