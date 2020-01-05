<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col sm="12" lg="12">
        <b-card no-body header-bg-variant="white">
          <b-card-header style="float: center; align-items: center; text-align:center;">
            <strong class="text-dark">{{ $ml.get('profile_email_information_title') }}</strong>
          </b-card-header>
          <b-card-body>
            <b-row>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  class="mb-3"
                  :label="$ml.get('profile_email')"
                >
                  <b-input-group>
                    <!-- <b-input-group-prepend>
                      <b-input-group-text>@</b-input-group-text>
                    </b-input-group-prepend> -->
                    <b-form-input
                      v-model="email"
                      type="text"
                      :readonly="!edit_click"
                      :placeholder="$ml.get('profile_enter_email')"
                    />
                  </b-input-group>
                </b-form-group>
              </b-col>
            </b-row>
            
            
            <b-button v-if="edit_click" @click="update_email" style="width:100px; float: right" block variant="primary">Update</b-button>
            <b-button v-if="edit_click" @click="edit_click = !edit_click" style="margin-right: 10px; width:100px; float: right" variant="warning">Cancel</b-button>

            <b-button v-if="!edit_click" @click="edit_click = !edit_click" style="width:100px; float: right" block variant="secondary">Edit</b-button>
          </b-card-body>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import store from '@/store'

export default {
  name: 'profile',
  components: {
  },
  data: () => {
    return { 
      
      email: '',

      edit_click: false,
      
    }
  },
  mounted () {
    // this.username = store.getters.username
    this.getEmail()
  },
  methods: {
    getEmail() {
      const self = this

      this.$api.apiRequest('', 'email', 'GET')
        .then((response) => {
          console.log(response)
          
          self.email = response.email
          self.notifice('success',response.msg)
        })
        .catch((error) => {
          self.notifice('error', this.$ml.get(error.error), ' ')
        })
    },
    update_email(){
      var self = this
      console.log(self.email)

      var objParams = {
        email: self.email,
      }

      this.$api.apiRequest(objParams, 'email', 'POST')
        .then((response) => {
          console.log(response)
          self.notifice('success',response.msg)
          self.edit_click = false
        })
        .catch((error) => {
          if(error.status == 'errorValidation') { 
            for (const [key, value] of Object.entries(error.error)) {
              var errMsg = '';
              for (var i = 0; i < value.length; i++) {
                value[i] = value[i].split(' ').join('_')
                value[i] = value[i].split('.').join('')
                value[i] = this.$ml.get(value[i])
                errMsg = errMsg + value[i] + '. '
              }
              self.notifice('error', errMsg, ' ')
            }
          } else {
            self.notifice('error', this.$ml.get(error.error), ' ')
          }
        })
    },
    notifice (type, title, message) {
      this.$notification[type]({
        message: title,
        description: message
      })
    },
  }
}
</script>
<style scoped>
.card-header {
    background-color: #ffffff;
}
</style>