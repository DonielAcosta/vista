<template>
  <v-card
    class="mx-auto"
  >
    <ValidationObserver v-slot="{ handleSubmit }" ref="form">
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
                    min: 8,
                    email: true
                  }"
                >
                  <v-text-field
                    v-model="form.email"
                    label="E-mail"
                    data-vv-as="email"
                    :error-messages="errors[0]"
                  />
                </ValidationProvider>
              </v-col>
              <v-col xs="12" sm="12" md="6" lg="4">
                <ValidationProvider
                  v-slot="{ errors }"
                  vid="password"
                  name="password"
                  :rules="{
                    required: true,
                    min: 8
                  }"
                >
                  <v-text-field
                    v-model="form.password"
                    autocomplete="off"
                    label="Password"
                    :error-messages="errors[0]"
                    :type="showPass ? 'text' : 'password'"
                    :append-icon="showPass ? 'mdi-eye-off' : 'mdi-eye'"
                    @click:append="showPass = !showPass"
                  />
                </ValidationProvider>
              </v-col>
              <v-col xs="12" sm="12" md="6" lg="4">
                <ValidationProvider
                  v-slot="{ errors }"
                  name="passwordVerify"
                  :rules="{
                    required: true,
                    min: 8,
                    confirmed: 'password'
                  }"
                >
                  <v-text-field
                    v-model="form.passwordVerify"
                    label="Verify Password"
                    :error-messages="errors[0]"
                    :type="showPass ? 'text' : 'password'"
                    :append-icon="showPass ? 'mdi-eye-off' : 'mdi-eye'"
                    @click:append="showPass = !showPass"
                  />
                </ValidationProvider>
              </v-col>
              <v-col xs="12" sm="12" md="6" lg="4">
                <ValidationProvider
                  v-slot="{ errors }"
                  vid="name"
                  name="name"
                  :rules="{
                    required: true,
                    max: 100
                  }"
                >
                  <v-text-field
                    v-model="form.name"
                    :counter="100"
                    label="Name"
                    :error-messages="errors[0]"
                  />
                </ValidationProvider>
              </v-col>
              <v-col xs="12" sm="12" md="6" lg="4">
                <ValidationProvider
                  v-slot="{ errors }"
                  vid="phone"
                  name="phone"
                  :rules="{
                    required: false,
                    max: 10,
                    min: 10
                  }"
                >
                  <v-text-field
                    v-model="form.phone"
                    :counter="10"
                    label="Phone Number"
                    :error-messages="errors[0]"
                  />
                </ValidationProvider>
              </v-col>
              <v-col xs="12" sm="12" md="6" lg="4">
                <ValidationProvider
                  v-slot="{ errors }"
                  vid="identification"
                  name="Identification"
                  :rules="{
                    required: true,
                    max: 11
                  }"
                >
                  <v-text-field
                    v-model="form.identification"
                    :counter="11"
                    label="Identification"
                    :error-messages="errors[0]"
                  />
                </ValidationProvider>
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
                        v-model="form.birthday"
                        label="Birthday date"
                        prepend-icon="mdi-calendar"
                        readonly
                        required
                        v-bind="attrs"
                        v-on="on"
                      />
                    </template>
                    <v-date-picker
                      v-model="form.birthday"
                      :active-picker.sync="activePicker"
                      :max="getMaxDate()"
                      min="1900-01-01"
                      @change="save"
                    />
                  </v-menu>
                </div>
              </v-col>
              <v-col xs="12" sm="12" md="6" lg="4">
                <ValidationProvider
                  v-slot="{ errors }"
                  vid="cityCode"
                  name="cityCode"
                  :rules="{
                    required: true,
                    max: 11
                  }"
                >
                  <v-text-field
                    v-model="form.cityCode"
                    label="City Code"
                    :error-messages="errors[0]"
                  />
                </ValidationProvider>
              </v-col>
            </v-row>
          </v-container>
          <v-btn
            :disabled="!valid"
            color="success"
            class="mr-4"
            @click="handleSubmit(submit)"
          >
            Save
          </v-btn>
        </v-card-text>
      </v-form>
    </ValidationObserver>
  </v-card>
</template>

<script>
export default {
  name: 'UserInfo',
  data: () => ({
    showPass: false,
    valid: true,
    form: {
      email: '',
      password: '',
      passwordVerify: '',
      name: '',
      phone: '',
      identification: '',
      birthday: null,
      cityCode: ''
    },
    activePicker: null,
    date: null,
    menu: false,
    select: null
  }),
  watch: {
    menu (val) {
      val && setTimeout(() => (this.activePicker = 'YEAR'))
    }
  },
  methods: {
    async submit () {
      // await this.$axios.post('', )
    },
    reset () {
      this.$refs.form.reset()
    },
    resetValidation () {
      this.$refs.form.resetValidation()
    },
    save (birthday) {
      this.$refs.menu.save(birthday)
    },
    getMaxDate () {
      const date = new Date()
      return new Date(date.setFullYear(new Date().getFullYear() - 18)).toISOString().substr(0, 10)
    }
  }
}
</script>

<style>

</style>
