<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col sm="8" lg="8">
        <b-row>
          <b-col sm="12" lg="12">
            <profile />
          </b-col>
        </b-row>
        <b-row>
          <b-col sm="12" lg="12">
            <email />
          </b-col>
        </b-row>
        <b-row>
          <b-col sm="12" lg="12">
            <password />
          </b-col>
        </b-row>
      </b-col>
      <b-col sm="4" lg="4">
        <!-- <account-info-card /> -->
      </b-col>
    </b-row>
  </div>
</template>

<script>
import Profile from './profile'
import Email from './email'
import Password from './password'

export default {
  name: 'dashboard',
  components: {
    Profile,
    Email,
    Password,
  },
  data: () => {
    return { 
    }
  }
}
</script>
