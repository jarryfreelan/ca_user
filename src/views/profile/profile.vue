<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col sm="12" lg="12">
        <b-card no-body header-bg-variant="white">
          <b-card-header style="float: center; align-items: center; text-align:center;">
            <strong class="text-dark">{{ $ml.get('profile_information_title') }}</strong>
          </b-card-header>
          <b-card-body>
            <b-row>
              <b-col cols="6" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_fullname')"
                >
                  <b-form-input
                    v-model="fullname"
                    type="text"
                    :placeholder="$ml.get('enter_fullname')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="6" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_phone')"
                >
                  <b-form-input
                    v-model="phone"
                    type="text"
                    :placeholder="$ml.get('enter_phone')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="6" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_gender')"
                >
                <select class="form-control" v-model="gender" :readonly="!edit_click">
                  <option value="M">{{$ml.get('profile_gender_M')}}</option>
                  <option value="F">{{$ml.get('profile_gender_F')}}</option>
                </select>
                
                </b-form-group>
              </b-col>
              <b-col cols="6" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_birth')"
                >
                  <b-form-input
                    v-model="birth"
                    type="text"
                    :placeholder="$ml.get('enter_birth')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
            </b-row>
            
            <b-row>
              <b-col cols="12" sm="12" md="12" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_address1')"
                >
                  <b-form-input
                    v-model="address1"
                    type="text"
                    :placeholder="$ml.get('enter_address1')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col cols="12" sm="12" md="12" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_address2')"
                >
                  <b-form-input
                    v-model="address2"
                    type="text"
                    :placeholder="$ml.get('enter_address2')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_postcode')"
                >
                  <b-form-input
                    v-model="postcode"
                    type="text"
                    :placeholder="$ml.get('enter_postcode')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_city')"
                >
                  <b-form-input
                    v-model="city"
                    type="text"
                    :placeholder="$ml.get('enter_city')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_state')"
                >
                  <b-form-input
                    v-model="state"
                    type="text"
                    :placeholder="$ml.get('enter_state')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_country')"
                >
                  <b-form-input
                    v-model="country"
                    type="text"
                    :placeholder="$ml.get('enter_country')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
            </b-row>
            
            
            <b-button v-if="edit_click" @click="update_profile" style="width:100px; float: right" block variant="primary">Update</b-button>
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
      
      fullname: '',
      gender: '',
      phone: '',
      birth: '',
      address1: '',
      address2: '',
      postcode: '',
      city: '',
      state: '',
      country: '',

      edit_click: false,
      
    }
  },
  mounted () {
    // this.username = store.getters.username
    this.getProfile()
  },
  methods: {
    getProfile() {
      const self = this

      this.$api.apiRequest('', 'profile', 'GET')
        .then((response) => {
          console.log(response)
          
          self.fullname = response.user.fullname
          self.gender = response.user.gender
          self.phone = response.user.phone
          self.birth = response.user.birth
          self.address1 = response.user.address1
          self.address2 = response.user.address2
          self.postcode = response.user.postcode
          self.city = response.user.city
          self.state = response.user.state
          self.country = response.user.country
          self.notifice('success',response.msg)
        })
        .catch((error) => {
          self.notifice('error', this.$ml.get(error.error), ' ')
        })
    },
    update_profile(){
      var self = this
      console.log(self.fullname)
      console.log(self.gender)
      console.log(self.phone)
      console.log(self.birth)
      console.log(self.address1)
      console.log(self.address2)
      console.log(self.postcode)
      console.log(self.city)
      console.log(self.state)
      console.log(self.country)

      var objParams = {
        fullname: self.fullname,
        gender: self.gender,
        phone: self.phone,
        birth: self.birth,
        address1: self.address1,
        address2: self.address2,
        postcode: self.postcode,
        city: self.city,
        state: self.state,
        country: self.country
      }

      this.$api.apiRequest(objParams, 'profile', 'POST')
        .then((response) => {
          console.log(response)
          self.edit_click = false
          self.notifice('success',response.msg)
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
    },
  }
}
</script>
<style scoped>
.card-header {
    background-color: #ffffff;
}
</style>