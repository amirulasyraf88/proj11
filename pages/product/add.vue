<template>
  <v-layout justify-center>
    <v-flex xl10 md10 sm10 xs12>
      <v-card>
        <v-toolbar dark>
          <v-toolbar-title>Add New Product</v-toolbar-title>
          
        </v-toolbar>
        <v-container>
          <v-row class="mx-2">
            <v-col cols="12">Product Main Detail<v-divider></v-divider></v-col>
            <v-col cols="12">
              <v-text-field
                dense
                filled
                label="Item Name"
                placeholder="eg. Love Charm"
              />
            </v-col>
            <v-col cols="6">
              <v-text-field
                dense
                filled
                label="Factory Code"
                placeholder="FTAXXXXX"
              />
            </v-col>
            <v-col cols="6">
              <v-text-field
                dense
                filled
                label="Design Code"
                placeholder="GGXXXXX"
              />
            </v-col>

            <v-col cols="6">
              <v-select
                v-model="value"
                :items="parentcat"
                label="Parent Category"
                outlined
              ></v-select>
            </v-col>

            <v-col cols="6">
              <v-select
                v-model="value"
                :items="childcat"
                label="Child Category"
                outlined
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-autocomplete
                v-model="value"
                :items="Size"
                label="Size (Pre-order)"
                multiple
                chips
                outlined
              ></v-autocomplete>
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="value"
                :items="premium"
                label="Premium"
                outlined
                
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-text-field
                type="number"
                outlined
                label="Approximate Weight"
                value="8.88"
                suffix="/gram"
              />
            </v-col>
            <v-col cols="12">Pricing Strategy<v-divider></v-divider></v-col>
            <v-col cols="4">
              <v-text-field
                type="number"
                outlined
                label="Labor Charge (/gram)"
                value="10.00"
                prefix="RM"
                suffix="/gram"
              />
            </v-col>
            <v-col cols="4">
              <v-text-field
                type="number"
                outlined
                label="Labor Charge (Fixed)"
                value="10.00"
                prefix="RM"
                suffix="/pcs"
              />
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="value"
                :items="formula"
                label="Price Formula"
                outlined
              ></v-select>
            </v-col>
            <v-col cols="12">Misc. (Visibility) <v-divider></v-divider></v-col>
            <v-col cols="4">
              <v-select
                v-model="value"
                :items="cusvisibility"
                label="Customer Visibility"
                outlined
                chips
                multiple
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="value"
                :items="preordervisibility"
                label="Pre-Order Visibility"
                outlined
                multiple
                chips
              ></v-select>
            </v-col>
            <v-col cols="12">Detail Of Product<v-divider></v-divider></v-col>
            
            <v-col cols="12">
              <v-textarea
                auto-grow
                outlined
                label="Item Description"
                placeholder="Please elaborate more about the item"
              />
            </v-col>
            <v-col cols="2">
              <v-text-field
                type="number"
                outlined
                label="Height (Tinggi)"
                value="10.00"
                suffix="cm"
              />
            </v-col>
            <v-col cols="2">
              <v-text-field
                type="number"
                outlined
                label="Length (Panjang)"
                value="10.00"
                suffix="cm"
              />
            </v-col>
            <v-col cols="2">
              <v-text-field
                type="number"
                outlined
                label="Width (Lebar)"
                value="10.00"
                suffix="cm"
              />
            </v-col>
            <v-col cols="6">
              <v-select
                v-model="value"
                :items="items"
                label="Extra Type"
                multiple
                outlined
              ></v-select>
            </v-col>
            
            <v-col cols="12">Media Uploader<v-divider></v-divider></v-col>

            <v-col cols="12">
              <center>
                <v-btn dark large
                  >Upload Media <v-icon>mdi-upload</v-icon></v-btn
                >
              </center>
            </v-col>
            <v-col cols="12">
              <v-slide-group
                v-model="model"
                class="pa-0 ma-0"
                active-class="success"
                show-arrows
              >
                <v-slide-item
                  v-for="n in 10"
                  :key="n"
                  v-slot:default="{ active, toggle }"
                >
                  <v-card
                    :color="active ? undefined : 'grey lighten-1'"
                    class="ma-2"
                    height="200"
                    width="200"
                    @click="toggle"
                  >
                    <v-row class="fill-height" align="center" justify="center">
                      <v-scale-transition>
                        <v-icon
                          v-if="active"
                          color="white"
                          size="48"
                          v-text="'mdi-close-circle-outline'"
                        ></v-icon>
                      </v-scale-transition>
                    </v-row>
                  </v-card>
                </v-slide-item>
              </v-slide-group>
            </v-col>
          </v-row>
        </v-container>

        <v-card-actions>
          <v-btn text color="red">Clear Form <v-icon>mdi-broom</v-icon></v-btn>
          <v-spacer />
          <v-btn color="red" dark large
            >Cancel <v-icon>mdi-cancel</v-icon></v-btn
          >
          <v-btn color="amber" large>Draft</v-btn>
          <v-btn color="teal" dark large>Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  data: () => ({
    valid: true,
    name: '',
    nameRules: [
      (v) => !!v || 'Name is required',
      (v) => (v && v.length <= 10) || 'Name must be less than 10 characters',
    ],
    email: '',

    select: null,
    items: ['Enamel', 'Zirconia Stone', 'Pure Stone', 'Gold Plated', 'Plastic'],
    cusvisibility: ['SVIP', 'VVIP', 'USER', 'Wholesale', 'All'],
    preordervisibility: ['SVIP', 'VVIP', 'USER', 'Wholesale', 'All'],
    premium: ['No Premium', 'Black Label', 'White Label', 'Gold Plated', 'Swarovski'],
    parentcat: ['Bangle', 'Bracelet', 'Anklet', 'Rings', 'Earring', 'Pendant'],
    childcat: ['Gold Plated', 'Love Charm', 'Love Bracelet', 'Vacation Bracelet', 'Vacation Charm'],
    formula: ['Formula A', 'Formula B', 'Formula C', 'Formula D', 'Formula E'],
    Size: ['All Ring Size', 'All Bangle Size', 'All Anklet Size', 'S 14', 'S 17'],
    items2: [
      {
        text: 'Dashboard',
        disabled: false,
        href: '/',
      },
      {
        text: 'Product',
        disabled: false,
        href: 'product',
      },
      {
        text: 'Add',
        disabled: true,
        href: 'add',
      },
    ],
    checkbox: false,
  }),
  methods: {
    validate() {
      this.$refs.form.validate()
    },
    reset() {
      this.$refs.form.reset()
    },
    resetValidation() {
      this.$refs.form.resetValidation()
    },
  },
}
</script>