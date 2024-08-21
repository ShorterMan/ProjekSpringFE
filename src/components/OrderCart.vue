<template>
  <div>
    <div>
      Customer: TomJerry
    </div>
    <div>
      Address: Jl. Tali 7 No.9, Jakarta Barat
    </div>
    <table>
      <tr>
        <th></th>
        <th>
          name
        </th>
        <th>
          type
        </th>
        <th>
          price
        </th>
        <th>
          qty
        </th>
        <th>
          total
        </th>
      </tr>
      <tr v-for="(data, i) in orderData" :key="i">
        <td>
          <input
            type="checkbox"
            :id="'checkboxData'.concat(i.toString())"
            :name="'checkboxData'.concat(i.toString())"
            v-model="checkedData"
            :value="i"
          />
        </td>
        <td>
          {{ data.name }}
        </td>
        <td>
          {{getProductTypeDescById(data.productType)}}
        </td>
        <td>
          {{ data.price }}
        </td>
        <td>
          {{ data.quantity }}
        </td>
        <td>
          {{ data.price * data.quantity }}
        </td>
      </tr>
    </table>

    <button @click="orderDataFunction">order</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      checkedData: [],
      orderData: [],
      typeOptions: [],
    };
  },
  async created(){
      await this.getTypeOptions();
  },
  methods: {
    addOrderData(data) {
      this.orderData.push(data);
    },
    async orderDataFunction() {
      let newOrderData = [];
      for (let index = 0; index < this.orderData.length; index++) {
        if (this.checkedData.includes(index))
          newOrderData.push(this.orderData[index]);
      }
      let response = await axios.post(
        process.env.VUE_APP_BACK_END_URL + "/api/products",
        newOrderData
      );
      console.log(response);
    },
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
    getProductTypeDescById(id){
        let data = this.typeOptions.filter((data) => data.id = id);
        return data[0].typedesc;
    }
  }
};
</script>

<style></style>
