<template>
    <div class="product__detail" v-if="product">
        <i class="fa-solid fa-arrow-left" style="color: #707070;" @click="emitDisplay"></i>
        <h2>{{ product.title }}</h2>
        <div class="product__detail__container">
            <div class="product__detail__container__picture">
                <img :src="product.image" alt="">
            </div>
            <div class="product__detail__container__description">
                <h3>Description</h3>
                <p>{{ product.description }}</p>
            </div>
            <div class="product__detail__container__category">
                <h3>Category</h3>
                <CategorySpan :category="product.category" />
            </div>
            <div class="product__detail__container__price">
                <h3>Price</h3>
                <div class="product__detail__container__price__infos">
                    <input type="number" name="price" :placeholder="product.price" v-model="price"><i>€</i>
                    <p><span>Price</span> (including VAT): {{ calculPriceVAT(product.price) }} €</p>
                </div>
            </div>
            <div class="product__detail__container__button">
                <button :disabled="disableButton" @click="updateProduct">Update product</button>
            </div>
        </div>
    </div>
</template>

<script>
import CategorySpan from './CategorySpan.vue';
export default {
    components: {
        CategorySpan
    },
    props: {
        products: Array,
        calculPriceVAT: Function
    },
    mounted() {
        window.scrollTo(0, 0);
        const urlParams = new URLSearchParams(window.location.search);
        this.selectedProductId = urlParams.get('productId') - 1; // récupération du paramètre de l'Url ex: productId=1 => je récupère 1 
        this.product = this.products[+this.selectedProductId] // conversion de selectedProductId en number et retrait de 1 pour récupérer le bon product
    },
    data() {
        return {
            selectedProductId: null,
            product: null,
            price: ''
        }
    },
    computed: {
        disableButton() {
            return this.price.length === 0; // si l'input est vide, le bouton sera disabled
        }
    },
    methods: {
        emitDisplay() {
            this.$emit('emit-display', true);
        },
        async updateProduct() {
            this.product.price = this.price;

            const url = `http://localhost:3000/products/${this.product.id}`;

            try {
                const response = await fetch(url, {
                    /* envoi du changement du prix du produit dans la base de données */
                    method: 'PATCH',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ price: this.product.price }),
                });

                if (response.ok) {
                    console.log('Product updated successfully');
                } else {
                    console.error('Failed to update product');
                }
            } catch (error) {
                console.error('An error occurred while updating the product', error);
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.product__detail {
    position: relative;
    height: 100vh;
    margin: 0 10px;

    .fa-arrow-left {
        position: absolute;
        top: 0;
        left: 0;
        font-size: 3.5rem;
        cursor: pointer;
        animation-duration: 2s;
        animation-iteration-count: infinite;

        &:hover {
            animation-name: translate-left;
            animation-timing-function: ease-in-out;
        }

        @media screen and (max-width: 1224px) {
            font-size: 2.5rem;
            top: 15px;
        }
    }

    @keyframes translate-left {

        0%,
        100% {
            transform: translateX(0);
        }

        50% {
            transform: translateX(-10px);
        }
    }

    h2,
    h3 {
        margin: 0;
        color: #564AFF;
    }

    h2 {
        text-align: center;
        font-size: 2.25rem;
        margin: 75px 0;

        @media screen and (max-width: 1824px) {
            padding: 0 50px;
        }

        @media screen and (max-width: 1224px) {
            font-size: 1.5rem;
            margin-bottom: 25px;
        }
    }

    h3 {
        font-size: 1.5rem;

        @media screen and (max-width: 1224px) {
            font-size: 1.25rem;
        }
    }

    p {
        color: #818181;
        padding-right: 10px;
    }

    &__container {
        display: grid;
        grid-template-columns: repeat(6, 1fr);
        grid-template-rows: auto auto 1fr;
        grid-column-gap: 40px;
        grid-row-gap: 20px;

        @media screen and (max-width: 1824px) {
            grid-template-columns: repeat(2, 1fr);
            grid-template-rows: repeat(4, 1fr);
            grid-gap: 20px;
        }

        @media screen and (max-width: 1224px) {
            grid-column-gap: 30px;
            grid-row-gap: 10px;
        }

        &__picture {
            grid-area: 1 / 1 / 4 / 4;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 700px;
            box-shadow: 1px 2px 10px -2px rgba(0, 0, 0, 0.1);

            img {
                width: auto;
                height: auto;
                max-width: 55%;
                max-height: 65%;
                object-fit: cover;
            }

            @media screen and (max-width: 1824px) {
                grid-area: 1 / 1 / 4 / 2;
                height: auto;
            }

            @media screen and (max-width: 1224px) {
                grid-area: 1 / 1 / 3 / 3;
            }
        }

        &__description {
            grid-area: 1 / 4 / 2 / 6;

            p {
                line-height: 1.7rem;
                font-weight: 300;
            }

            @media screen and (max-width: 1824px) {
                grid-area: 1 / 2 / 3 / 3;
            }

            @media screen and (max-width: 1224px) {
                grid-area: 3 / 1 / 4 / 3;

                p {
                    font-size: .9rem;
                }
            }
        }

        &__category {
            grid-area: 1 / 6 / 2 / 7;

            @media screen and (max-width: 1824px) {
                grid-area: 3 / 2 / 4 / 3;
            }

            @media screen and (max-width: 1224px) {
                grid-area: 4 / 2 / 5 / 3;
            }
        }

        &__price {
            grid-area: 2 / 4 / 3 / 6;
            position: relative;

            &__infos {
                display: flex;
                justify-content: space-between;

                input[type="number"] {
                    width: 6.75rem;
                    margin: 20px 0;
                    padding: 12px 15px;
                    border: 1px solid #564AFF;
                    border-radius: 4px;

                    &:focus {
                        outline: 1.5px solid #564AFF;
                    }

                    &::-webkit-inner-spin-button,
                    &::-webkit-outer-spin-button {
                        -webkit-appearance: none;
                    }
                }

                i {
                    position: absolute;
                    left: 120px;
                    top: 65px;
                    font-weight: 300;
                    color: #D8D8D8;
                }

                p {
                    font-size: .975rem;
                    font-weight: 300;
                    margin: auto 0;

                    span {
                        font-weight: 500;
                    }
                }

                @media screen and (max-width: 1824px) {
                    justify-content: initial;
                    height: auto;
                    gap: 40px;
                }

                @media screen and (max-width: 1224px) {
                    flex-direction: column;
                    gap: 0;

                    p {
                        font-size: .875rem;
                    }
                }
            }

            @media screen and (max-width: 1824px) {
                grid-area: 4 / 1 / 5 / 2;
            }
        }

        &__button {
            grid-area: 3 / 4 / 4 / 5;

            button {
                padding: 12px;
                border: none;
                border-radius: 4px;
                background: #564AFF;
                color: #ffffff;
                font-size: 1rem;
                transition: background .2s linear;

                &:disabled {
                    background: #D8D8D8;
                    cursor: not-allowed;
                }

                &:not(:disabled):hover {
                    background: #7066ff;
                    cursor: pointer;
                }
            }

            @media screen and (max-width: 1824px) {
                grid-area: 4 / 2 / 5 / 3;

                button {
                    margin-top: 55px;
                }
            }

            @media screen and (max-width: 1224px) {
                grid-area: 5 / 1 / 6 / 3;
                margin: auto;

                button {
                    margin: 20px 0;
                }
            }
        }
    }
}
</style>