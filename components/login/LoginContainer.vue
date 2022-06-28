<template>
  <v-row no-gutters>
    <v-col cols="12" md="4" sm="4" class="sidebar-blue">
      <v-row class="mx-2 " align="center" justify="center">
        <v-img src="/earist_logo.png" max-width="280px"></v-img>
        <!-- <v-col>
          <span style="color: #f7b918">
            <b class="ssd-logo-text">Solutions<br />Security<br />Dedication</b></span
          >
        </v-col> -->
      </v-row>
      <v-row class="flex-column">
        <v-col class="welcome mb-6">
          <p style="font-family: OpenSans; margin-bottom: 0px;">Welcome to</p>
          <h1 style="font-family: OpenSans-Bold">EARIST</h1>
        </v-col>
        <v-col>
          <v-form
            ref="form"
            v-model="valid"
            lazy-validation
            class="mx-10"
            @submit.prevent="submitHandler"
          >
          <div class="mb-2 white--text">
            Enter ID
          </div>
            <!-- <label class="white--text">Enter ID</label> -->
            <v-text-field
              solo
              flat
              v-model="login.username"

            ></v-text-field>
            <div class="mb-2 white--text">
              Password
            </div>
            <!-- <label class="white--text">Password</label> -->
            <v-text-field
              solo
              flat
              @click:append="password_reveal=!password_reveal" :append-icon="!password_reveal ? 'mdi-eye' : 'mdi-eye-off'"
              :type="!password_reveal ? 'password' : null"
              v-model="login.password"
        
              
            ></v-text-field>
            <v-container class="d-flex justify-center">
              <v-btn
                color="#F9CC56"
                type="submit"
                rounded depressed
                width="150"
                @click="signIn"
                x-large
                :disabled="!valid"
                :loading="btnLoading"
              >
                <span >
                  Sign In
                </span> 
              </v-btn>
            </v-container>
          </v-form>
        </v-col>
      </v-row>
    </v-col>
    <v-col cols="12" md="8" sm="8">
      <v-img
        src="/earist_bg.png"
        alt="parked-trucks"
        height="100vh"
        width="auto"
      ></v-img>
    </v-col>
  </v-row>
</template>

<script>
export default {
  head() {
    return {
      title: "Login"
    };
  },
  layout: 'plain',
  middleware: 'isLoggedIn',
  data() {
    return {
      password_reveal:false,
      valid: false,
      btnLoading: false,
      login: {
        username: '',
        password: '',
      },
    }
  },
  methods: {
    signIn(){
       if(this.login.username!='admin@earist.com' && this.login.password!='wew123WEW'){
        alert("Wrong Credentials")
        return
      }
      window.location.href="/directory"
    },
    async submitHandler() {
      return
      const valid = this.$refs.form.validate()
      if (valid) {
        try {
          this.btnLoading = true
          this.$store.dispatch('snackbar/setLoadingOverlay', true)
          const response = await this.$auth.loginWith('local', {
            data: this.login,
          })
          // console.log(response);
          const { access } = response.data
          // this.$router.push('/')
          this.btnLoading = false
          this.$refs.form.reset()
          this.$store.dispatch('snackbar/setLoadingOverlay', false)
          // location.reload();
        } catch (error) {
          this.$store.dispatch('snackbar/setLoadingOverlay', false)
          this.btnLoading = false
        }
      }
    },
  },
}
</script>

<style>
.ssd-logo-text {
  font-family: Montserrat-SemiBold;
}

.text-container {
  padding-top: 24px;
}

.welcome {
  /* margin: 150px auto; */
  /* text-transform: uppercase; */
  color: #ffffff;
  text-align: center;
}

.sign-in {
  text-transform: capitalize;
  letter-spacing: normal;
  font-family: OpenSans-Bold !important;
  color: white !important;
}

.v-btn:hover .sign-in {
  color: black !important;
}

.v-btn.sign-in-btn:hover {
  background-color: #F9CC56 !important;
}

.sidebar-blue {
  background-color: #080B48;
}

</style>
