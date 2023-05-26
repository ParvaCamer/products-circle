<template>
    <i class="fa-solid fa-bars" :class="{ isActive: display }"></i>
</template>

<script>
export default {
    props: ['display']
}
</script>

<style lang="scss" scoped>
i {
    display: none;
}

@media screen and (max-width: 1224px) {
    i {
        display: inline-block;
        position: absolute;
        top: 20px;
        left: 15px;
        font-size: 30px;
        color: #564AFF;
        z-index: 3;
        transition: color .75s ease-in-out;

        &.isActive {
            color: #ffffff;
        }
    }
}
</style>