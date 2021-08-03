<template>

  

<v-layout justify-center>
  
<v-flex xl8 md10 sm10 xs12>

 
      <v-card>
      <v-toolbar dark>
          <v-toolbar-title>New Order</v-toolbar-title>
          <v-spacer/>
          <v-spacer/>
          <v-spacer/>
          <v-spacer/>
          <v-text-field class="pt-5 mt-2" solo-inverted label="#545353"></v-text-field>
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
            
            
            <v-col cols="6">
             <v-menu
        ref="menu"
        v-model="menu"
        :close-on-content-click="false"
        :return-value.sync="date"
        transition="scale-transition"
        offset-y
        min-width="290px"
      >
        <template v-slot:activator="{ on }">
          <v-text-field
            v-model="date"
            label="Order Date"
            filled
            readonly
            v-on="on"
          ></v-text-field>
        </template>
        <v-date-picker v-model="date" no-title scrollable>
          <v-spacer></v-spacer>
          <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
          <v-btn text color="primary" @click="$refs.menu.save(date)">OK</v-btn>
        </v-date-picker>
      </v-menu>
            </v-col>
            <v-col cols="6">
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
            label="Due Date"
            filled
            readonly
            v-on="on"
          ></v-text-field>
        </template>
        <v-date-picker v-model="date" no-title scrollable>
          <v-spacer></v-spacer>
          <v-btn text color="primary" @click="menu2 = false">Cancel</v-btn>
          <v-btn text color="primary" @click="$refs.menu2.save(date)">OK</v-btn>
        </v-date-picker>
      </v-menu>
            </v-col>
            
            <v-col cols="2">
              <v-autocomplete
                v-model="value"
                :items="items"
                
                outlined
                label="Product Code"
                ></v-autocomplete>
            </v-col>
             
            <v-col cols="4">
              <v-text-field
              
       
            label="Product Name"
           readonly
           disabled
            outlined
          ></v-text-field>
            </v-col>
            <v-col cols="1">
              <v-text-field
              
       
            label="Size"
           readonly
           disabled
            outlined
          ></v-text-field>
            </v-col>
            <v-col cols="1">
              <v-text-field
              
       
            label="Price"
           readonly
           disabled
            outlined
          ></v-text-field>
            </v-col>
            <v-col cols="1">
              <v-text-field
              
       
            label="Weight"
           readonly
           disabled
            outlined
          ></v-text-field>
            </v-col>
            <v-col cols="1">
              <v-text-field
              
       
            label="labor Charge"
           readonly
           disabled
            outlined
          ></v-text-field>
            </v-col>
            <v-col cols="1">
              <v-text-field
              
       
            label="labor Charge"
           readonly
           disabled
            outlined
          ></v-text-field>
            </v-col>
           <v-col cols="1">
              <v-btn x-large color="primary" icon><v-icon>mdi-plus</v-icon></v-btn>
            </v-col>
            <v-col cols="12">
            <v-data-table
    :headers="headers"
    :items="desserts"
    :items-per-page="5"
    class="elevation-1"
    hide-default-footer
  ></v-data-table>
            </v-col>
              <v-col cols="6">
              <v-textarea auto-grow outlined label="Customer Note" placeholder="Please elaborate more about the item"/>
            </v-col>
            
            
      
           
            
          </v-row>
          
        </v-container>
        
        <v-card-actions>
          <v-btn text color="red">Clear Form <v-icon>mdi-broom</v-icon></v-btn>
          <v-spacer />
          <v-btn color="red" dark large>Cancel <v-icon>mdi-cancel</v-icon></v-btn>
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
        date: new Date().toISOString().substr(0, 10),
      menu: false,
      modal: false,
      menu2: false,
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters',
      ],
      email: '',
      
      select: null,
      items: [
        'Enamel',
        'Zirconia Stone',
        'Pure Stone',
        'Gold Plated',
        'Plastic',
      ],
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
      validate () {
        this.$refs.form.validate()
      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },
    },
  }
</script>