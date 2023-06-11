<template>
  <div id="app">
    <div class="container w-auto h-auto">
      <h3 class="category md-5 text-2xl m-4">Product Categories</h3>
      <div v-for="(products, category) in jsonData" :key="category" class="category-wrapper">
        <ul @click="toggleProducts(category)" class="category bg-slate-600 w-auto" :class="{ active: Category === category && showProducts }">
          <li class="cursor-pointer flex justify-start align-center hover:bg-slate-50 text-xl p-2 border-b">
            <div class="mr-2">
              <i :class="Category === category && showProducts ? 'fas fa-minus' : 'fas fa-plus'"></i>
            </div>
            {{ category }}
          </li>
        </ul>
        <div class="product" v-if="Category === category && showProducts">
          <div class="product-item bg-white" v-for="product in products" :key="product.id">
            <div @click="toggleOptions(product)" class="product-name cursor-pointer justify-center flex align-center border-b hover:text-slate-200 hover:bg-indigo-600">
              {{ product.name }}
            </div>
            <ul  v-if="product.showOptions" class="product-options cursor-pointer">
              <li   v-for="option in product.options" :key="option" class="hover:text-slate-200 hover:bg-indigo-600 flex justify-center border-b align-center">
                <div @click="toggleSubOptions(product)" v-if="typeof option === 'object'">
                  <span>{{ Object.keys(option)[0] }}</span>
                  <ul @click="toggleSubOptions(product)" v-if="product.showSubOptions && Object.keys(option)[0] === 'Option 1'" class="sub-options">
                    <li   v-for="subOption in Object.values(option)[0]" :key="subOption" class="hover:text-black hover:bg-yellow-300 flex justify-center border-b align-center">
                      <div class="flex justify-center align-center">{{ subOption }}</div>
                    </li>
                  </ul>
                </div>
                <div v-else>
                  {{ option }}
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      jsonData: {
        "Monitor": [
          {
            "id": 1,
            "name": "Dell",
            "options": ["monitor 24inch", "monitor 18inch ", "monitor 22inch"]
          },
          {
            "id": 2,
            "name": "Apple",
            "options": ["Apple 24inch monitor", "Apple 18 inch monitor", "Apple 32 inch monitor"]
          },
          {
            "id": 3,
            "name": "LG",
            "options": ["LG 32inch", "LG 20 inch", "LG 22inch"]
          }
        ],
        "Server": [
          {
            "id": 1,
            "name": "Server 1",
            "options": [{ "Option 1": ["Option 1.1", "Option 1.2"] }, "Option 2", "Option 3"]
          },
          {
            "id": 2,
            "name": "Server 2",
            "options": [{ "Option 1": ["Option 1.1", "Option 1.2", "Option 1.3"] }, "Option 2", "Option 3"]
          },
          {
            "id": 3,
            "name": "Server 3",
            "options": [{ "Option 1": ["Option 1.1", "Option 1.2", "Option 1.3"] }, "Option 2", "Option 3"]
          }
        ],
        "Printing": [
          {
            "id": 1,
            "name": "Product 1",
            "options": ["Option 1", "Option 2", "Option 3"]
          },
          {
            "id": 2,
            "name": "Product 2",
            "options": ["Option 1", "Option 2", "Option 3"]
          },
          {
            "id": 3,
            "name": "Product 3",
            "options": ["Option 1", "Option 2", "Option 3"]
          }
        ],
        "Computer": [
          {
            "id": 1,
            "name": "Core i7",
            "options": ["Option 1", "Option 2", "Option 3"]
          },
          {
            "id": 2,
            "name": "Computer i5",
            "options": ["Option 1", "Option 2", "Option 3"]
          },
          {
            "id": 3,
            "name": "Computer i9",
            "options": ["Option 1", "Option 2", "Option 3"]
          }
        ]
      },
      Category: null,
      showProducts: false,
      showSubOptions: false
    };
  },
  methods: {
    toggleProducts(category) {
      if (this.Category === category && this.showProducts) {
        this.showProducts = false;
      } else {
        this.Category = category;
        this.showProducts = true;
      }
    },
    toggleOptions(product) {
      
      product.showOptions = !product.showOptions;
      if (typeof product.options[0] === 'object') {
        product.showSubOptions = !product.showOptions;
      }
    },
    toggleSubOptions(product) {
      
      product.subOption = !product.subOptions;
      if (typeof product.options[0] === 'object') {
        product.showSubOptions = !product.showSubOptions;
      }
    }
  }
};



</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  background-color: aquamarine;
}

.category {
  color: azure;
}

.category:hover {
  color: black;
}

.product-name {
  background-color: rgb(217, 181, 181);
}</style>