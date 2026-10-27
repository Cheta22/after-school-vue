
<script setup>
import { ref, computed } from 'vue'

const sortBy = ref('subject')
const sortOrder = ref('ascending')
const cart = ref([])
const showCart = ref(false)
const customerName = ref('')
const customerPhone = ref('')
const orderSubmitted = ref(false)

const cartTotal = computed(() => {
  return cart.value.reduce((total, lesson) => {
    return total + lesson.price
  }, 0)
})

const isNameValid = computed(() => {
  return /^[A-Za-z ]+$/.test(customerName.value.trim())
})

const isPhoneValid = computed(() => {
  return /^[0-9]+$/.test(customerPhone.value)
})

const lessons = ref([
  {
    id: 1001,
    subject: 'Mathematics',
    location: 'Hendon',
    price: 250,
    spaces: 10,
    image: '/images/mathematics.jpg'
  },
  {
    id: 1002,
    subject: 'French',
    location: 'CatHill',
    price: 150,
    spaces: 15,
    image: '/images/mathematics.jpg'
  },
  {
    id: 1003,
    subject: 'Science',
    location: 'Brent Cross',
    price: 250,
    spaces: 10,
    image: '/images/science.jpg'
  },
    {
    id: 1004,
    subject: 'History',
    location: 'Hendon',
    price: 220,
    spaces: 6,
    image: '/images/history.jpg'
  },
  {
    id: 1005,
    subject: 'Geography',
    location: 'Finchley',
    price: 240,
    spaces: 9,
    image: '/images/geography.jpg'
  },
  {
    id: 1006,
    subject: 'Art',
    location: 'Mill Hill',
    price: 180,
    spaces: 7,
    image: '/images/art.jpg'
  },
  {
    id: 1007,
    subject: 'Music',
    location: 'Hendon',
    price: 280,
    spaces: 5,
    image: '/images/music.jpg'
  },
  {
    id: 1008,
    subject: 'Computer Science',
    location: 'Finchley',
    price: 320,
    spaces: 10,
    image: '/images/computerscience.jpg'
  },
  {
    id: 1009,
    subject: 'English',
    location: 'Brent Cross',
    price: 230,
    spaces: 4,
    image: '/images/english.jpg'
  },
  {
    id: 1010,
    subject: 'Drama',
    location: 'Mill Hill',
    price: 210,
    spaces: 11,
    image: '/images/drama.jpg'
  }
])

const sortedLessons = computed(() => {
  return [...lessons.value].sort((a, b) => {
    let result
    if(sortBy.value === 'subject' || sortBy.value === 'location'){
      result = a[sortBy.value].localeCompare(b[sortBy.value])
    }else{
      result = a[sortBy.value] - b[sortBy.value]
    }
    return sortOrder.value === 'ascending'? result : -result
  })
})

function addToCart(lesson){
  if(lesson.spaces > 0){
    cart.value.push(lesson)
    lesson.spaces--
  }
}


function removeFromCart(index) {
  const lesson = cart.value[index]
  lesson.spaces++
  cart.value.splice(index, 1)
}

function checkout() {
  orderSubmitted.value = true
}



</script>

<template>
<h1>After School Lessons</h1>

<button @click="showCart = !showCart"
:disabled="cart.length === 0">
{{ showCart ? 'Back to Lessons' : 'Cart (' + cart.length + ')' }}
</button>

<label for="sort">Sort lessons by:</label>

<select id="sort" v-model="sortBy">
  <option value="subject">Subject</option>
  <option value="location">Location</option>
  <option value="price">Price</option>
  <option value="spaces">Available spaces</option>
</select>

<label for="sortOrder">Sort direction:</label>
<select id="sortOrder" v-model="sortOrder">
<option value="ascending">Ascending</option>
<option value="descending">Descending</option>
</select>

<div v-if="!showCart">
<div v-for="lesson in sortedLessons" :key="lesson.id">
  <i class="fa-solid fa-book-open lesson-icon" aria-hidden="true"></i>

  <img :src="lesson.image" :alt="lesson.subject" width="150">
    <h2>{{ lesson.subject }}</h2>
    <p>Location: {{ lesson.location }}</p>
    <p>Price: Rs {{ lesson.price }}</p>
    <p>Spaces available: {{ lesson.spaces }}</p>
  

  <button
  @click="addToCart(lesson)"
  :disabled="lesson.spaces === 0"
  >
  Add to Cart
  </button>
  </div>
  </div>
  <div v-else>
  <h2>Your Cart</h2>

  <p v-if="cart.length === 0">Your cart is empty.</p>

  <p>Total: Rs {{ cartTotal }}</p>

  <h3>Checkout</h3>

<label for="custonpm.cmd run devmerName">Full Name:</label>
<input
  id="customerName"
  type="text"
  v-model="customerName"
  placeholder="Enter your full name"
/>
<p v-if="customerName && !isNameValid">
Please enter a name using letters only
</p>

<label for="customerPhone">Phone Number:</label>
<input
  id="customerPhone"
  type="text"
  v-model="customerPhone"
  placeholder="Enter your phone number"
/>

<p v-if="customerPhone && !isPhoneValid">
Please enter digits only for your phone number
</p>

<button @click="checkout"
  :disabled="cart.length === 0 || !isNameValid || !isPhoneValid"
>
  Checkout
</button>

<p v-if="orderSubmitted">
  Thank you, {{ customerName }}! Your order has been submitted successfully.
</p>

    <div v-for="(lesson, index) in cart" :key="index">
    <h3>{{ lesson.subject }}</h3>
    <p>Location: {{ lesson.location }}</p>
    <p>Price: Rs {{ lesson.price }}</p>

<button @click="removeFromCart(index)">
  Remove
</button>
  </div>


  </div>
</template>

<style scoped>
</style>
