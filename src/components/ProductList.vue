<template>
  <div>
    <div>
      Name:
    </div>
    <input type="text" v-model="name" />
    <div>
      type:
    </div>
    <select v-model="selectedType">
      <option :value="null">Silahkan Pilih Type</option>
      <option v-for="data in typeOptions" :key="data.id" :value="data.id">{{
        data.typedesc
      }}</option>
    </select>
    <div>
      price:
    </div>
    <div>Rp<input type="text" v-model="price" /></div>

    <div>
      Quantity:
    </div>
    <input type="text" v-model="quantity" />

    <button @click="addToCart()">
      Add to Cart
    </button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      typeOptions: [
        {
          id: "1",
          typedesc: "test"
        }
      ],
      selectedType: null,
      price: 0,
      quantity: 0,
      error: ""
    };
  },
  async created() {
    this.getTypeOptions();
  },
  methods: {
    async getTypeOptions() {
      console.log(process.env.VUE_APP_BACK_END_URL);
      let response = await axios.get(
        process.env.VUE_APP_BACK_END_URL + "/api/producttypes",
        {
          params: {
            page: 0,
            size: 20
          }
        }
      );
      const productTypes = response.data.content;
      this.typeOptions = productTypes.map(({ id, typedesc }) => ({ id, typedesc }));
    },
    addToCart(){
      if(this.name && this.selectedType && this.price > 0 && this.quantity > 0){
        console.log(this.selectedType)
        let orderData = {
          name: this.name,
          productType: this.selectedType,
          price: this.price,
          quantity: this.quantity
        }
        console.log(orderData)
        this.$emit("order", orderData);
      }else
        this.error = "MOHON LENGKAPI SEMUA DATA";
    }
  }
};
</script>

<style></style>
