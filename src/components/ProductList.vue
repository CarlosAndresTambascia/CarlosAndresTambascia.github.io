<template>
  <div class="container">
      <navbar/>
      <div class="scrolleable">
        <div :show="activateAnother">
          <b-alert :show="dismissCountDown"
                   dismissible
                   variant="warning"
                   @dismissed="dismissCountDown=0"
                   @dismiss-count-down="countDownChanged"
                   >
            <p>Se recomienda abrir otra caja. La alerta desaparecera en {{dismissCountDown}} segundos...</p>
            <b-progress variant="warning"
                        :max="dismissSecs"
                        :value="dismissCountDown"
                        height="4px">
            </b-progress>
          </b-alert>
        </div>
        <b-table class="container" striped hover :items="getProduct">
          <template slot="Delete" scope="row">
            <b-button size="sm" variant="danger" @click.stop="deleteItem(row.index)">Pago</b-button>
            <b-button size="sm" variant="secondary" @click.stop="editItem(row.item, row.index)">Editar</b-button>
          </template>
        </b-table>
      </div>
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
        openNew: false,
        activateAnother: false,
        dismissCountDown: 0,
        dismissSecs: 3
      }
    },
    methods: {
      showAlert() {
        this.dismissCountDown = this.dismissSecs
      },
      countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown
      },
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
      calculateFlow(products) {
        let timeToWait = 0;
        const vm = this;
        vm.products.map(function (product) {
          //debugger;
          if (product.facturas == "Ninguno") {
            timeToWait = timeToWait + 0;
          } else {
            timeToWait = timeToWait + parseInt(product.facturas) * 3;
          }
          if (product.pago == "Efectivo") {
            timeToWait = timeToWait + 2;
          } else if (product.pago == "Tarjeta") {
            timeToWait = timeToWait + 3;
          }
          if (timeToWait > 10 && products) {
            vm.activateAnother = true;
            vm.showAlert();
          }
        });
      }
    },
    computed: {
      getProduct() {
        this.calculateFlow(this.products);
        return this.products;
      }
    },
    created() {
      this.$bus.$on('NewProduct', (product) => {
        product.Delete = 'Delete';
        products.push(product);
        products = [...new Set(products.filter(p => p.facturas))];
      });
    },
    updated() {
      this.calculateFlow()
    }
  }
</script>

<style>
  .container {
    background: #fff;
    border-radius: 4px;
    height: 80vh;
    padding: 0;
    width: 40%;
  }

  table.container {
    height: calc(100% - 66px);
    margin: 0;
    width: 100%;
  }

  .scrolleable{
    overflow: auto;
    max-height: calc(100% - 66px);
  }

  th {
    text-align: center;
  }
</style>
