<template>
    <div class="page-search">
         <div class="columns is-multiline">
             <div class="column is-12">
                 <h1 class="title">Search</h1>
                 <h2 class="is-size-5 has-text-grey">Search term: "{{query}}"</h2>
             </div>
             <ProductBox v-for="product in products" :key="product.id" :product="product"/>
         </div>
    </div>
</template>

<script>
import axios from 'axios';
import ProductBox from '@/components/ProductBox';

export default {
    name: 'Search',
    data() {
        return {
            products: [],
            query: '',
        };
    },
    methods: {
        async performSearch() {
            this.$store.commit('setIsLoading', true);

            await axios
                .post('/api/v1/products/search/', {'query': this.query})
                .then(response => {
                    this.products = response.data;
                    console.log(this.products);
                })
                .catch(error => {
                    console.log('Search' + error);
                });

            this.$store.commit('setIsLoading', false);
        }
    },
    mounted() {
        document.title = 'Search | Freeman\'s Shop';

        let uri = window.location.search.substring(1);
        let params = new URLSearchParams(uri);

        if (params.get('query')) {
            this.query = params.get('query');
            this.performSearch();
        }
    },
    components: {
        ProductBox,
    },
}
</script>