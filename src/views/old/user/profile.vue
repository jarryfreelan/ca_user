<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col cols="12" sm="12" md="4">
        <b-button variant="light" style="border: 2px solid #d9e6fe;" block @click="$bvModal.show('profile-edit-modal')">{{ $ml.get('edit_profile') }}</b-button>
        <b-button variant="light" style="border: 2px solid #d9e6fe;" block @click="$bvModal.show('profile-password-modal')">{{ $ml.get('change_password') }}</b-button>
        <b-button variant="light" style="border: 2px solid #d9e6fe;" block to="/user/verification">{{ $ml.get('change_verify') }}</b-button>
        <b-dropdown variant="light" block style="width: 100%; padding: 10px 0 10px 0; border: 2px solid #d9e6fe;">
          <template v-slot:button-content>
            <span v-if="$ml.get('lang') == 'en'">
              <i class="flag-icon flag-icon-us fa-1x" title="EN" id="en" style="padding-right: 25px;"></i><span style="font-size: 15px;"> English</span>
            </span>
            <span v-if="$ml.get('lang') == 'cn'">
              <i class="flag-icon flag-icon-cn fa-1x" title="CN" id="cn" style="padding-right: 25px;"></i><span style="font-size: 15px;"> 中文</span>
            </span>
          </template>
          <b-dropdown-item @click = "switchLang('en')"><i class="flag-icon flag-icon-us" title="EN" id="en"></i> English</b-dropdown-item>
          <b-dropdown-item @click = "switchLang('cn')"><i class="flag-icon flag-icon-cn" title="CN" id="cn"></i> 中文</b-dropdown-item>
        </b-dropdown>
      </b-col>
      <b-col cols="12" sm="12" md="8">
        <b-card no-body>
          <b-card-header>
            <h4><li class="fa fa-user" style="padding-right: 10px;" /><strong>{{ $ml.get('my_profile') }}</strong></h4>
          </b-card-header>
          <b-card-body>
            <b-row>
              <b-col cols="2" sm="2" md="2" style="float: center; align-items: center; text-align:center;">
                <i class="fa fa-user fa-3x" />
              </b-col>
              <b-col cols="9" sm="9" md="9">
                <b-row>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px; font-color: #BEBEBE;">{{ $ml.get('username') }} :</span>
                  </b-col>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px;"><strong>{{ profile.username }}</strong></span>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col cols="2" sm="2" md="2" style="float: center; align-items: center; text-align:center;">
                <i class="fa fa-mobile-phone fa-3x" />
              </b-col>
              <b-col cols="9" sm="9" md="9">
                <b-row>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px; font-color: #BEBEBE;">{{ $ml.get('phone_number') }} :</span>
                  </b-col>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px;"><strong>{{ profile.phone }}</strong></span>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col cols="2" sm="2" md="2" style="float: center; align-items: center; text-align:center;">
                <i class="fa fa-envelope fa-2x" />
              </b-col>
              <b-col cols="9" sm="9" md="9">
                <b-row>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px; font-color: #BEBEBE;">{{ $ml.get('email') }} :</span>
                  </b-col>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px;"><strong>{{ profile.email }}</strong></span>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
            <hr>
            <b-row>
              <b-col cols="2" sm="2" md="2" style="float: center; align-items: center; text-align:center;">
                <i class="fa fa-map-pin fa-2x" />
              </b-col>
              <b-col cols="9" sm="9" md="9">
                <b-row>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px; font-color: #BEBEBE;">{{ $ml.get('country') }} :</span>
                  </b-col>
                  <b-col cols="12" sm="12" md="12">
                    <span style="font-size: 16px;"><strong>{{ profile.country }}</strong></span>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
          </b-card-body>
        </b-card>
      </b-col>
    </b-row>
    <b-modal id="profile-edit-modal" hide-footer>
      <template v-slot:modal-title>
        <h4><li class="fa fa-edit" style="padding-right: 10px;" /><strong>{{ $ml.get('profile') }}</strong></h4>
      </template>
      <div>
        <b-form @submit="onSubmit">
          <b-row>
            <b-col cols="12" sm="6" md="6" xl class="mb-3 mb-xl-0">
              <b-form-group
                id="username"
                :label="$ml.get('username')"
                label-for="username"
                :invalid-feedback="invalidFeedback.username"
                :state="state.username"
              >
                <b-form-input
                  id="username"
                  v-model="form.username"
                  type="text"
                  required
                  @input="checkUsername"
                />
              </b-form-group>
            </b-col>
            <b-col cols="12" sm="6" md="6" xl class="mb-3 mb-xl-0">
              <b-form-group
                id="email"
                :label="$ml.get('email')"
                label-for="email"
                :invalid-feedback="invalidFeedback.email"
                :state="state.email"
              >
                <b-form-input
                  id="email"
                  v-model="form.email"
                  type="email"
                  required
                  @input="checkEmail"
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="phone"
                :label="$ml.get('phone_number')"
                label-for="phone"
                :invalid-feedback="invalidFeedback.phone"
                :state="state.phone"
              >
                <vue-tel-input
                  class="form-control"
                  id="phone"
                  v-model="form.phone"
                  :valid-characters-only="true"
                  :placeholder="$ml.get('phone_number')"
                  style="padding: 3px 0 3px 0;"
                  @input="checkPhone"
                />
              </b-form-group>
              <b-button type="submit" block squared variant="success">{{ $ml.get('save_change') }}</b-button>
            </b-col>
          </b-row>
        </b-form>
      </div>
    </b-modal>
    <b-modal id="profile-password-modal" hide-footer>
      <template v-slot:modal-title>
        <h4><li class="fa fa-edit" style="padding-right: 10px;" /><strong>{{ $ml.get('change_password') }}</strong></h4>
      </template>
      <div>
        <b-form @submit="onSubmitPassword">
          <b-row>
            <b-col cols="12" sm="6" md="6" xl class="mb-3 mb-xl-0">
              <b-form-group
                id="oldPassword"
                :label="$ml.get('old_password')"
                label-for="username"
              >
                <b-form-input
                  id="oldpassword"
                  v-model="password.old"
                  type="password"
                  required
                  :placeholder="$ml.get('enter_old_password')"
                />
              </b-form-group>
            </b-col>
            <b-col cols="12" sm="6" md="6" xl class="mb-3 mb-xl-0">
              <b-form-group
                id="newPassword"
                :label="$ml.get('new_password')"
                label-for="email"
                :invalid-feedback="invalidFeedback.newPassword"
                :state="state.newPassword"
              >
                <b-form-input
                  id="newPassword"
                  v-model="password.new"
                  type="password"
                  required
                  :placeholder="$ml.get('enter_new_password')"
                  @input="checkPassword"
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="repeatPassword"
                :label="$ml.get('repeat_password_profile')"
                label-for="password"
                :invalid-feedback="invalidFeedback.repeatPassword"
                :state="state.repeatPassword"
              >
                <b-form-input
                  id="repeatPassword"
                  v-model="password.repeat"
                  type="password"
                  required
                  :placeholder="$ml.get('enter_repeat_password')"
                  @input="checkRepeatPassword"
                ></b-form-input>
              </b-form-group>
              <b-button type="submit" block squared variant="success">{{ $ml.get('save_change') }}</b-button>
            </b-col>
          </b-row>
        </b-form>
      </div>
    </b-modal>
  </div>
</template>

<script>
import store from '@/store'

export default {
  name: 'profile',
  data: () => {
    return { 
      togglePress: false,
      model_edit_profile: true,
      profile: {
        username: '',
        email: '',
        phone: '',
        country: '',
      },
      form: {
        username: '',
        email: '',
        phone: '',
        country: '',
      },
      invalidFeedback: {
        username: '',
        email: '',
        phone: '',
        oldPassword: '',
        newPassword: '',
        repeatPassword: '',
      },
      state: {
        username: true,
        email: true,
        phone: true,
        oldPassword: true,
        newPassword: true,
        repeatPassword: true,
      },
      password: {
        old: '',
        new: '',
        repeat: '',
      }
    }
  },
  mounted () {
    this.getProfile()
  },
  methods: {
    switchLang (lang) {
      this.$ml.change(lang)
      localStorage.setItem('lang', lang)
      this.$router.go()
    },
    getProfile() {
      this.form.username = store.getters.username
      this.form.email = store.getters.email
      this.form.phone = store.getters.phone
      this.form.country = store.getters.country
      this.profile.username = store.getters.username
      this.profile.email = store.getters.email
      this.profile.phone = store.getters.phone
      this.profile.country = store.getters.country
    },
    onSubmit (evt) {
      evt.preventDefault()
      const self = this
      if (this.state.username && this.state.email && this.state.phone) {
        var objParams = {
          e: 'updateProfile',
          form: this.form
        }
        this.$api.apiRequest(objParams)
          .then((response) => {
            console.log(response)
            if (response.status === 'SUCCESS') {
              store.commit('UPDATE_USER', JSON.stringify(response.user))
              self.getProfile()
              this.$bvModal.hide('profile-edit-modal')
              self.notifice('success', this.$ml.get('success_update_profile'), this.$ml.get('user_profile_updated'))
            } else {
              self.notifice('error', this.$ml.get('error_update_profile'), this.$ml.get('something_went_wrong_profile'))
            }
          })
          .catch((error) => {
            self.notifice('error', this.$ml.get('error_update_profile'), this.$ml.get('something_went_wrong_profile'))
          })
      } else {
        this.$message.error(this.$ml.get('validation_error'))
      }
    },
    onSubmitPassword (evt) {
      evt.preventDefault()
      const self = this
      if (this.state.newPassword && this.state.repeatPassword) {
        var objParams = {
          e: 'updatePass',
          password: this.password
        }
        this.$api.apiRequest(objParams)
          .then((response) => {
            console.log(response)
            if (response.status === 'SUCCESS') {
              this.$bvModal.hide('profile-password-modal')
              self.notifice('success', this.$ml.get('success_update_password'), this.$ml.get('user_password_changed'))
            } else {
              self.notifice('error', this.$ml.get('error_update_password'), this.$ml.get('something_went_wrong_password'))
            }
          })
          .catch((error) => {
            self.notifice('error', this.$ml.get('error_update_password'), this.$ml.get('something_went_wrong_old_password'))
          })
      } else {
        this.$message.error(this.$ml.get('validation_error'))
      }
    },
    notifice (type, title, message) {
      this.$notification[type]({
        message: title,
        description: message
      })
    },
    checkUsername (name) {
      if (name.length < 8) {
        this.invalidFeedback.username = this.$ml.get('invalid_username')
        this.state.username = false
      } else {
        this.invalidFeedback.username = ''
        this.state.username = true
      }

      if (this.state.username) {
        const self = this
        var objParams = {
          e: 'checkExistingUsername',
          username: name
        }
        this.$api.apiRequest(objParams)
          .then((response) => {
            if (response.status === 'SUCCESS') {
              self.state.username = true
              self.invalidFeedback.username = ''
            }
          })
          .catch((error) => {
            self.state.username = false
            self.invalidFeedback.username = this.$ml.get('invalid_existing_username')
          })
      }
    },
    checkEmail (email) {
      var re = /\S+@\S+\.\S+/

      if (!email.match(re)) {
        this.invalidFeedback.email = this.$ml.get('invalid_email')
        this.state.email = false
      } else {
        this.invalidFeedback.email = ''
        this.state.email = true
      }

      if(this.state.email) {
        const self = this
        var objParams = {
          e: 'checkExistingEmail',
          email: email
        }
        this.$api.apiRequest(objParams)
          .then((response) => {
            if (response.status === 'SUCCESS') {
              self.state.email = true
              self.invalidFeedback.email = ''
            }
          })
          .catch((error) => {
            self.state.email = false
            self.invalidFeedback.email = this.$ml.get('invalid_existing_email')
          })
      }  
    },
    checkPhone (formattedNumber, { number, valid, country }) {
      this.form.phone = number.international
      this.form.country = country.name
      if (!valid) {
        if (number.international) {
          this.invalidFeedback.phone = this.$ml.get('invalid_phone')
          this.state.phone = false
        } else {
          this.invalidFeedback.phone = ''
          this.state.phone = true
        }
      } else {
        this.invalidFeedback.phone = ''
        this.state.phone = true
      }
    },
    checkPassword (password) {
      var lowerCaseLetters = /[a-z]/g
      var upperCaseLetters = /[A-Z]/g
      var numbers = /[0-9]/g

      this.invalidFeedback.newPassword = '';

      if (password.length < 8) {
        this.invalidFeedback.newPassword += this.$ml.get('invalid_password')
        this.state.newPassword = false
      }
      if (!password.match(lowerCaseLetters)) {
        this.invalidFeedback.newPassword += this.$ml.get('invalid_password_lowercase')
        this.state.newPassword = false
      }
      if (!password.match(upperCaseLetters)) {
        this.invalidFeedback.newPassword += this.$ml.get('invalid_password_uppercase')
        this.state.newPassword = false
      }
      if (!password.match(numbers)) {
        this.invalidFeedback.newPassword += this.$ml.get('invalid_password_digit')
        this.state.newPassword = false
      } 
      if(password.length >= 8 && password.match(lowerCaseLetters) && password.match(upperCaseLetters) && password.match(numbers)){
        this.invalidFeedback.newPassword = ''
        this.state.newPassword = true
      }
      this.checkRepeatPassword(this.password.repeat)
    },
    checkRepeatPassword (repeatPassword) {
      if (repeatPassword !== this.password.new) {
        this.invalidFeedback.repeatPassword = this.$ml.get('invalid_password_match')
        this.state.repeatPassword = false
      } else {
        this.invalidFeedback.repeatPassword = ''
        this.state.repeatPassword = true
      }
    },
  }
}
</script>

<style scoped>
.vue-tel-input {
  border: 1px solid #e4e7ea;
}
</style>
