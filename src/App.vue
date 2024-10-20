<script setup>
import { ref, onMounted } from 'vue'

//頁籤預設第1個
const currentTab = ref(0);

//有三種狀態的頁籤
const tabs = ref([
  { title: '全部' },
  { title: '已完成' },
  { title: '未完成' },
]);

//項目初始值為0個
const list = ref([]);

//用來獲取新項目
const newItem = ref('');

//儲存本地資料
const saveItem = () => {
  localStorage.setItem('todolist', JSON.stringify(list.value));
  localStorage.setItem('selectAll', JSON.stringify(selectAll.value));
}

//取得本地資料
const getItem = () => {
  const getSaveItem = localStorage.getItem('todolist');
  const getSelectAll = localStorage.getItem('selectAll');
  if(getSaveItem){
    list.value = JSON.parse(getSaveItem);
  } else {
    list.value = [];
  }

  if (getSelectAll) {
    selectAll.value = JSON.parse(getSelectAll);
  } else {
    selectAll.value = false;
  }
}

//新增項目
const addNewItem = () => {
  if( newItem.value === '' ) return
  list.value.unshift({
    title:newItem.value,
    isFinish:false,
  })
  newItem.value = '';
  saveItem();
};

//刪除項目
const delItem = (index) => {
  list.value.splice(index,1);
  saveItem();
};

//全選預設為false
const selectAll = ref(false);

//全選開關
const toggleAll = (isCheck) => {
  list.value.forEach( item => {
    item.isFinish = isCheck
  })
  saveItem();
};


//瀏覽器加載完執行
onMounted(getItem);

</script>

<template>
  <main class="w-full p-5 pb-10 bg-white shadow-xl rounded-xl lg:rounded-3xl lg:p-10">
    <h1 class="text-2xl font-bold text-center mb-10 text-primary lg:text-4xl">To Do List</h1>
    <div class="flex gap-2 mb-10 lg:gap-5 lg:mb-14">
      <div class="flex-1"><input v-model="newItem" @keydown.enter="addNewItem()" type="text" aria-label="新增任務" class="w-full h-8 text-base border border-gray-300 rounded-md px-2 lg:text-lg lg:h-12 lg:px-5 lg:rounded-lg" placeholder="請輸入代辦事項名稱"></div>
      <div class="w-16 lg:w-24"><button @click="addNewItem()" class="w-full bg-black text-white text-base h-8 rounded-md lg:rounded-lg lg:text-lg lg:h-12"><i class="fa-regular fa-file-lines"></i> 新增</button></div>
    </div>

    <div class="mb-5 flex justify-end gap-4 lg:mb-10">
      <label class="text-sm lg:text-base"><input v-model="selectAll" @change="toggleAll(selectAll)" type="checkbox" aria-label="全選"> 全選</label>
    </div>

    <div class="mb-10 border-b-2 border-primary lg:mb-16">
      <ul class="flex">
        <li v-for="(tab,index) in tabs" class="flex-1"><button @click="currentTab = index" :class="{'tab-active' : currentTab === index}" class="tab">{{ tab.title }}</button></li>
      </ul>
    </div>
    <div class="space-y-8 lg:space-y-12">
      <div v-if="currentTab === 2 || currentTab === 0">
        <div class="flex items-center mb-5">
          <h2 class="flex items-center gap-2 text-[#333333] text-sm pr-4 font-bold lg:text-lg"><div class="w-4"><img class="w-full" src="../public/hourglass.png"></div> 未完成</h2>
          <div class="h-px bg-gray-200 flex-auto"></div>
        </div>
        <ul v-if="list.filter( item => !item.isFinish ).length > 0" class="space-y-2.5">
          <template v-for="(item,index) in list">
            <li v-if="item.isFinish === false" class="flex p-3.5 bg-[#f8f8f8] border-l-4 border-gray-300 lg:p-5">
              <div class="flex flex-1 items-center">
                <label class="flex gap-3 lg:gap-5">
                  <input type="checkbox" v-model="item.isFinish" @change="saveItem">
                  <span class="text-base lg:text-lg">{{ item.title }}</span>
                </label>
              </div>
              <div class="w-6 lg:w-8">
                <div @click="delItem(index)" class="w-full aspect-square rounded-full close-btns bg-red-600 cursor-pointer" aria-label="刪除任務"></div>
              </div>
            </li>
          </template>
        </ul>
        <div v-else class="w-full h-10 flex justify-center items-center text-gray-400 text-sm lg:h-16 lg:text-base">沒有代辦清單</div>
      </div>
  
      <div v-if="currentTab === 1 || currentTab === 0">
        <div class="flex items-center mb-5">
          <h2 class="flex items-center gap-2 text-[#333333] text-sm pr-4 font-bold lg:text-lg"><div class="w-4"><img class="w-full" src="../public/star.png"></div> 已完成</h2>
          <div class="h-px bg-gray-200 flex-auto"></div>
        </div>
        <ul v-if="list.filter( item => item.isFinish ).length > 0" class="space-y-2.5">
          <template v-for="(item,index) in list">
            <li v-if="item.isFinish === true" class="flex p-3.5 bg-[#f8f8f8] border-l-4 border-primary lg:p-5">
              <div class="flex flex-1 items-center">
                <label class="flex gap-3 lg:gap-5">
                  <input type="checkbox" v-model="item.isFinish" @change="saveItem">
                  <span class="text-base lg:text-lg">{{ item.title }}</span>
                </label>
              </div>
              <div class="w-6 lg:w-8">
                <div @click="delItem(index)" class="w-full aspect-square rounded-full close-btns bg-red-600 cursor-pointer" aria-label="刪除任務"></div>
              </div>
            </li>
          </template>
        </ul>
        <div v-else class="w-full h-10 flex justify-center items-center text-gray-400 text-sm lg:h-16 lg:text-base">沒有代辦清單</div>
      </div>

    </div>
  </main>
</template>

<style scoped>

</style>
