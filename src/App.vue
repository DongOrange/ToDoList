<script setup>
import { ref } from 'vue'

const currentTab = ref(0);
const tabs = ref([
  { title: '全部' },
  { title: '已完成' },
  { title: '未完成' },
]);

const list = ref([
  { title: '倒垃圾', isFinish: false },
  { title: '擦窗戶', isFinish: false },
]);

const newItem = ref('');

const addNewItem = () => {
  if( newItem.value === '' ) return
  list.value.unshift({
    title:newItem.value,
    isFinish:false,
  })
  newItem.value = ''
};

const delItem = (index) => {
  list.value.splice(index,1)
};

</script>

<template>
  <main class="w-full p-5 pb-10 bg-white shadow-xl rounded-xl lg:rounded-3xl lg:p-10">
    <h1 class="text-2xl font-bold text-center mb-10 text-primary lg:text-4xl">To Do List</h1>
    <div class="flex gap-2 mb-10 lg:gap-5 lg:mb-20">
      <div class="flex-1"><input v-model="newItem" @keydown.enter="addNewItem()" type="text" aria-label="新增任務" class="w-full h-8 text-lg border border-gray-300 rounded-md px-2 lg:h-12 lg:px-5 lg:rounded-lg"></div>
      <div class="w-14 lg:w-24"><button @click="addNewItem()" class="w-full bg-primary text-white text-base h-8 rounded-md lg:rounded-lg lg:text-lg lg:h-12">新增</button></div>
    </div>

    <div class="mb-5 flex justify-end gap-4 lg:mb-10">
      <label class="text-sm lg:text-base"><input type="checkbox" aria-label="全選"> 全選</label>
    </div>

    <div class="mb-10 border-b-2 border-primary lg:mb-20">
      <ul class="flex">
        <li v-for="(tab,index) in tabs" class="flex-1"><button @click="currentTab = index" :class="{'tab-active' : currentTab === index}" class="tab">{{ tab.title }}</button></li>
      </ul>
    </div>

    <div>
      <div v-if="currentTab === 2 || currentTab === 0" class="mb-10">
        <h2 class="w-fit bg-gray-100 text-sm py-1 px-4 rounded-full text-gray-400 lg:text-base">未完成</h2>
        <ul>
          <template v-for="(item,index) in list">
            <li v-if="item.isFinish === false" class="border-b border-gray-300 px-0 py-5 flex lg:p-5">
              <div class="flex flex-1 items-center">
                <label class="flex gap-3 lg:gap-5">
                  <input type="checkbox" v-model="item.isFinish">
                  <span class="text-lg">{{ item.title }}</span>
                </label>
              </div>
              <div class="w-6 lg:w-8">
                <div @click="delItem(index)" class="w-full aspect-square rounded-full close-btns bg-red-600 cursor-pointer" aria-label="刪除任務"></div>
              </div>
            </li>
          </template>
        </ul>
      </div>
  
      <div v-if="currentTab === 1 || currentTab === 0" class="mb-10">
        <h2 class="w-fit bg-primary text-sm py-1 px-4 rounded-full text-white lg:text-base">已完成</h2>
        <ul>
          <template v-for="(item,index) in list">
            <li v-if="item.isFinish === true" class="border-b border-gray-300 px-0 py-5 flex lg:p-5">
              <div class="flex flex-1 items-center">
                <label class="flex gap-3 lg:gap-5">
                  <input type="checkbox" v-model="item.isFinish">
                  <span class="text-lg">{{ item.title }}</span>
                </label>
              </div>
              <div class="w-6 lg:w-8">
                <div @click="delItem(index)" class="w-full aspect-square rounded-full close-btns bg-red-600 cursor-pointer" aria-label="刪除任務"></div>
              </div>
            </li>
          </template>
        </ul>
      </div>
    </div>
  </main>
</template>

<style scoped>

</style>
