<template>
  <div id="app">
    <div class="container w-auto h-auto">
      <h3 class=" md-5 text-2xl m-4">Product Categories</h3>
      <div>
        <button @click="addCategory"
          class="border border-slate-300 hover:border-indigo-300 bg-indigo-500 p-2 text-xs hover:bg-slate-300">Add
          Category</button>
      </div>
      <div v-for="(products, category) in jsonData" :key="category" class="category-wrapper">
        <ul @click="toggleProducts(category)"
          class="category bg-slate-600 w-auto flex justify-start gap-x-5 flex-row mt-1"
          :class="{ active: Category === category && showProducts }">
          <li class="cursor-pointer flex justify-start align-center text-xl p-2">
            <div class="mr-2">
              <i :class="Category === category && showProducts ? 'fas fa-minus' : 'fas fa-plus'"></i>
            </div>
            {{ category }}
          </li>
          <div class="flex space-x-2">
            <button @click="deleteCategory(category)"><i class="fas fa-trash"></i></button>
            <button @click="addProduct(category)"><i class="fas fa-plus"></i></button>
          </div>
        </ul>
        <div class="product" v-if="Category === category && showProducts">
          <div class="product-item " v-for="(product, index) in products" :key="product.id">
            <div @click="toggleOptions(category, index)"
              class="product-name cursor-pointer flex align-center ml-5 mt-1.5 border-b  gap-x-5 hover:text-slate-200 hover:bg-indigo-600">
              {{ product.name }}
              <div class="flex space-x-2">
                <button @click="deleteProduct(category, index)"><i class="fas fa-trash"></i></button>
                <button @click="addOption(category, index)"><i class="fas fa-plus"></i></button>
              </div>
            </div>
            <ul v-if="product.showOptions" class="product-options cursor-pointer flex justify-start flex-col">
              <li v-for="(option, optionIndex) in product.options" :key="optionIndex"
                class="hover:text-slate-200 hover:bg-indigo-600 flex mt-1 gap-x-8 ml-9 justify-start align-center">

                <div @click="toggleSubOptions(category, index, optionIndex)"
                  class="flex mt-1 gap-x-2 justify-center  relative align-center">
                  <div class="flex justify-center">
                    <div class="flex justify-center gap-x-2">
                      {{ displayOption(option) }}
                      <div>
                        <button @click="deleteSubOption(category, index, optionIndex, subOptionIndex)"><i
                            class="fas fa-trash"></i></button>
                        <button @click="addSubOption(category, index, optionIndex)"><i class="fas fa-plus"></i></button>
                      </div>


                    </div>
                  </div>

                </div>
                <div @click="toggleSubOptions(category, index, optionIndex)" v-if="typeof option === 'object'"
                  class="flex mt-8 justify-center">
                  <ul v-if="product.showSubOptions && Object.keys(option)[0] === product.selectedOption"
                    class="sub-options justify-center">
                    <li v-for="(subOption, subOptionIndex) in Object.values(option)[0]" :key="subOptionIndex"
                      class="hover:text-black bg-slate-300 hover:bg-yellow-300  flex mt-1 gap-x-4 ml-16 p-1 justify-centeralign-center">
                      <div class="flex justify-between">
                        <div class="flex justify-center ml-3">{{ subOption }}</div>
                        <div class="flex justify-center ml-2">
                          <button @click="deleteSubOption(category, index, optionIndex, subOptionIndex)"><i
                              class="fas fa-trash"></i></button>
                        </div>
                      </div>

                    </li>
                  </ul>
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
import { ref } from 'vue';

export default {
  name: 'App',
  computed: {
    displayOption() {
      return (option) => {
        if (typeof option === 'object') {
          return Object.keys(option)[0];
        } else {
          return option;
        }
      };
    }
  },
  setup() {
    const jsonData = ref({
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
    });

    const Category = ref(null);
    const showProducts = ref(false);
    const showSubOptions = ref(false);

    const addCategory = () => {
      const newCategoryName = prompt('Enter the name of the new category:');
      if (newCategoryName && !Object.prototype.hasOwnProperty.call(jsonData.value, newCategoryName)) {
        jsonData.value[newCategoryName] = [];
      }
    };

    const deleteCategory = (category) => {
      if (confirm(`Are you sure you want to delete the category '${category}'?`)) {
        delete jsonData.value[category];
        if (Category.value === category) {
          Category.value = null;
          showProducts.value = false;
        }
      }
    };

    const addProduct = (category) => {
      const newProductName = prompt('Enter the name of the new product:');
      if (newProductName) {
        const newProduct = {
          name: newProductName,
          options: [],
          showOptions: false,
          showSubOptions: false
        };
        jsonData.value[category].push(newProduct);
      }
    };

    const deleteProduct = (category, productIndex) => {
      if (confirm(`Are you sure you want to delete the product '${jsonData.value[category][productIndex].name}'?`)) {
        jsonData.value[category].splice(productIndex, 1);
      }
    };



    const toggleProducts = (category) => {
      if (Category.value === category && showProducts.value) {
        showProducts.value = false;
      } else {
        Category.value = category;
        showProducts.value = true;
      }
    };

    const toggleOptions = (category, productIndex) => {
      const product = jsonData.value[category][productIndex];
      product.showOptions = !product.showOptions;
      if (typeof product.options[0] === 'object') {
        product.showSubOptions = !product.showOptions;
      }
    };

    const toggleSubOptions = (category, productIndex, optionIndex) => {
      const product = jsonData.value[category][productIndex];
      const option = product.options[optionIndex];
      if (typeof option === 'object') {
        const selectedOption = product.selectedOption;
        if (selectedOption === Object.keys(option)[0]) {

          product.selectedOption = null;
        } else {

          product.selectedOption = Object.keys(option)[0];
        }
      }
    };




    const addOption = (category, productIndex) => {
      const newOption = prompt('Enter the name of the new option:');
      if (newOption) {
        jsonData.value[category][productIndex].options.push(newOption);
      }
    };

    const deleteOption = (category, productIndex, optionIndex) => {
      if (confirm(`Are you sure you want to delete the option '${jsonData.value[category][productIndex].options[optionIndex]}'?`)) {
        jsonData.value[category][productIndex].options.splice(optionIndex, 1);
      }
    };

    const addSubOption = (category, productIndex, optionIndex) => {
      const newSubOption = prompt('Enter the name of the new sub-option:');
      if (newSubOption) {
        const product = jsonData.value[category][productIndex];
        const option = product.options[optionIndex];

        if (typeof option === 'object') {
          const selectedOption = product.selectedOption;
          const subOptions = option[selectedOption];

          if (subOptions) {
            subOptions.push(newSubOption);
          } else {
            option[selectedOption] = [newSubOption];
          }
        } else {
          const existingOption = product.options[optionIndex];
          product.options[optionIndex] = { [existingOption]: [newSubOption] };
          product.showOptions = true;
          product.showSubOptions = true;
        }
      }
    };



    const deleteSubOption = (category, productIndex, optionIndex, subOptionIndex) => {
      if (confirm(`Are you sure you want to delete the sub-option '${jsonData.value[category][productIndex].options[optionIndex][Object.keys(jsonData.value[category][productIndex].options[optionIndex])[0]][subOptionIndex]}'?`)) {
        jsonData.value[category][productIndex].options[optionIndex][Object.keys(jsonData.value[category][productIndex].options[optionIndex])[0]].splice(subOptionIndex, 1);
      }
    };



    return {
      jsonData,
      Category,
      showProducts,
      showSubOptions,
      addCategory,
      deleteCategory,
      addProduct,
      deleteProduct,
      toggleProducts,
      toggleOptions,
      toggleSubOptions,
      addOption,
      deleteOption,
      addSubOption,
      deleteSubOption,

    };
  }
};
</script>


<style scoped>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}


.category {
  color: azure;
}

.category:hover {
  color: antiquewhite;
}

.product-name {
  background-color: rgb(217, 181, 181);
}
</style>
