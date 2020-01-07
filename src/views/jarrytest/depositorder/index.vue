<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col sm="6" lg="6">
        <b-card no-body header-bg-variant="white">
          <b-card-header style="float: center; align-items: center; text-align:center;">
            <strong class="text-dark">Deposit</strong>
          </b-card-header>
          <b-card-body>
            <b-row>
              <b-col cols="6" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  class="mb-3"
                  label="Currency"
                >
                  <b-input-group>
                    <b-form-input
                      v-model="currency"
                      type="text"
                    />
                  </b-input-group>
                </b-form-group>
              </b-col>
            </b-row>
            <b-button @click="getAddressOrder" variant="primary">Submit</b-button>
          </b-card-body>
        </b-card>
      </b-col>
      <b-col sm="6" lg="6">
        <b-card no-body header-bg-variant="white">
          <b-card-header style="float: center; align-items: center; text-align:center;">
            <strong class="text-dark">Receive</strong>
          </b-card-header>
          <b-card-body>
            <b-row>
              <b-col cols="12" sm="12" md="12">
                <qrcode-vue :value="address" :size="size" level="H"></qrcode-vue>
              </b-col>
              <b-col cols="12" sm="12" md="12">
                <p>Address: {{ address }}</p>
              </b-col>
            </b-row>
          </b-card-body>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import QrcodeVue from 'qrcode.vue'

export default {
  name: 'depositOrder',
  components: {
    QrcodeVue,
  },
  data: () => {
    return { 
      address: '',
      size: 200,
      currency: '',
    }
  },
  methods: {
    getAddressOrder () {
      const self = this
      var objParams = {
        currency: self.currency
      }
      this.$api.apiRequest(objParams, 'deposit', 'POST')
        .then((response) => {
          console.log(response)
          self.address = response.depositOrder.order_address
        })
        .catch((error) => {
          self.notifice('error', this.$ml.get(error.error), ' ')
        })
    },
    notifice (type, title, message) {
      this.$notification[type]({
        message: title,
        description: message
      })
    }
  }
}
</script>
