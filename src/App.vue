<template>
  <div id="app">
    <div class="w-1/2 mx-auto">
      <div class="flex justify-between">
        <div class="text-2xl"><h1>To-do List</h1></div>
        <div class="flex flex-row items-center text-xs">
          <input type="checkbox" class="mr-1" v-model="hide">
          <p class="text-blue-700">
            Gizlenenleri göster
          </p>
        </div>
      </div>
      <div class="flex flex-row justify-center ">
        <div>
          <input class="bg-blue-300 rounded-l-lg outline-none p-2 w-64" maxlength="24" v-model="newItemTitle" required>
        </div>
        <div>
          <button class="bg-blue-600 rounded-r-lg p-2 text-amber-200" @click="addNewItem">Ekle</button>
        </div>
      </div>
      <hr>
        <div class="flex flex-row justify-around px-3 py-2" v-for="list in listHidden" :key="list.lists">
          <div>{{list.task}}</div>
          <div><input type="checkbox" v-model="list.comp" ></div>
          <button class="bg-red-700 p-1.5 rounded text-amber-300" @click="deleteItem">Sil</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
    lists : [],
      hide: false,
      newItemTitle : ""
    }
  },
  computed: {
    listHidden() {
      return this.hide ? this.lists.filter(list => !list.comp) : this.lists
    }
  },
  methods: {
    addNewItem() {
      this.lists.push({ task: this.newItemTitle, comp: false});
      localStorage.setItem("todoTask", JSON.stringify(this.lists))
    },
    deleteItem() {
      this.lists = this.lists.filter(list => !list.comp);
      localStorage.setItem(`todoTask`, JSON.stringify(this.lists));
    }
  },
  created() {
    let data = localStorage.getItem("todoTask");
    if (data != null) {
      this.lists = JSON.parse(data);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Source Code Pro', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
  height: 100%;
}
</style>
