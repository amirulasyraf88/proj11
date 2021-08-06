<template>
  <v-layout justify-center>
    <v-flex xl8 md10 sm10 xs12>
      <v-card>
        <v-toolbar dark>
          <v-toolbar-title>Gold Variable</v-toolbar-title>
          <v-spacer />
          <v-spacer />
          <v-spacer />
          <v-spacer />
        </v-toolbar>

        <v-container>
          <v-row class="mx-2">
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
            <v-col cols="12"
              >Master Product Detail<v-divider></v-divider
            ></v-col>
            <v-col cols="12">
              <v-text-field label="Product Name" dense filled></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field label="Design Code" dense filled></v-text-field>
            </v-col>
            <v-col cols="2">
              <v-text-field label="Factory Code" dense filled></v-text-field>
            </v-col>
            <v-col cols="2">
              <v-autocomplete
                v-model="value"
                :items="cat_type"
                filled
                label="Type"
              ></v-autocomplete>
            </v-col>
            <v-col cols="2">
              <v-text-field label="Approx. Weight" dense filled></v-text-field>
            </v-col>
            <v-col cols="2">
              <v-autocomplete
                v-model="value"
                :items="itmpurity"
                filled
                label="Purity"
              ></v-autocomplete>
            </v-col>
            <v-col cols="4">
              <v-text-field
                label="Labour Price (gram)"
                dense
                filled
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                label="Labour Price (fixed)"
                dense
                filled
              ></v-text-field>
            </v-col>
            <v-col cols="2">
              <v-autocomplete
                v-model="value"
                :items="preorderstatus"
                filled
                label="Pre-order status"
              ></v-autocomplete>
            </v-col>
            <v-col cols="2">
              <v-autocomplete
                v-model="value"
                :items="priceformula"
                filled
                label="Price To Follow"
              ></v-autocomplete>
            </v-col>
            <v-col cols="12">Detail Of Product<v-divider></v-divider></v-col>
            <v-col cols="12">
              <v-textarea
                auto-grow
                filled
                label="Item Description"
                placeholder="Please elaborate more about the item"
              />
            </v-col>
            <v-col cols="12">Customer Visibility<v-divider></v-divider></v-col>
            <v-col cols="1">
              <v-checkbox
                class="mt-0"
                v-model="ex4"
                label="SVIP"
                color="red"
                value="red"
                hide-details
              ></v-checkbox>
            </v-col>
            <v-col cols="1">
              <v-checkbox
                class="mt-0"
                v-model="ex4"
                label="VIP"
                color="red"
                value="red"
                hide-details
              ></v-checkbox>
            </v-col>
            <v-col cols="1">
              <v-checkbox
                class="mt-0"
                v-model="ex4"
                label="WH"
                color="red"
                value="red"
                hide-details
              ></v-checkbox>
            </v-col>
            <v-col cols="1">
              <v-checkbox
                class="mt-0"
                v-model="ex4"
                label="User"
                color="red"
                value="red"
                hide-details
              ></v-checkbox>
            </v-col>
          </v-row>
          <v-row class="mx-2">
            <v-col cols="12">Stock Entry<v-divider></v-divider></v-col>

            <v-col cols="12">
              <stocktable />
            </v-col>
          </v-row>
        </v-container>

        <v-card-actions>
          <v-spacer />

          <v-btn color="teal" dark large>Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
import stocktable from '~/components/stocktable.vue'
export default {
  components: { stocktable },
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    menu: false,
    modal: false,
    menu2: false,
    valid: true,

    name: '',
    nameRules: [
      (v) => !!v || 'Name is required',
      (v) => (v && v.length <= 10) || 'Name must be less than 10 characters',
    ],
    email: '',

    select: null,
    items: ['Enamel', 'Zirconia Stone', 'Pure Stone', 'Gold Plated', 'Plastic'],
    cat_type: [
      'Bangle',
      'Bracelet',
      'Ring',
      'Anklet',
      'Necklace',
      'Dangle',
      'Bead',
      'Pendant',
      'Others',
    ],
    itmpurity: ['916', '999.9', '999', 'Gold Plated'],
    preorderstatus: ['Pre-Order', 'Cant Pre-Order (Limited)'],
    priceformula: ['Formula D', 'Formula C', 'Formula B', 'Formula A'],
    checkbox: false,
  }),
}
</script>