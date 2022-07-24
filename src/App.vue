<template>
  <div id="app">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css"
          integrity="sha512-KfkfwYDsLkIlwQp6LFnl8zNdLGxu9YAA1QvwINks4PhcElQSvqcyVLLD9aMhXd13uQjoXtEKNosOWaZqXgel0g=="
          crossorigin="anonymous" referrerpolicy="no-referrer"/>
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <div class="w-3/4 mx-auto p-4">
      <div class="flex justify-between text-xs lg:text-sm">
        <div class="text-lg lg:text-2xl"><h1>TODO-LİST</h1></div>
          <div class="flex flex-col sm:flex-row items-center">
          <button @click="itemHide">31</button>
        </div>
      </div>
      <div class="flex flex-row justify-between my-8 text-sm font-medium">
        <div class="p-1 text-blue-500 bg-blue-200">
          <h1>TOTAL : {{ lists.length }}</h1>
        </div>
        <div class="p-1 text-green-500 bg-green-200">
          <h1>SUCCESS : {{ completeHidden.length || 0 }}</h1>
        </div>
        <div class="p-1 text-red-500 bg-red-200">
          <h1>PENDING : {{ listHidden.length }}</h1>
        </div>
      </div>
      <div class="flex flex-row justify-center">
        <div class="w-full ">
          <input class="outline-none p-1 lg:p-2 w-5/6 border-b border-[#11cdef] focus:border-[#075d6e]"
                 maxlength="24"
                 v-model="newItemTitle"
                 placeholder="Buraya yazabilirsin..."
                 v-on:keydown.enter="addNewItem($event)"
                 required>
          <i
              class="fa fa-arrow-right submit-icon cursor-pointer text-lg"
              @click="addNewItem($event)"
              aria-hidden="true"
          ></i>
        </div>
      </div>
      <div
          class=" w-5/6 flex flex-col sm:flex-row justify-between py-2 my-2 mx-auto text-center items-center justify-center"
          v-for="(list, index) in listHidden" :key="index">
        <div>{{ list.task }}</div>
        <div class="w-40 flex flex-row items-center justify-between">
        <div class="text-xs mr-2">{{ list.inDate }}</div>
          <div class="flex flex-row">
          <button
              class="flex btn items-center justify-center"
              @click="completeItem(list)"
              :aria-label="list.comp ? 'Undone' : 'Done'"
              :title="list.comp ? 'Undone' : 'Done'">
            <i
                aria-hidden="true"
                class="material-icons"
            >{{ list.comp ? 'check_box' : 'check_box_outline_blank' }}</i>
          </button>
          <button class="flex ml-2 rounded btn"
                  aria-label="Delete"
                  title="Delete"
                  @click="deleteItem(list)">
            <i aria-hidden="true" class="material-icons">delete</i>
          </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: 'App',
  data() {
    return {
      lists: [],
      hide: true,
      newItemTitle: ""
    }
  },
  computed: {
    listHidden() {
      return this.hide ? this.lists.filter(list => !list.comp) : this.lists
    },
    completeHidden() {
      return this.lists.filter(list => list.comp === true)
    }
  },
  methods: {
    itemHide() {
      this.hide = !this.hide
    },
    addNewItem() {
      if (this.newItemTitle === null || this.newItemTitle === "") {
        alert("Bir şey yazmalısın!")
      } else {
        this.lists.push({
          task: this.newItemTitle,
          inDate: moment().locale('tr').format("MMM D"),
          comp: false
        });
        localStorage.setItem("todoTask", JSON.stringify(this.lists))
        this.newItemTitle = ""
        console.log(this.lists)
      }
    },
    completeItem(completedItem) {
      completedItem.comp = !completedItem.comp
      localStorage.setItem("todoTask", JSON.stringify(this.lists))
    },
    deleteItem(deletedItem) {
      this.lists = this.lists.filter(list => list !== deletedItem)
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

#app .btn {
  border: none;
  background: none;
  -webkit-appearance: none;
  cursor: pointer;
  color: #11cdef;
}
</style>
