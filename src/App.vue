<template>
  <div class="flex items-center justify-center outline">

    <div class="flex items-center justify-center min-h-screen flex-col w-1/2 ">

      <!-- dark mode -->
      <div class="outline outline-1 my-4">
        <div class="overflow-hidden shadow-md text-gray-100">
          <!-- card header -->
          <div class="px-4 py-2 bg-gray-800 border-b border-gray-600 font-bold uppercase text-red-500">
            How to use (วิธีการใช้งาน)
          </div>

          <!-- card body -->
          <div class="py-1 px-4 bg-gray-800 border-b border-gray-600">
            <!-- content goes here -->
            วิธีการใช้งาน เพิ่ม order ด้วยการกรอกข้อมูลที่ช่อง Enter order โดยใส่เป็นชื่อ order คั้นด้วย ','
            เเล้วต่อด้วยราคา จากนั้นกดปุ่ม ADD เพื่อเพิ่ม order หากต้องการลบบาง order ให้กดปุ่ม
            "delete" (ปุ่มจะปรากฎหลังจากเพิ่ม order) หากต้องการลบ order ทั้งหมดให้กดปุ่ม "Clear All"
            หลังจากกรอกข้อมูลเสร็จสิ้นหมดเเล้วให้กดปุ่ม "Check Bill" เเล้วกรอก Head Count เเล้วกดปุ่ม "Submit"
            ระบบจะเเสดง Average cost per head
          </div>
        </div>
      </div>
      <!-- AddBar-->
      <div class="w-full flex gap-2">
        <div class='w-full outline outline-slate-200 outline-1 rounded-sm'>
          <div
            class="relative flex items-center w-full h-12 rounded-lg focus-within:shadow-lg bg-white overflow-hidden ">
            <div class="grid place-items-center  h-full w-12 text-gray-300">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
              </svg>
            </div>
            <input v-model="inputText" class="peer h-full w-full outline-none text-sm text-gray-700 pr-2" type="text"
              id="search" placeholder="Enter Order.." />
          </div>
        </div>
        <button @click="createdOrder" class="outline outline-1 outline-green-500 w-1/3 bg-green-300">ADD</button>
      </div>
      <!-- Header AddBar-->

      <!-- Order list -->
      <div class="outline outline-slate-400 w-full mt-5  outline-1">
        <h1 class="p-2">All Order</h1>
        <div class="outline outline-1 outline-slate-400 w-full max-h-80 h-80 overflow-y-auto">
          <ul v-for="item in orderList" class="mt-5 ">
            <li class="outline outline-1 outline-slate-300 m-2 p-2 rounded-md flex justify-between">{{ item.name }}
              <div class="w-1/4 flex items-center justify-between">
                {{ item.price }}
                <button @click="deletedOrder(item)" class="mr-4">delete</button>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <!-- Order list -->

      <!-- Check bill and Clear Button -->
      <div class="flex w-full gap-2 justify-end px-2 mt-2">
        <button type="button" @click="clearOrder"
          class="w-1/5 py-2 px-4 max-w-md flex justify-center items-center bg-red-600 hover:bg-red-700 focus:ring-red-500 focus:ring-offset-red-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg">
          <svg width="20" height="20" fill="currentColor" class="mr-2" viewBox="0 0 1792 1792"
            xmlns="http://www.w3.org/2000/svg">
            <path
              d="M896 786h725q12 67 12 128 0 217-91 387.5t-259.5 266.5-386.5 96q-157 0-299-60.5t-245-163.5-163.5-245-60.5-299 60.5-299 163.5-245 245-163.5 299-60.5q300 0 515 201l-209 201q-123-119-306-119-129 0-238.5 65t-173.5 176.5-64 243.5 64 243.5 173.5 176.5 238.5 65q87 0 160-24t120-60 82-82 51.5-87 22.5-78h-436v-264z">
            </path>
          </svg>
          Clear All
        </button>
        <button type="button" @click="openPopup"
          class="w-1/5 py-2 px-4 max-w-md flex justify-center items-center bg-green-600 hover:bg-green-700 focus:ring-red-500 focus:ring-offset-red-200 text-white  transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg">
          <svg width="20" height="20" fill="currentColor" class="mr-2" viewBox="0 0 1792 1792"
            xmlns="http://www.w3.org/2000/svg">
            <path
              d="M896 786h725q12 67 12 128 0 217-91 387.5t-259.5 266.5-386.5 96q-157 0-299-60.5t-245-163.5-163.5-245-60.5-299 60.5-299 163.5-245 245-163.5 299-60.5q300 0 515 201l-209 201q-123-119-306-119-129 0-238.5 65t-173.5 176.5-64 243.5 64 243.5 173.5 176.5 238.5 65q87 0 160-24t120-60 82-82 51.5-87 22.5-78h-436v-264z">
            </path>
          </svg>
          Check Bill
        </button>
      </div>
      <!-- Check bill and Clear Button -->

    </div>
  </div>

  <!-- PopUp -->
  <div v-if="isPopupOpen"
    class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-gray-800 bg-opacity-50">
    <div class="bg-white p-8 rounded-lg">
      <h2 class="text-lg font-semibold mb-4">Enter Head Count</h2>
      <input v-model="headCount" type="number" class="w-full border border-gray-300 rounded-md px-3 py-2 mb-4">
      <div class="flex justify-end">
        <button @click="submitForm" class="px-4 py-2 bg-green-500 text-white rounded-md">Submit</button>
        <button @click="closePopup" class="px-4 py-2 bg-gray-300 text-gray-700 rounded-md ml-2">Cancel</button>
      </div>
    </div>
  </div>
  <!-- PopUp -->

</template>


<script setup>
import { ref, onMounted } from 'vue';

const orderList = ref([])
const inputText = ref("")
const headCount = ref("")
const isPopupOpen = ref(false);
let totalCost = 0



// อ่านข้อมูลจาก localStorage เมื่อโหลด component เสร็จ
onMounted(() => {
  const storedOrders = JSON.parse(sessionStorage.getItem('orderList'))
  if (storedOrders) {
    orderList.value = storedOrders
  }

  scheduleClearSessionStorage()
})

const createdOrder = () => {
  let arrInput = inputText.value.split(",")
  let order = {
    name: arrInput[0],
    price: parseInt(arrInput[1])
  }
  totalCost += order.price
  addOrder(order)
}

const addOrder = (order) => {
  orderList.value.push(order)
  console.log(`addOrder: ${orderList.value} , totalCost = ${totalCost}`)
  // เมื่อเพิ่ม order เราจะบันทึก orderList ลงใน localStorage
  sessionStorage.setItem('orderList', JSON.stringify(orderList.value))
}

const deletedOrder = (item) => {
  console.log(item.name, item.price)
  if (item.price === NaN) {
    console.log(`Not a number`)
    // return
  }
  orderList.value = orderList.value.filter((order) => order !== item)
  totalCost -= item.price
  console.log(`deletedOrder: ${orderList.value}, totalCost = ${totalCost}`)
  // เมื่อลบ order เราจะบันทึก orderList ลงใน localStorage
  sessionStorage.setItem('orderList', JSON.stringify(orderList.value))
}

const clearOrder = () => {
  orderList.value = []
  totalCost = 0
  // เมื่อลบ order เราจะลบข้อมูลที่เก็บใน localStorage ด้วย
  sessionStorage.removeItem('orderList')
}

const openPopup = () => {
  isPopupOpen.value = true
  headCount.value = ""
}

const closePopup = () => {
  isPopupOpen.value = false
}

const submitForm = () => {
  if (headCount.value !== "" && parseInt(headCount.value) > 0) {
    closePopup()
    return setTimeout(() => {
      alert(`Average head count = ${totalCost / parseInt(headCount.value)}`)
      clearOrder()
    }, 100);
  }
  alert("wrong head count")
}

const scheduleClearSessionStorage = ()=> {
  const clearTime = 3600000  // 1 hr
  setTimeout(() => {
    clearOrder()
  }, clearTime)
}

</script>

