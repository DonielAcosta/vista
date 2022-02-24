<template>
  <v-card
    class="mx-auto"
  >
    <!-- max-width="444" -->
    <v-form
      ref="form"
      v-model="valid"
      lazy-validation
    >
      <v-card-title>User Information</v-card-title>
      <v-card-text>
        <v-container class="">
          <v-row>
            <v-col xs="12" sm="12" md="6" lg="4">
              <ValidationProvider
                v-slot="{ errors }"
                vid="email"
                name="email"
                :rules="{
                  required: true,
                  min: 8
                }"
              >
                <v-text-field
                  v-model="email"
                  :rules="emailRules"
                  label="E-mail"
                  data-vv-as="email"
                  :error-messages="errors[0]"
                />
              </ValidationProvider>
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <v-text-field
                v-model="password"
                :counter="10"
                :rules="nameRules"
                label="Password"
                required
              />
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <v-text-field
                v-model="passwordVerify"
                :counter="10"
                :rules="emailRules"
                label="Verify Password"
                required
              />
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <v-text-field
                v-model="name"
                :counter="100"
                :rules="nameRules"
                label="Name"
                required
              />
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <v-text-field
                v-model="phone"
                :counter="10"
                :rules="emailRules"
                label="Phone Number"
                required
              />
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <v-text-field
                v-model="identification"
                :counter="11"
                :rules="nameRules"
                label="Identification"
                required
              />
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <div>
                <!-- <div class="mb-6">Active picker: <code>{{ activePicker || 'null' }}</code></div> -->
                <v-menu
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="birthday"
                      label="Birthday date"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    />
                  </template>
                  <v-date-picker
                    v-model="birthday"
                    :active-picker.sync="activePicker"
                    :max="(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)"
                    min="1950-01-01"
                    @change="save"
                  />
                </v-menu>
              </div>
            </v-col>
            <v-col xs="12" sm="12" md="6" lg="4">
              <v-text-field
                v-model="cityCode"
                :rules="nameRules"
                label="City Code"
                required
              />
            </v-col>
          </v-row>
        </v-container>
        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="validate"
        >
          Save
        </v-btn>
      </v-card-text>
    </v-form>
  </v-card>
</template>

<script>
export default {
  name: 'UserInfo',
  data: () => ({
    valid: true,
    email: '',
    password: '',
    passwordVerify: '',
    name: '',
    phone: '',
    identification: '',
    birthday: null,
    cityCode: '',
    activePicker: null,
    date: null,
    menu: false,
    nameRules: [
      v => !!v || 'Name is required',
      v => (v && v.length <= 10) || 'Name must be less than 10 characters'
    ],
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
    ],
    select: null,
    items: [
      'Item 1',
      'Item 2',
      'Item 3',
      'Item 4'
    ],
    checkbox: false
  }),
  watch: {
    menu (val) {
      val && setTimeout(() => (this.activePicker = 'YEAR'))
    }
  },
  methods: {
    validate () {
      this.$refs.form.validate()
    },
    reset () {
      this.$refs.form.reset()
    },
    resetValidation () {
      this.$refs.form.resetValidation()
    },
    save (birthday) {
      this.$refs.menu.save(birthday)
    }
  }
}
</script>

<style>

</style>
