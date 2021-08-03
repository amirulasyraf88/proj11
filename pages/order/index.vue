<template>
<div>
  
  
  <v-data-table
    :headers="headers"
    :items="tbl_logistic"
    :search="search"
    sort-by="calories"
    class="elevation-1"
   
  >
    <template v-slot:top>
        <v-toolbar dark>
        <v-icon>mdi-receipt</v-icon>&nbsp;<v-toolbar-title>&nbsp;Pre Order</v-toolbar-title>
        
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
      <v-toolbar dark dense color="blue darken-3">
        <v-toolbar-items><v-btn text  dark class="mb-0" to="order/manual-order"><v-icon>mdi-plus</v-icon>Manual Order</v-btn></v-toolbar-items>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        
        <v-switch  class="pt-5 pr-5" label="SVIP"></v-switch>
        <v-switch class="pt-5 pr-5" label="VIP"></v-switch>
        
        <v-checkbox class="pt-5 pr-5" label="Processing"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="Pending Payment"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="Cancel"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="Complete"></v-checkbox>
        <v-checkbox class="pt-5 pr-5" label="Shipped"></v-checkbox>
      <v-dialog v-model="dialog" max-width="800px">
        
          <v-card>
            <v-card-title>
              {{ formTitle }}
            </v-card-title>
            <v-card-subtitle>To edit more you need to click the item and it will view the editing dashboard</v-card-subtitle>
  <v-divider></v-divider>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field outlined v-model="editedItem.ship_company" label="Shipping Company" ></v-text-field>
                  </v-col> 
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field outlined v-model="editedItem.ship_tracking" label="Tracking Number" ></v-text-field>
                  </v-col>  
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field outlined v-model="editedItem.ship_price" label="Shipping Price" type="number" prefix="RM"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red" dark  @click="close" large>Cancel<v-icon>mdi-cancel</v-icon></v-btn>
              <v-btn color="teal" dark @click="save" large>Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      
        
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
         
      >
        mdi-delete
      </v-icon>
      
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</div>
</template>
<script>
  export default {
    data: () => ({
      
      search: '',
      dialog: false,
      dialog2: false,
      headers: [
        {
          text: 'Order ID',
          align: 'start',
          sortable: false,
          value: 'order_numb',
        },
        { text: 'Date', value: 'order_date' },
        { text: 'Customer', value: 'customer_name' },
        
        { text: 'Membership', value: 'cust_membership' },
        { text: 'Total Order (RM)', sortable: false, value: 'ttl_inv',},
        { text: 'Total Gold  916(gram)', value: 'ttl_weight916' },
        { text: 'Total Gold  999(gram)', value: 'ttl_weight999' },
        { text: 'Item Quantity (pcs)', value: 'item_quantity' },
        { text: 'Status', value: 'order_status' },
        
        { text: 'Action', value: 'actions', sortable: false },
        
      ],
      tbl_logistic: [],
      editedIndex: -1,
      editedItem: {
        order_numb: '',
        customer_name: '',
        cust_membership: '',
        ttl_weight916: 0,
        ttl_weight999: 0,
        order_status:0,
        order_date:0,
        item_quantity:0,
      },
      defaultItem: {
        order_numb: '',
        customer_name: '',
        cust_membership: '',
        order_status:0,
        ttl_weight999: 0,
        order_date:0,
        ttl_weight916: 0,
       item_quantity:0,
      },
    }),
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Add Order' : 'Edit Order'
      },
    },
    watch: {
      dialog (val) {
        val || this.close()
      },
    },
    created () {
      this.initialize()
    },
    methods: {
      initialize () {
        this.tbl_logistic = [
          {
            order_numb: '#48849',
            ttl_weight916: 23.4,
            ttl_weight999: 23.4,
            ttl_inv: 20000.30,
            item_quantity:4,
            order_status: 'Pending Shipping',
            order_date: '12/2/2020',
            cust_membership: 'Special',
           
            customer_name:'Norazlan Nordin',
         
          },
          
          
        ]
      },
      editItem (item) {
        this.editedIndex = this.tbl_logistic.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
      deleteItem (item) {
        const index = this.tbl_logistic.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.tbl_logistic.splice(index, 1)
      },
      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },
      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.tbl_logistic[this.editedIndex], this.editedItem)
        } else {
          this.tbl_logistic.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>