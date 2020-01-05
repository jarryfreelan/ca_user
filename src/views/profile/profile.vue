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
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_fullname')"
                >
                  <b-form-input
                    v-model="profile.fullname"
                    type="text"
                    :placeholder="$ml.get('profile_enter_fullname')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_phone')"
                >
                  <b-form-input
                    v-model="profile.phone"
                    type="text"
                    :placeholder="$ml.get('profile_enter_phone')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_gender')"
                >
                <select class="form-control" v-model="profile.gender" :readonly="!edit_click">
                  <option value="M">{{$ml.get('profile_gender_M')}}</option>
                  <option value="F">{{$ml.get('profile_gender_F')}}</option>
                </select>
                
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_birth')"
                >
                  <b-form-input
                    v-model="profile.birth"
                    type="text"
                    :placeholder="$ml.get('profile_enter_birth')"
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
                    v-model="profile.address1"
                    type="text"
                    :placeholder="$ml.get('profile_enter_address1')"
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
                    v-model="profile.address2"
                    type="text"
                    :placeholder="$ml.get('profile_enter_address2')"
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
                    v-model="profile.postcode"
                    type="text"
                    :placeholder="$ml.get('profile_enter_postcode')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_city')"
                >
                  <b-form-input
                    v-model="profile.city"
                    type="text"
                    :placeholder="$ml.get('profile_enter_city')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_state')"
                >
                  <b-form-input
                    v-model="profile.state"
                    type="text"
                    :placeholder="$ml.get('profile_enter_state')"
                    :readonly="!edit_click"
                  />
                </b-form-group>
              </b-col>
              <b-col cols="12" sm="6" md="6" xl class="mb-6 mb-xl-0">
                <b-form-group
                  :label="$ml.get('profile_country')"
                >
                  <b-form-input
                    v-model="profile.country"
                    type="text"
                    :placeholder="$ml.get('profile_enter_country')"
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
      profile: {
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
      },
      edit_click: false,
    }
  },
  mounted () {
    this.getProfile()
  },
  methods: {
    getProfile() {
      const self = this
      this.$api.apiRequest('', 'profile', 'GET')
        .then((response) => {
          console.log(response)
          self.profile = response.user
        })
        .catch((error) => {
          self.notifice('error', this.$ml.get(error.error), ' ')
        })
    },
    update_profile(){
      var self = this
      console.log(self.profile)
      var objParams = {
        form: self.profile
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