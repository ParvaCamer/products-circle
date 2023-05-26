<template>
    <!-- si isHeader est à true, afficher le header, sinon, afficher les données de l'API -->
    <div class="dashboard__table" :class="[isHeader ? 'dashboard__table__header' : 'dashboard__table__content']">
        <p>{{ isHeader ? "Product name" : productName }}</p>
        <SelectBoxes v-if="isHeader" @emit-category="getCategory" />
        <CategorySpan v-else :category="category" />
        <p>{{ isHeader ? "Price" : price + "€" }}</p>
        <p v-if="isHeader">Price <span>(including VAT)</span></p>
        <p v-else>{{ calculPriceVAT(price) }}€</p>
    </div>
</template>

<script>
import SelectBoxes from './SelectBoxes.vue';
import CategorySpan from '../CategorySpan.vue';
export default {
    components: {
        SelectBoxes,
        CategorySpan
    },
    props: {
        isHeader: Boolean,
        productName: String,
        category: String,
        price: Number,
        calculPriceVAT: Function
    },
    methods: {
        getCategory(value) {
            this.$emit('emit-category', value);
        }
    }
}
</script>

<style lang="scss" scoped>
.dashboard__table {
    display: flex;
    align-items: center;
    padding: 5px 20px;
    margin-bottom: 8px;
    border-radius: 4px;
    box-shadow: 2px 2px 6px 0px rgba(0, 0, 0, 0.1);

    p {
        &:first-child {
            width: 50%;
        }

        &:nth-child(2) {
            width: 20%;
        }

        &:nth-child(3) {
            width: 15%;
            text-align: right;
        }

        &:nth-child(4) {
            width: 15%;
            text-align: right;
        }

        @media screen and (max-width: 1224px) {
            font-size: .75rem;

            &:first-child {
                width: 40%;
            }

            &:nth-child(2) {
                width: 30%;
            }

            &:nth-child(3) {
                width: 15%;
                text-align: right;
            }

            &:nth-child(4) {
                width: 15%;
                text-align: right;
            }
        }
    }

    &__header {
        color: #ffffff;
        background: #564AFF;
        position: relative;

        span {
            font-weight: 300;
            font-size: .95rem;
            letter-spacing: .05em;
        }

        @media only screen and (max-width: 1824px) {
            p {
                font-size: .95rem;
            }

            span {
                font-size: .65rem;
            }
        }

        @media only screen and (max-width: 1224px) {
            span {
                font-size: .4rem;
            }
        }
    }

    &__content {
        color: #707070;
        background: #ffffff;
        cursor: pointer;
    }
}
</style>