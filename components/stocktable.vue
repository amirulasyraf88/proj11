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
        <v-toolbar dark dense>
          <v-text-field
            dense
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
            solo-inverted
          ></v-text-field>
        </v-toolbar>
        <v-toolbar flat color="teal" dark dense>
          <v-row class="pt-4 mt-0" lazy-validation>
            <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
              <v-autocomplete
                v-model="value"
                :items="designcode"
                label="Design Code"
                dense
                solo
                light
              ></v-autocomplete>
            </v-col>
            <v-col cols="2" sm="2" md="2" class="pl-2 ml-0 pr-0 mr-0">
              <v-text-field solo dense light label="Barcode"></v-text-field>
            </v-col>
            <v-col cols="2" class="pl-2 ml-0 pr-0 mr-0">
              <v-text-field
                solo
                dense
                light
                label="Weight"
                type="number"
              ></v-text-field>
            </v-col>

            <v-col cols="1" sm="1" md="1" class="pl-2 ml-0 pr-0 mr-0">
              <v-autocomplete
                v-model="value"
                :items="itemSize"
                label="Size"
                dense
                solo
                light
              ></v-autocomplete>
            </v-col>
            <v-col cols="2" class="pl-2 ml-0 pr-0 mr-0">
              <v-text-field
                solo
                dense
                light
                label="LabGram"
                type="number"
              ></v-text-field>
            </v-col>
            <v-col cols="2" class="pl-2 ml-0 pr-0 mr-0">
              <v-text-field
                solo
                dense
                light
                label="LabFixed"
                type="number"
              ></v-text-field>
            </v-col>
            <v-col cols="1" class="pl-2 ml-0 pr-0 mr-0">
              <v-autocomplete
                v-model="value"
                :items="premium"
                label="Premium"
                dense
                solo
                light
              ></v-autocomplete>
            </v-col>
          </v-row>

          <v-spacer/>
          <v-toolbar-items>
            <v-btn text><v-icon>mdi-plus</v-icon>Add Stock</v-btn>
          </v-toolbar-items>
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
  </div>
</template>
<script>
export default {
  data: () => ({
    search: '',
    designcode: ['GGC03020', 'GGB09494', 'GGM0202', 'GGJ38372', 'GGS9382'],
    itemSize: ['S 15', 'S 16', 'S 19', 'S 14', 'S 17'],
    premium: [
      'No Premium',
      'Black Label',
      'White Label',
      'Gold Plated',
      'Swarovski',
    ],
    itemLocation: ['Tray 1', 'Tray 2', 'Tray 3', 'Tray 4', 'Tray 5'],
    dialog: false,
    headers: [
      {
        text: 'Product Name',
        align: 'start',
        sortable: false,
        value: 'name',
      },
      { text: 'Design Code', value: 'design_code' },

      { text: 'Barcode', value: 'prod_barcode' },
      { text: 'Category', sortable: false, value: 'category_type' },
      { text: 'Weight (gram)', value: 'real_weight' },
      { text: 'Labor Fee (gram)', value: 'stock_labfee_1' },
      { text: 'Labor Fee (fixed)', value: 'stock_labfee_2' },
      { text: 'Premium Code', sortable: false, value: 'stock_prem_code' },
      { text: 'Location', sortable: false, value: 'location_item' },
      { text: 'Action', value: 'actions', sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      design_code: '',
      prod_barcode: '',
      real_weight: 0,
      stock_labfee_1: 0,
      stock_labfee_2: 0,
      stock_labfee_3: 0,
      stock_prem_code: '',
    },
    defaultItem: {
      name: '',
      design_code: '',
      prod_barcode: '',
      stock_labfee_1: 0,
      stock_labfee_2: 0,
      real_weight: 0,
      stock_labfee_3: 0,
      stock_prem_code: '',
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
          name: 'Emas Permata',
          real_weight: 2.35,
          category_type: 'Bangle',
          stock_prem_code: 'BL',

          stock_labfee_1: 17.26,
          stock_labfee_2: 17.26,
          prod_barcode: 'GC0453959',
          stock_labfee_3: 22.3,
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          real_weight: 2.35,
          category_type: 'Bangle',
          stock_prem_code: 'BL',

          stock_labfee_1: 17.26,
          stock_labfee_2: 17.26,
          prod_barcode: 'GC0453959',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          real_weight: 2.35,
          category_type: 'Bangle',
          stock_prem_code: 'BL',

          stock_labfee_1: 17.26,
          stock_labfee_2: 17.26,
          prod_barcode: 'GC0453959',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          real_weight: 2.35,
          category_type: 'Bangle',
          stock_prem_code: 'BL',

          stock_labfee_1: 17.26,
          stock_labfee_2: 17.26,
          prod_barcode: 'GC0453959',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Ayam',
          real_weight: 2.35,
          category_type: 'Bangle',
          stock_prem_code: 'BL',

          stock_labfee_1: 17.26,
          stock_labfee_2: 17.26,
          prod_barcode: 'GC0453959',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          real_weight: 2.35,
          category_type: 'Bangle',
          stock_prem_code: 'BL',

          stock_labfee_1: 17.26,
          stock_labfee_2: 17.26,
          prod_barcode: 'GC0453959',
          design_code: 'GGH92985',
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