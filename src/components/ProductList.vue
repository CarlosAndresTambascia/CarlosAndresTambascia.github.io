<template>
  <div class="container">
    <b-container fluid>
      <navbar/>
      <b-table class="container" striped hover :items="products">
        <template slot="Delete" scope="row">
          <b-button size="sm" variant="danger" @click.stop="deleteItem(row.index)">Pago</b-button>
          <b-button size="sm" variant="secondary" @click.stop="editItem(row.item, row.index)">Editar</b-button>
        </template>
      </b-table>

      <!-- Product Update form -->
      <b-modal ref="myModalRef" hide-footer title="Modificar carga">
        <b-form>
          <!-- Product Name -->
          <b-row class="my-1">
            <b-col sm="3">
              <label>Facturas:</label>
            </b-col>
            <b-col sm="9">
              <b-form-select class="mb-3" v-model="form.facturas" :options="bills" required></b-form-select>
            </b-col>
          </b-row>
          <b-row class="my-1">
            <b-col sm="3">
              <label>Modo de Pago:</label>
            </b-col>
            <b-col sm="9">
              <b-form-select class="mb-3" v-model="form.pago" :options="paymentTypes" required></b-form-select>
            </b-col>
          </b-row>
        </b-form>
        <b-btn class="mt-3" variant="outline-success" block @click="hideModal()">Actualizar</b-btn>
      </b-modal>
    </b-container>
  </div>
</template>

<script>

  let products = [
    {},
  ];

  export default {
    data() {
      return {
        products: products,
        form: {
          facturas: null,
          pago: null
        },
        paymentTypes: [
          'Efectivo', 'Tarjeta'
        ],
        bills: [
          'Ninguno', '1', '2', '3', '4', '5'
        ],
        activateAnother: false
      }
    },
    methods: {
      deleteItem(index) {
        products.splice(index, 1);
      },
      editItem(data, index) {
        this.$refs.myModalRef.show();
        this.form = data;
      },
      hideModal() {
        this.$refs.myModalRef.hide();
      },
      calculateFlow(){
        let timeToWait;
        products.forEach(function (product) {
          debugger;
          if (product.facturas == "Ninguno") {
            timeToWait = timeToWait + 0;
          } else {
            timeToWait = parseInt(product.facturas) * 3;
          }
          if (product.pago == "Efectivo") {
            timeToWait = timeToWait + 2;
          } else if (product.pago == "Tarjeta") {
            timeToWait = timeToWait + 3;
          }
          console.log(timeToWait);
          if (timeToWait > 10) {
            debugger;
            this.activateAnother = true;
          }
          console.log(this.activateAnother);
        });
      }
    },
    created() {
      this.$bus.$on('NewProduct', (product) => {
        product.Delete = 'Delete';
        products.push(product);
        products = [...new Set(products.filter(p => p.facturas))];
        this.calculateFlow();
      });

    }
  }
</script>

<style>
  .container {
    width: 80%;
  }

  th {
    text-align: center;
  }
</style>
