<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="tbl_product"
      :search="search"
      sort-by="calories"
      class="elevation-1"
      show-select
    >
      <template v-slot:top>
        <v-toolbar dark>
          <v-icon>mdi-atom</v-icon>&nbsp;<v-toolbar-title
            >Product Manager</v-toolbar-title
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
        <v-toolbar color="primary" dense flat dark>
          <v-toolbar-items
            ><v-btn text class="mb-2" to="/product/add"
              ><v-icon>mdi-plus</v-icon> Add</v-btn
            ></v-toolbar-items
          >
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>

          <v-switch class="pt-5 pr-5" label="Normal 916"></v-switch>
          <v-switch class="pt-5 pr-5" label="Heavy Weight"></v-switch>
          <v-switch class="pt-5 pr-5" label="White Label"></v-switch>
          <v-switch class="pt-5 pr-5" label="Black Label"></v-switch>

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
                    <v-col cols="12" sm="12" md="12">
                      <v-text-field
                        filled
                        v-model="editedItem.name"
                        label="Item Name"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        filled
                        v-model="editedItem.design_code"
                        label="Design Code"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        filled
                        v-model="editedItem.fac_code"
                        label="Factory Code"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        outlined
                        v-model="editedItem.weight_appox"
                        label="Weight Approximate"
                        suffix="gram"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        outlined
                        v-model="editedItem.labfee_1"
                        label="Labor Fee (gram)"
                        prefix="RM"
                        suffix="/gram"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        outlined
                        v-model="editedItem.labfee_2"
                        label="Labor Fee (fixed)"
                        prefix="RM"
                        suffix="/pcs"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        outlined
                        v-model="editedItem.labfee_3"
                        label="Extra Fee (fixed)"
                        prefix="RM"
                        suffix="/pcs"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="4" md="4">
                      <v-text-field
                        outlined
                        v-model="editedItem.prem_code"
                        label="Premium Code"
                      ></v-text-field>
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
  </div>
</template>
<script>
export default {
  data: () => ({
    search: '',
    dialog: false,
    headers: [
      {
        text: 'Product Name',
        align: 'start',
        sortable: false,
        value: 'name',
      },
      { text: 'Design Code', value: 'design_code' },

      { text: 'Factory Code', value: 'fac_code' },
      { text: 'Category', sortable: false, value: 'category_type' },
      { text: 'Weight Appox', value: 'weight_appox' },
      { text: 'Labor Fee (gram)', value: 'labfee_1' },
      { text: 'Labor Fee (fixed)', value: 'labfee_2' },
      { text: 'Stock Availibility', value: 's_avail' },
      { text: 'Premium Code', sortable: false, value: 'prem_code' },
      { text: 'Action', value: 'actions', sortable: false },
    ],
    tbl_product: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      design_code: '',
      fac_code: '',
      weight_appox: 0,
      labfee_1: 0,
      labfee_2: 0,
      labfee_3: 0,
      prem_code: '',
    },
    defaultItem: {
      name: '',
      design_code: '',
      fac_code: '',
      labfee_1: 0,
      labfee_2: 0,
      weight_appox: 0,
      labfee_3: 0,
      prem_code: '',
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
      this.tbl_product = [
        {
          name: 'Emas Permata',
          weight_appox: 2.35,
          category_type: 'Bangle',
          prem_code: 'BL',
          s_avail: 26,
          labfee_1: 17.26,
          labfee_2: 17.26,
          fac_code: 'RK9384',
          labfee_3: 22.3,
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          weight_appox: 2.35,
          category_type: 'Bangle',
          prem_code: 'BL',
          s_avail: 26,
          labfee_1: 17.26,
          labfee_2: 17.26,
          fac_code: 'RK9384',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          weight_appox: 2.35,
          category_type: 'Bangle',
          prem_code: 'BL',
          s_avail: 26,
          labfee_1: 17.26,
          labfee_2: 17.26,
          fac_code: 'RK9384',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          weight_appox: 2.35,
          category_type: 'Bangle',
          prem_code: 'BL',
          s_avail: 26,
          labfee_1: 17.26,
          labfee_2: 17.26,
          fac_code: 'RK9384',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Ayam',
          weight_appox: 2.35,
          category_type: 'Bangle',
          prem_code: 'BL',
          s_avail: 26,
          labfee_1: 17.26,
          labfee_2: 17.26,
          fac_code: 'RK9384',
          design_code: 'GGH92983',
        },
        {
          name: 'Emas Permata',
          weight_appox: 2.35,
          category_type: 'Bangle',
          prem_code: 'BL',
          s_avail: 26,
          labfee_1: 17.26,
          labfee_2: 17.26,
          fac_code: 'RK9384',
          design_code: 'GGH92985',
        },
      ]
    },
    editItem(item) {
      this.editedIndex = this.tbl_product.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },
    deleteItem(item) {
      const index = this.tbl_product.indexOf(item)
      confirm('Are you sure you want to delete this item?') &&
        this.tbl_product.splice(index, 1)
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
        Object.assign(this.tbl_product[this.editedIndex], this.editedItem)
      } else {
        this.tbl_product.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>