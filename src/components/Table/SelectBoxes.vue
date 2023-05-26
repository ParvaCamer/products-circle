<template>
    <form id="app-cover">
        <div id="select-box">
            <input aria-label="options-view-button" type="checkbox" name="options-view-button" id="options-view-button"
                @click="this.displayCategory = !this.displayCategory">
            <div id="select-button" class="brd">
                <div id="selected-value">
                    <span>Category</span>
                </div>
                <div id="chevrons">
                    <i class="fa-solid fa-sort-down fa-xs" :class="{ 'border-i': displayCategory }"></i>
                </div>
            </div>
            <div id="options" :class="{ 'options-border': displayCategory }">
                <div class="option" @click="toggleCategory('show all')">
                    <input aria-label="all-top" class="s-c top" id="all-top" type="radio" name="all-top" value="all">
                    <input aria-label="all-bottom" class="s-c bottom" id="all-bottom" type="radio" name="all-bottom"
                        value="all">
                    <i class="fa-solid fa-list" :class="{ 'block-i': displayCategory }"></i>
                    <span class="label" :class="{ 'block-label': displayCategory }">Show All</span>
                </div>
                <div class="option" @click="toggleCategory('electronics')">
                    <input aria-label="electronics-top" class="s-c top" id="electronics-top" type="radio"
                        name="electronics-top" value="bolt">
                    <input aria-label="electronics-bottom" class="s-c bottom" id="electronics" type="radio"
                        name="electronics-bottom" value="bolt">
                    <i class="fa-solid fa-bolt" :class="{ 'block-i': displayCategory }"></i>
                    <span class="label" :class="{ 'block-label': displayCategory }">Electronics</span>
                </div>
                <div class="option" @click="toggleCategory('jewelery')">
                    <input aria-label="jewelery-top" class="s-c top" id="jewelery-top" type="radio" name="jewelery-top"
                        value="gem">
                    <input aria-label="jewelery-bottom" class="s-c bottom" id="jewelery" type="radio" name="jewelery-bottom"
                        value="gem">
                    <i class="fa-regular fa-gem" :class="{ 'block-i': displayCategory }"></i>
                    <span class="label" :class="{ 'block-label': displayCategory }">Jewelery</span>
                </div>
                <div class="option" @click="toggleCategory('men\'s clothing')">
                    <input aria-label="mensclothing-top" class="s-c top" id="mensclothing-top" type="radio"
                        name="mensclothing-top" value="person">
                    <input aria-label="mensclothing-bottom" class="s-c bottom" id="mensclothing" type="radio"
                        name="mensclothing-bottom" value="person">
                    <i class="fa-solid fa-person" :class="{ 'block-i': displayCategory }"></i>
                    <span class="label" :class="{ 'block-label': displayCategory }">Men's clothing</span>
                </div>
                <div class="option" @click="toggleCategory('women\'s clothing')">
                    <input aria-label="womensclothing-top" class="s-c top" id="womensclothing-top" type="radio"
                        name="womensclothing-top" value="dress">
                    <input aria-label="womensclothing-bottom" class="s-c bottom" id="womensclothing-bottom" type="radio"
                        name="womensclothing-bottom" value="dress">
                    <i class="fa-solid fa-person-dress" :class="{ 'block-i': displayCategory }"></i>
                    <span class="label" :class="{ 'block-label': displayCategory }">Women's clothing</span>
                </div>
                <div id="option-bg"></div>
            </div>
        </div>
    </form>
</template>

<script>
export default {
    data() {
        return {
            displayCategory: false
        }
    },
    methods: {
        toggleCategory(category) {
            /* envoie la data category au composant parent pour faire fonctionner le filtre */
            this.displayCategory = !this.displayCategory;
            this.$emit('emit-category', category);
        }
    }
}

</script>

<style lang="scss" scoped>
#app-cover {
    position: relative;
    width: 20%;
    height: 42px;
    margin: auto 0;
    z-index: 1;

    /* INPUT */
    #options-view-button {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        width: 35%;
        height: 100%;
        margin: 0;
        opacity: 0;
        cursor: pointer;
        z-index: 3;
    }

    #select-button {
        position: relative;
        height: 16px;
        padding: 12px 0;

        #selected-value {
            font-size: 1rem;
            line-height: 1;
            margin-right: 26px;

            span {
                color: #ffffff;
            }
        }
    }
}

.option,
.label {
    color: #2d3667;
    font-size: 1rem;
}

#chevrons {
    position: absolute;
    top: 0;
    left: 65px;
    bottom: 0;
    padding: 6px 14px;
}

#chevrons i {
    font-size: 1rem;
}

.border-i {
    color: #2d3667;
}

#options {
    position: absolute;
    top: 34px;
    right: 0;
    left: 0;
    width: 298px;
    margin: 0 auto;
    background-color: #fff;
    border-radius: 4px;
    transition: height 2s ease-in;
}

.options-border {
    border: 1px solid #e2eded;
    border-color: #eaf1f1 #e4eded #dbe7e7 #e4eded;
}

.option {
    position: relative;
    line-height: 1;
    transition: 0.3s ease all;
    z-index: 2;

    input[type="radio"] {
        position: absolute;
        right: 0;
        left: 0;
        width: 100%;
        height: 50%;
        margin: 0;
        opacity: 0;
        cursor: pointer;
    }

    i {
        width: 16px;
        height: 16px;
        position: absolute;
        left: 14px;
        padding: 0;
        display: none;
    }

    .label {
        display: none;
        padding: 0;
        margin-left: 27px;
        transition: 0.3s ease all;
    }
}

.block-i {
    display: block !important;
    padding: 12px 0 !important;
}

.block-label {
    display: block !important;
    padding: 12px 14px !important;
}

.s-c {
    position: absolute;
    left: 0;
    width: 100%;
    height: 50%;

    &.top {
        top: 0;

        &:hover~i {
            top: -25px;
            animation: movedown 0.3s ease 0.1s forwards;
        }
    }

    &.bottom {
        bottom: 0;

        &:hover+i {
            bottom: -25px;
            animation: moveup 0.3s ease 0.1s forwards;
        }
    }

    &:hover {
        height: 100%;
        z-index: 1;

        &~i {
            color: #fff;
            opacity: 0;
        }
    }
}

@keyframes moveup {
    0% {
        bottom: -25px;
        opacity: 0;
    }

    100% {
        bottom: 0;
        opacity: 1;
    }
}

@keyframes movedown {
    0% {
        top: -25px;
        opacity: 0;
    }

    100% {
        top: 0;
        opacity: 1;
    }
}


.option input[type="radio"]:checked~i {
    top: 0;
    bottom: auto;
    opacity: 1;
    animation: unset;
}

.option input[type="radio"]:checked~.label:before {
    content: "";
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: -1;
}

.option .fa-list {
    color: #e85151;
}

.option .fa-bolt {
    color: #f1e024;
}

.option .fa-gem {
    color: #32B436;
}

.option .fa-person {
    color: #FFA846;
}

.option .fa-person-dress {
    color: #564AFF;
}

#option-bg {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    height: 40px;
    transition: 0.3s ease all;
    z-index: 1;
}

#options-view-button:checked~#options #option-bg {
    display: block;
}

.option:hover .label {
    color: #fff;
}

.option:nth-child(1):hover~#option-bg {
    top: 0;
    background-color: #e85151;
    border-radius: 4px 4px 0 0;
}

.option:nth-child(2):hover~#option-bg {
    top: 40px;
    background-color: #f1e024;
}

.option:nth-child(3):hover~#option-bg {
    top: 80px;
    background-color: #32B436;
}

.option:nth-child(4):hover~#option-bg {
    top: 120px;
    background-color: #FFA846;
}

.option:nth-child(5):hover~#option-bg {
    top: 160px;
    background-color: #564AFF;
    border-radius: 0 0 4px 4px;
}

@media screen and (max-width: 1224px) {
    #app-cover {
        width: 30%;

        #options-view-button {
            width: 100%;
        }
    }
}
</style>