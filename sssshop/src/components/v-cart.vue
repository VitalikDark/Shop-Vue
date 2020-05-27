<template>
  <div class='v-cart'>
    <router-link :to="{name: 'catalog'}">
      <div class="v-catalog__link_to_cart">Back to Catalog</div>
    </router-link>
    <h1>Cart</h1>
    <p v-if="!cart_data.length">There are no products in cart...</p>
    <v-cart-item
        v-for="(item, index) in cart_data"
        :key="item.article"
        :cart_item_data="item"
        @deleteFromCart="deleteFromCart(index)"
        @Plus="Plus(index)"
        @unPlus="unPlus(index)"
    />
    <div class="v-card__total">
      <div >Total</div>
      <span class="total__price">{{totalPrice}}</span>
    </div>
  </div>
</template>

<script>
  import vCartItem from './v-cart-item'
  import {mapActions} from 'vuex'

  export default {
    name: "v-cart",
    components: {
      vCartItem
    },
    props: {
      cart_data: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {}
    },
    computed: {
      totalPrice() {
        let rezult=[];
        if (this.cart_data.length) {
            for (let item of this.cart_data) {
            rezult.push(item.price*item.quantity);
          }

          
          rezult=rezult.reduce(function(sum, el) {
            return sum+el;
          });
          return rezult
        }
        else {
          return 0;
        }
        
        
      }
    },
    methods: {
      ...mapActions([
        'DELETE_FROM_CART',
        'PLUS_CARD_ITEM',
        'UNPLUS_CARD_ITEM'
      ]),
      Plus(index) {
        this.PLUS_CARD_ITEM(index)
      },
      unPlus(index) {
        this.UNPLUS_CARD_ITEM(index)
      }, 
      deleteFromCart(index) {
        this.DELETE_FROM_CART(index)
      }
    }
  }
</script>

<style scoped>
  .v-card__total {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;

  }
  .total__price {
    color: #333;
  }
</style>