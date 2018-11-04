<template>
  <div class="container">
    <b-container fluid>

      <!-- navbar-1.vue -->
      <navbar/>

      <!-- jumbotron -->
      <b-jumbotron header="Nuevo cliente" style="padding: 1rem"></b-jumbotron>
      <!-- Add New Product Form  -->
      <b-form @submit="onSubmit">
        <!-- Facturas -->
        <b-row class="my-1">
          <b-col sm="3">
            <label>Facturas:</label>
          </b-col>
          <b-col sm="9">
            <b-form-select class="mb-3" v-model="form.facturas" :options="bills" required></b-form-select>
          </b-col>
        </b-row>


        <!-- Modo de Pago -->
        <b-row class="my-1">
          <b-col sm="3">
            <label>Modo de Pago</label>
          </b-col>
          <b-col sm="9">
            <b-form-select class="mb-3" v-model="form.pago" :options="paymentTypes" required></b-form-select>
          </b-col>
        </b-row>

        <!-- Action Buttons -->
        <b-row class="my-1">
          <b-col sm="9">
          </b-col>
          <b-col sm="3">
            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button type="reset" variant="secondary" v-on:click="cancel">Reset</b-button>
          </b-col>
        </b-row>
      </b-form>
    </b-container>
  </div>
</template>
<script>

export default {
  data() {

    return {
      form: {
        facturas: null,
        pago: null
      },
      paymentTypes: [
        { text: 'Seleccionar', value: null },
        'Efectivo', 'Tarjeta'
      ],
      bills: [
        { text: 'Seleccionar', value: null },
        'Ninguno', '1', '2', '3', '4', '5'
      ]
    }
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault();
      this.$bus.$emit('NewProduct', this.form);
      window.location.replace('#/');
    },
    cancel: function() {
      this.form = null;
    }
  }
};
</script>

<style>
.container {
  width: 80%;
}
</style>
