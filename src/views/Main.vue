<template>

    <main>
        <CategoryList :products="products" :categories="categories" :productAddBasket="productAddBasket"/>
    </main>

</template>


<script>
    import CategoryList from '../components/CategoryList.vue';
    import axios from 'axios';

    export default {
        name: 'ViewMain',
        components: {
            CategoryList,
        },

        data() {
            return {
                products: null,
                categories: null,
                basketList: [],
            }
        },

        methods: {
            productList() {
                axios
                .get(`${process.env.VUE_APP_URL}/products`)
                .then((response) => {
                    this.products = response.data;
                    console.log(this.products);
                });
            },

            categoryList() {
                axios
                .get(`${process.env.VUE_APP_URL}/categories`)
                .then((response) => {
                    this.categories = response.data;
                    console.log(this.categories);
                });
            },

            productAddBasket(item) {
                console.log(item);

                // Retrieving localStorage
                if (localStorage.product && localStorage.product.length > 0 && this.basketList.length <= 0) {
                    this.basketList = JSON.parse(localStorage.product);
                }

                // Add basket
                this.basketList.push(item);
                localStorage.setItem('product', JSON.stringify(this.basketList));
            },
        },

        mounted() {
            this.productList();
            this.categoryList();
        },
    }

</script>


<style lang="scss">
    main {
        padding: 20px;
    }
</style>