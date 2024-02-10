<template>
  <div
    class="m-auto lg:pb-12 xs:pb-8 max-w-screen-xl lg:my-10 xs:my-6 w-4/5 bg-white h-fit rounded-2xl shadow-2xl"
  >
    <the-cart-block v-if="cartOpen"></the-cart-block>
    <the-header :total-price="totalPrice" @onOpenCart="openCart"></the-header>
    <router-view></router-view>
  </div>
</template>

<script setup>
import TheHeader from '@/components/TheHeader.vue'
import axios from 'axios'
import TheCartBlock from '@/components/TheCartBlock.vue'
import { ref, watch, provide, onMounted, computed } from 'vue'

const cartItems = ref([])
const cartOpen = ref(false)
const isCreatedOrder = ref(false)
const products = ref([])
const overFlowHtml = ref('auto')
const showOrderCreated = ref(undefined)
const buttonCartDisabled = computed(() => isCreatedOrder.value || cartIsEmpty.value)
const tax = computed(() => Math.round(totalPrice.value * 5) / 100)
const cartIsEmpty = computed(() => cartItems.value.length === 0)
const totalPrice = computed(() => cartItems.value.reduce((a, i) => a + i.price, 0))

provide('paymentInfo', { totalPrice, tax, cartIsEmpty, showOrderCreated })

const getProducts = async (params = { sortBy: 'title' }) => {
  try {
    let response = await axios.get('https://91d2bdc954865766.mokky.dev/sneakers', {
      params: params
    })
    let favourites = await axios.get('https://91d2bdc954865766.mokky.dev/favourites')
    products.value = response.data.map((prod) => {
      let checkFav = favourites.data.find((fav) => fav.prodId === prod.id)
      return {
        ...prod,
        isFavourite: checkFav ? true : false,
        favouriteId: checkFav ? checkFav.id : null,
        isAdded: false
      }
    })
  } catch (error) {
    console.log(error)
  }
}
provide('products', { products, getProducts })
onMounted(async () => {
  let localCart = localStorage.getItem('cart')
  cartItems.value = localCart ? JSON.parse(localCart) : []
  await getProducts()
  products.value = products.value.map((prod) => ({
    ...prod,
    isAdded: cartItems.value.find((cartProd) => cartProd.id == prod.id) ? true : false
  }))
  const test = async () => {
    let response = await axios.get('https://api.ipify.org?format=json')
    console.log(response.data.ip)
    let location = await axios.get(`http://ipwho.is`).then((resp) => console.log(resp.data))
  }
  test()
})

const addToFavourites = async (id) => {
  try {
    let product = products.value.find((prod) => prod.id == id)
    if (!product.isFavourite) {
      let obj = {
        prodId: id,
        product: product
      }
      product.isFavourite = !product.isFavourite
      let response = await axios.post('https://91d2bdc954865766.mokky.dev/favourites', obj)
      product.favouriteId = response.data.id
    } else if (product.isFavourite) {
      await axios.delete(`https://91d2bdc954865766.mokky.dev/favourites/${product.favouriteId}`)
      product.isFavourite = !product.isFavourite
    } else if (product.favouriteId == null) {
      console.log('null')
    }
  } catch (error) {
    console.log(error)
  }
}

provide('addToFavourites', addToFavourites)

watch(
  cartItems,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartItems.value))
  },
  { deep: true }
)

const openCart = () => {
  overFlowHtml.value = 'hidden'
  cartOpen.value = true
  updateHtmlOverflow()
}

const closeCart = () => {
  overFlowHtml.value = 'auto'
  cartOpen.value = false
  updateHtmlOverflow()
}

const createOrder = async () => {
  try {
    isCreatedOrder.value = true
    let { data } = await axios.post('https://91d2bdc954865766.mokky.dev/orders', {
      products: cartItems.value,
      totalPrice: totalPrice.value
    })

    cartItems.value.map((cartProd) => {
      let product = products.value.find((prod) => prod.id == cartProd.id)
      product.isAdded = false
    })

    cartItems.value = []

    showOrderCreated.value = data.id
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreatedOrder.value = false
  }
}

const updateHtmlOverflow = () => {
  document.documentElement.style.overflowY = overFlowHtml.value
}

const onClickCartAction = async (item) => {
  let product = products.value.find((prod) => prod.id == item.id)
  if (!product.isAdded) {
    product.isAdded = true
    addToCart(product)
  } else {
    product.isAdded = false
    let cartProduct = cartItems.value.find((cartItem) => cartItem.id == item.id)
    removeFromCart(cartProduct)
  }
}

const addToCart = (item) => {
  cartItems.value.push(item)
}

const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

provide('onClickCartAction', onClickCartAction)
provide('cart', { cartItems, closeCart, cartOpen, removeFromCart, createOrder, buttonCartDisabled })

// вариант на снятие isAdded, deep это все изменения
// watch(
//   cartItems,
//   () => {
//     products.value = products.value.map((prod) => ({
//       ...prod,
//       isAdded: false
//     }))
//     console.log(1)
//   },
//   {
//     deep: true
//   }
// )
</script>

<style></style>
