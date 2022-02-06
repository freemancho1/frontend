<template>
    <div class="page-category">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h2 class="is-size-2 has-text-centered">{{category.name}}</h2>
            </div>
            <ProductBox v-for="product in category.products" :key="product.id" :product="product" />
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { toast } from 'bulma-toast';

import ProductBox from '@/components/ProductBox';

export default {
    name: 'Category',
    data() {
        return {
            category: {
                products: [],
            },
        };
    },
    watch: {
        // This case is works fine,
        // but since it's a router related change, use the part below
        // category() {
        //     this.getCategory();
        // },
        $route(to, from) {
            if (to.name === 'Category') {   // The router name in router/index.js
                this.getCategory();
            }
        },
    },
    components: {
        ProductBox,
    },
    methods: {
        async getCategory() {
            this.$store.commit('setIsLoading', true);

            const categorySlug = this.$route.params.category_slug;

            axios 
                .get(`/api/v1/products/${categorySlug}`)
                .then(response => {
                    this.category = response.data;
                    document.title = this.category.name + " : Freeman\'s Shop";
                })
                .catch(error => {
                    console.log('System' + error);
                    toast({
                        message: 'Something went wrong. Please try again.',
                        type: 'is-danger',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 5000,
                        position: 'bottom-center',
                    });
                })

            this.$store.commit('setIsLoading', false);
        }
    },
    mounted() {
        this.getCategory();
    },
}
</script>

<style scoped>
</style>