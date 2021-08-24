<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :search="search"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar dark>
        <v-icon>mdi-account-group</v-icon>&nbsp;<v-toolbar-title
          >Track Record</v-toolbar-title
        >

        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
          solo-inverted
        ></v-text-field>
      </v-toolbar>
      <v-toolbar dark dense color="brown darken-1">
        <v-spacer></v-spacer>
        <v-switch class="pt-5 pr-5" label="Active"></v-switch>
        <v-switch class="pt-5 pr-5" label="Terminated"></v-switch>
        <v-checkbox class="pt-5 pr-5" label="SVIP"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="Wholesale"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="VIP"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="User"></v-checkbox>
        <v-dialog v-model="dialog" max-width="800px">
          <v-card>
            <v-card-title>
              {{ formTitle }}
            </v-card-title>
            <v-card-subtitle
              >To edit more you need to click the item and it will view the
              editing dashboard</v-card-subtitle
            >
            <v-divider></v-divider>
            <v-card-text>
              <v-container>
                <v-row>
                  
                  <v-col cols="12" sm="4" md="4">
                    <v-autocomplete
                      outlined
                      v-model="editedItem.cust_status"
                      label="Customer Status"
                      :items="custStatus"
                   
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="4" md="4">
                    <v-autocomplete
                      outlined
                      v-model="editedItem.cust_tier"
                      label="Customer Tier"
                      :items="custTier"
                   
                    ></v-autocomplete>
                  </v-col>
                  
                  
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red" dark @click="close" large
                >Cancel<v-icon>mdi-cancel</v-icon></v-btn
              >
              <v-btn color="teal" dark @click="save" large>Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>
<script>
export default {
  data: () => ({
    search: '',
    custStatus: ['Active', 'Terminated', 'On-Hold'],
    custTier: ['SVIP', 'VIP', 'Wholesale', 'User'],
    dialog: false,
    headers: [
      {
        text: 'ID',
        align: 'start',
        sortable: false,
        value: 'cust_id',
      },
      { text: 'Customer Name', value: 'cust_name' },
      { text: 'Tier', value: 'cust_tier' },
      { text: 'SVIP', value: 'cust_svip' },
      { text: 'This Month', value: 'this_month' },
      { text: 'Last Month', value: 'last_month' },
      { text: '1st Quarter', value: 'q1_salegram' },
      { text: '2nd Quarter', value: 'q2_salegram' },
      { text: '3rd Quarter', value: 'q3_salegram' },
      { text: '4th Quarter', value: 'q4_salegram' },
      { text: 'Current Year', value: 'year_salegram' },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      cust_id: '',
      cust_name: '',
      cust_tier: '',
    
    },
    defaultItem: {
      cust_id: '',
      cust_name: '',
      cust_tier: '',
     
    },
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    },
  },
  watch: {
    dialog(val) {
      val || this.close()
    },
  },
  created() {
    this.initialize()
  },
  methods: {
    initialize() {
      this.desserts = [
        {
          cust_id: 'GGCSS928',
          cust_name: 'Amirul Asyraf',
          cust_tier: 'VIP',
          cust_svip:'Dhyia',
          this_month: 23.3,
          last_month: 23.2,
          q1_salegram:23.3,
          q2_salegram:23.3,
          q3_salegram:23.3,
          q4_salegram:23.3,
          year_salegram:23.3
        },
        {
          cust_id: 'GGCSS928',
          cust_name: 'Amirul Asyraf',
          cust_tier: 'VIP',
          cust_svip:'Dhyia',
          this_month: 23.3,
          last_month: 23.2,
          q1_salegram:23.3,
          q2_salegram:23.3,
          q3_salegram:23.3,
          q4_salegram:23.3,
          year_salegram:23.3
        },
        {
          cust_id: 'GGCSS928',
          cust_name: 'Amirul Asyraf',
          cust_tier: 'VIP',
          cust_svip:'Dhyia',
          this_month: 23.3,
          last_month: 23.2,
          q1_salegram:23.3,
          q2_salegram:23.3,
          q3_salegram:23.3,
          q4_salegram:23.3,
          year_salegram:23.3
        },
      ]
    },
    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },
    deleteItem(item) {
      const index = this.desserts.indexOf(item)
      confirm('Are you sure you want to delete this item?') &&
        this.desserts.splice(index, 1)
    },
    close() {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>