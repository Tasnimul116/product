<template>
  <div id="app">
    <div class="container  w-auto h-auto">
      <h3 class="catagory md-5 text-2xl m-4">Product Categories</h3>
      <div v-for="(products, category) in jsonData" :key="category" class="category-wrapper">
        <ul @click="toggleProducts(category)" class="category  bg-slate-600 w-auto  " :class="{ active: selectedCategory === category }">
         <li class="cursor-pointer flex justify-start hover:bg-slate-50 text-xl p-2 border-b">
          {{ category }}
        </li> 
        </ul>
        <div class="product" v-if="selectedCategory === category && showProducts">
          <div class="product-item bg-white" v-for="product in products" :key="product.id">
            <div @click="toggleOptions(product)" class="product-name cursor-pointer justify-center flex align-center border-b hover:text-slate-200 hover:bg-indigo-600">
              {{ product.name }}
            </div>
            <ul v-if="product.showOptions" class="product-options cursor-pointer" >
              <li v-for="option in product.options" :key="option" class="hover:text-slate-200 hover:bg-indigo-600  flex justify-center border-b align-center " >
                {{ option }}
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
            "options": ["Dell 24inch monitor", "Dell 18 inch monitor", "Dell 32 inch monitor"]

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
            "options": ["Option 1", "Option 2", "Option 3"]

          },
          {
            "id": 2,
            "name": "Server 2",
            "options": ["Option 1", "Option 2", "Option 3"]

          },
          {
            "id": 3,
            "name": "Server 3",
            "options": ["Option 1", "Option 2", "Option 3"]

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



      selectedCategory: null,
      showProducts: false
    };
  },
  methods: {
    toggleProducts(category) {
      if (this.selectedCategory === category) {
        this.showProducts = !this.showProducts;
      } else {
        this.selectedCategory = category;
        this.showProducts = true;
      }
    }, toggleOptions(product) {
      product.showOptions = !product.showOptions;
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

.container{
  background-color: aquamarine;
}

.category{
  color: azure;
}

.category:hover{
  color: black;
}
.product-name{
  background-color: rgb(202, 201, 200);
}
</style>