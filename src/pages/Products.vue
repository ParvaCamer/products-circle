<template>
    <div class="dashboard">
        <div v-if="displayDashboard">
            <h2>Products management</h2>
            <div class="dashboard__table">
                <TableContent :is-header="true" @emit-category="getCategory" />
                <TableContent @click="toggleDisplay(product.id)" v-for="product in productsToDisplay" :key="product.id"
                    :productName="product.title" :category="product.category" :price="product.price"
                    :calculPriceVAT="calculPriceVAT" />
            </div>
        </div>
        <ProductDetail v-else :products="products" :calculPriceVAT="calculPriceVAT" @emit-display="handleEmitDisplay" />
    </div>
</template>

<script>
import TableContent from '../components/Table/TableContent.vue';
import ProductDetail from '../components/ProductDetail.vue';
export default {
    components: {
        TableContent,
        ProductDetail
    },
    props: {
        deletePath: Function
    },
    data() {
        return {
            products: [],
            filteredProducts: [],
            displayDashboard: true,
            displayCategory: null
        }
    },
    async mounted() {
        const length = await this.checkDatabase();
        if (length === 0) { /* si la BDD json est vide, on fetch l'api et on envoie les data dans la BDD json*/
            await this.fetchUrl('https://fakestoreapi.com/products');

            /* la fonction map() sert à ne pas avoir de tableau imbriqué */
            const promises = this.products.map(async (product) => {
                const post = await fetch("http://localhost:3000/products", {
                    method: "POST",
                    body: JSON.stringify(product),
                    headers: { 'Content-Type': 'application/json' }
                });

                if (post.ok) {
                    const response = await post.json();
                    console.log("Product added :", response);
                    return response;
                } else {
                    console.error("Error when adding a product :", post.status);
                    throw new Error("Error when adding a product");
                }
            });

            try {
                const createdProducts = await Promise.all(promises);
                console.log("All products have been added :", createdProducts);
            } catch (error) {
                console.error("Error when adding products :", error);
            }
        } else { /* sinon, on fetch la BDD json */
            this.fetchUrl('http://localhost:3000/products');
        }
    },
    computed: {
        productsToDisplay() {
            return this.filteredProducts.length ? this.filteredProducts : this.products;
        }
    },
    methods: {
        async checkDatabase() {
            /*  vérifie la longueur de la base de donnée json */
            const response = await fetch("http://localhost:3000/products");
            const data = await response.json();
            return data.length;
        },
        async fetchUrl(url) {
            const response = await fetch(url);
            const data = await response.json();
            this.products = data;
        },
        toggleDisplay(productId) {
            /* fonction pour afficher/cacher le ProductDetail */
            this.displayDashboard = !this.displayDashboard;
            const urlParams = new URLSearchParams(window.location.search); //récupération de l'URL de la fenêtre
            urlParams.set('productId', productId);
            const newUrl = window.location.pathname + (urlParams.toString() ? '?' + urlParams.toString() : '');
            window.history.pushState({ path: newUrl }, '', newUrl); // envoie de l'Url avec les nouveaux paramètres 
        },
        handleEmitDisplay(value) {
            /* fonction pour afficher le dashboard */
            this.displayDashboard = value;
            this.deletePath();
        },
        getCategory(value) {
            /* filtrer les datas en fonction de la catégorie cliquée */
            this.filteredProducts = this.products.filter(product => product.category === value);;
        },
        calculPriceVAT(value) {
            /* calcul du prix du produit avec la TVA */
            return (value * 1.2).toFixed(2);
        }
    }
}
</script>

<style lang="scss" scoped>
.dashboard {
    h2 {
        text-align: center;
        font-size: 2.5rem;
        margin: 75px 0;
        color: #564AFF;
    }
}
</style>