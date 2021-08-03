<template>
<div>
  
  <v-data-table
    :headers="headers"
    :items="desserts"
    :search="search"
    sort-by="calories"
    class="elevation-1"
    
  >
    <template v-slot:top>
       
    
       <v-toolbar flat color="teal" dark dense>

          <v-row class="pt-4 mt-0">
                  <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
                    <v-text-field solo dense light label="Formula Name"></v-text-field>
                  </v-col>
                  <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
                    <v-text-field solo dense light label="SVIP" type="number"></v-text-field>
                  </v-col>
                  <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
                    <v-text-field solo dense light label="VIP" type="number"></v-text-field>
                  </v-col>
                  <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
                    <v-text-field solo dense light label="WH" type="number"></v-text-field>
                  </v-col>
                  <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
                    <v-text-field solo dense light label="USER" type="number"></v-text-field>
                  </v-col>
                  <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
                    <v-text-field solo dense light label="PURITY EXC" type="number"></v-text-field>
                  </v-col>
                  
        </v-row>
        <v-spacer></v-spacer>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-toolbar-items>
        <v-btn text><v-icon>mdi-plus</v-icon>Add Formula</v-btn>

        </v-toolbar-items>
        
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
      headers: [
        {
          text: 'Formula Name',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'SVIP', value: 'design_code' },
        
        { text: 'VIP', value: 'prod_barcode' },
        { text: 'WH', sortable: false, value: 'category_type',},
        { text: 'USER', value: 'real_weight' },
        { text: 'Purity Exchange', value: 'stock_labfee_1' },
        { text: 'Product Attach', value: 'stock_labfee_2' },
        
        { text: 'Action', value: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        design_code: '',
        prod_barcode: '',
        real_weight: 0,
        stock_labfee_1:0,
        stock_labfee_2:0,
        stock_labfee_3:0,
        stock_prem_code: '',
      },
      defaultItem: {
        name: '',
        design_code: '',
        prod_barcode: '',
        stock_labfee_1:0,
        stock_labfee_2:0,
        real_weight: 0,
        stock_labfee_3:0,
        stock_prem_code: '',
      },
    }),
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
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
        this.desserts = [
          {
            name: 'Formula A',
            real_weight: 233,
            category_type: '233',
            stock_prem_code: 'BL',
            
            stock_labfee_1: 930,
            stock_labfee_2: 23,
            prod_barcode: '244',
            stock_labfee_3: 244, 
            design_code:'243'
            
          },
          
          
          
          
          
          
        ]
      },
      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
      deleteItem (item) {
        const index = this.desserts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
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
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>