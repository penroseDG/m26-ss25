<template>
  <div>
    <h1>list product</h1>
    <form @submit.prevent="handleSubmit">
      <div>
        <label for="">name</label>
        <input v-model="productInfo.name" type="text"/>
      </div>
      <div>
        <label for="">price</label>
        <input v-model="productInfo.price" type="number"/>
      </div>
      <div>
        <label for="">quantity</label>
        <input v-model="productInfo.quantity" type="number"/>
      </div>
    </form>
    <table border="1">
      <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>price</th>
          <th>quantity</th>
          <th>option</th>
        </tr>
      </thead>
      <tbody v-for="product in products" :key="product.id">
        <tr>
          <td>{{ product.id }}</td>
          <td>{{product.name}}</td>
          <td>{{product.price}}</td>
          <td>{{product.quantity}}</td>
          <td>
            <button>edit</button>
            <button>delete</button>

          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref } from "vue";

const handleSubmit=async()=>{
  // b1: validate du lieu 
  // b2: goi api them moi du lieu 
  const response= await fetch("http://localhost:8080/product",{
      method: "POST",
      headers : {
        "Content-Type" : "application/json",
      },
      body: JSON.stringify(productInfo),
  });

  // b3:reset form 
}

const products = ref([]);
const loadData = async () => {
  // fetch("http://localhost:8080/product")
  // .then((response) => response.json())
  // .then((data) => console.log(data))
  // .catch((error) => console.log(error));
  try {
    const response = await fetch("http://localhost:8080/product");

    const data = await response.json();

    return data;
  } catch (error) {
    return error;
  }
};
onMounted(async () => {
  const data = await loadData();
  products.value = data;
  console.log("data", data);
});
const productInfo= reactive({
  name:"",
  price:0,
  quantity:0,
})
</script>

<style></style>