<script>
import { v4 as uuidv4 } from 'uuid';

export default {
  props: ['title'],
  data() {
    return {
      password: '',
      tempPass: '',
      options: {
        number: false,
        uppercase: false,
        special: false
      },
      value: 4,
      max: 24
    }
  },
  methods: {
    generatePassword() {    
      if (this.value < 4) return;
      
      const pass = (uuidv4() + '').replaceAll('-', '');
      
      if (this.options.number) {
        this.password = pass.replace(/\D/g,'').substring(0, this.value);
        if (this.findEquals(this.password)) this.generatePassword();
        if (this.password.length + this.tempPass.length < this.value) {
          this.tempPass = this.password;
          this.generatePassword();
        }
        if (this.tempPass.length > 0) {
          this.password = this.tempPass + this.password
          this.password = this.password.substring(0, this.value);
          this.tempPass = ''
        }
        return;
      }
      if (this.options.uppercase) {
        this.password = pass.substring(0, this.value);
        if (this.findEquals(this.password)) this.generatePassword();
        this.toUpperCase(this.password);
        return;
      }
      this.password = pass.substring(0, this.value);
    },
    
    findEquals(pass = '') {
      for (let index = 0; index < pass.length; index++) {
        const element = pass[index];
        if (element === pass[index + 1]) return true;
      }
      return false;
    },
    toUpperCase(pass = '') {
      pass = pass.split('');
      for (let index = 0; index < pass.length / 2; index++) {
        const position = Math.floor(Math.random() * this.value);
        if (!isNaN(pass.at(position))) continue;
        pass[position] = pass.at(position).toUpperCase()
      }
      this.password = pass.join().replaceAll(',', '');
    },
    resetOptionUpperAndSpecial() {
      this.options.special = false;
      this.options.uppercase = false;
    }
  }
}
</script>

<template>
  <v-row>
    <v-col cols="9">
      <v-text-field 
        label="Password"
        v-model="password">
      </v-text-field>
    </v-col>
    <v-col cols="3">
      <v-text-field 
        v-model="value" 
        type="number" 
        step="1" 
        min="4" 
        max="24"
        @change="generatePassword">
      </v-text-field>
    </v-col>
  </v-row>
  <div class="d-flex mb-5">
    <input
      type="checkbox"
      label="Number"
      v-model="options['number']"
      @change="resetOptionUpperAndSpecial()"
    >&nbsp; Only Numbers &nbsp; 
    <input
      type="checkbox"
      label="Letters"
      v-model="options['uppercase']"
      :disabled="options.number"
    >
    <span :disabled="options.number">&nbsp; Uppercase &nbsp;</span> 
    <input
      type="checkbox"
      label="Caracter especial"
      v-model="options['special']"
      :disabled="options.number"
    >
    <span :disabled="options.number">&nbsp; Special Char</span>
  </div>
  <v-btn
    color="primary"
    @click="generatePassword()"
    >Generate</v-btn>
</template>

<style scoped>
</style>