<template>
  <v-layout justify-center>
    <v-flex xl8 md10 sm10 xs12>
      <v-card>
        <v-toolbar dark>
          <v-toolbar-title>New Order</v-toolbar-title>
          <v-spacer />

          <v-menu
            ref="menu2"
            v-model="menu2"
            :close-on-content-click="false"
            :return-value.sync="date"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="date"
                class="pt-5 mt-2 pr-2"
                label="Due Date"
                solo-inverted
                readonly
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu2 = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu2.save(date)"
                >OK</v-btn
              >
            </v-date-picker>
          </v-menu>
          
          <v-text-field
            class="pt-5 mt-2"
            solo-inverted
            label="#545353"
            disabled
          ></v-text-field>
        </v-toolbar>

        <v-container>
          <v-row class="mx-2">
            <v-col cols="12">Product Main Detail<v-divider></v-divider></v-col>
            <v-col cols="12">
              <v-autocomplete
                v-model="value"
                :items="items"
                dense
                filled
                label="Customer Name"
              ></v-autocomplete>
            </v-col>

            
            <v-col cols="12">
              <stocktable />
            </v-col>
            <v-col cols="6">
              <v-textarea
                auto-grow
                outlined
                label="Customer Note"
                placeholder="Please elaborate more about the item"
              />
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