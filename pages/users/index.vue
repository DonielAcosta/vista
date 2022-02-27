<template>
  <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      />
    </v-card-title>
    <v-data-table
      :page.sync="page"
      :loading="loading"
      :headers="headers"
      :items="users"
      :search="search"
      :items-per-page.sync="perPage"
      :server-items-length="total"
      :footer-props="{
        'items-per-page-options':[5,10,15,20]
      }"
    >
      <!-- <template v-slot:item.age="{ value }">
        <span v-if="value == true">
          <v-icon color="success">fa-check</v-icon>
        </span>
        <span v-else>
          <v-icon color="error">fa-times</v-icon>
        </span>
      </template> -->
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  name: 'UserList',
  data: () => ({
    users: [],
    loading: false,
    perPage: 10,
    page: 1,
    total: 0,
    search: '',
    headers: [
      {
        text: '#',
        align: 'start',
        filterable: true,
        value: 'id'
      },
      {
        text: 'Email',
        value: 'email'
      },
      { text: 'Name', value: 'user_data.name' },
      { text: 'Phone', value: 'user_data.phone' },
      { text: 'Identification', value: 'user_data.identification' },
      { text: 'Age', value: 'user_data.identification' },
      { text: 'Birthdate', value: 'user_data.date_of_birth' },
      { text: 'City', value: 'user_data.code_city' }
    ]
  }),
  watch: {
    perPage () {
      this.getUserData()
    },
    page () {
      this.getUserData()
    }
  },
  mounted () {
    this.getUserData()
  },
  methods: {
    getUserData () {
      this.$axios.get('users', {
        params: {
          page: this.page,
          size: this.perPage,
          search: this.search
        }
      })
        .then((res) => {
          const qry = res.data.data
          this.users = qry.data
          this.perPage = qry.per_page
          this.page = qry.page
          this.total = qry.total
          console.log('Los usuarios x => ', this.users)
        })
        .catch((err) => {
          console.log('Error en la peticion doniel => ', err)
        })
    }
  }
}
</script>
