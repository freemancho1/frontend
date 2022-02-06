<template>
<tr>
    <td><router-link :to="item.product.get_absolute_url">
        {{item.product.name}}
        </router-link></td>
    <!-- <td>{{item.product.price}}</td> -->
    <td>{{dispPrice(item)}}</td>
    <td>
        {{item.quantity}}
        <a @click="decrementQuantity(item)"><span class="icon"><i class="fas fa-minus-square"></i></span></a>
        <a @click="incrementQuantity(item)"><span class="icon"><i class="fas fa-plus-square"></i></span></a>
    </td>
    <td>{{getItemTotalValue(item)}}</td>
    <td><button class="delete" @click="removeFromCart(item)"></button></td>
</tr>
</template>

<script>
export default {
    name: 'CartItem',
    props: {
        initialItem: Object,
    },
    data() {
        return {
            item: this.initialItem,
        };
    },
    methods: {
        getItemTotalValue(item) {
            return (item.quantity * item.product.price).toLocaleString('ko-KR');
        },
        dispPrice(item) {
            return parseInt(item.product.price).toLocaleString('ko-KR');
        },
        decrementQuantity(item) {
            item.quantity--;
            if (item.quantity === 0) {
                this.$emit('removeFromCart', item);
            }
            this.updateCart();
        },
        incrementQuantity(item) {
            item.quantity++;
            this.updateCart();
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart));
        },
        removeFromCart(item) {
            this.$emit('removeFromCart', item);
            this.updateCart();
        }
    },
}
</script>
