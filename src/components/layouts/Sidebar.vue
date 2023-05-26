<template>
    <BurgerMenu @click="displaySidebar" :display="displayBar" />
    <div class="sidebar" :class="{ animation: displayBar }">
        <div class="sidebar__header">
            <div class="sidebar__header__circle">
                <div class="circle__purple"></div>
            </div>
            <div class="sidebar__header__title">
                <h1>Circle</h1>
                <h1>Products</h1>
            </div>
        </div>
        <div class="sidebar__content">
            <h2>Dashboard</h2>
            <div class="sidebar__content__links">
                <p><span></span> Products management</p>
                <p>Employees management</p>
            </div>
            <p>Logout</p>
        </div>
    </div>
</template>

<script>
import BurgerMenu from './BurgerMenu.vue';
export default {
    components: {
        BurgerMenu
    },
    data() {
        return {
            displayBar: false
        }
    },
    methods: {
        displaySidebar() {
            this.displayBar = !this.displayBar;
        }
    }
}
</script>

<style lang="scss" scoped>
.sidebar {
    background: #564AFF;
    color: #ffffff;
    height: 100vh;
    width: 275px;
    position: fixed;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 80px 50px;
    transition: all 1s ease-in-out;
    z-index: 2;

    &__header {
        display: flex;
        gap: 20px;

        &__circle {
            position: relative;
            display: grid;
            place-items: center;
            width: 65px;
            height: 65px;
            border-radius: 50%;
            background: #ffffff;

            .circle__purple {
                position: absolute;
                height: 45px;
                width: 45px;
                border-radius: 50%;
                background: #564AFF;
            }
        }

        &__title {
            text-align: left;

            h1 {
                font-size: 1.5rem;
                margin: 0;
            }
        }
    }

    .sidebar__content {
        width: 100%;
        margin-top: 150px;
        text-align: left;
        display: grid;
        gap: 40px;

        h2,
        p {
            font-size: 1.05rem;
            margin: 0;
        }

        &__links {
            display: grid;
            align-items: center;
            gap: 35px;
            padding-bottom: 35px;
            border-bottom: 1px solid #8F88FF;

            span {
                display: inline-block;
                height: 10px;
                width: 10px;
                margin-right: 10px;
                border-radius: 50%;
                background: #ffffff;
            }
        }
    }

    @media screen and (max-width: 1224px) {
        padding: 80px 0;
        transform: translateX(-400px);

        &.animation {
            width: 100%;
            transform: translateX(0);
        }

        &__content {
            width: 80% !important;
            margin-top: 100px !important;
        }
    }
}
</style>