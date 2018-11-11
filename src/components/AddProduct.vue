<template>
  <div class="container">
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
      <b-row class="my-1 button-group">
        <b-button type="reset" variant="secondary" v-on:click="cancel">Reset</b-button>
        <b-button type="submit" variant="primary">Submit</b-button>
      </b-row>
    </b-form>
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
          {text: 'Seleccionar', value: null},
          'Efectivo', 'Tarjeta'
        ],
        bills: [
          {text: 'Seleccionar', value: null},
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
      cancel: function () {
        this.form = null;
      }
    }
  };
</script>

<style scoped>
  .container {
    background: #fff;
    border-radius: 4px;
    height: 80vh;
    padding: 0;
    width: 40%;
  }
  .row.button-group{
    display: flex;
    justify-content: space-evenly;
  }

  form {
    padding: 0px 55px 0px 50px;
  }
</style>
