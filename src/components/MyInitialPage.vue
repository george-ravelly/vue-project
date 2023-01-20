<script>

export default {
  props: ['title'],
  data() {
    return {
      password: '',
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
      if (this.options.number) {
        let min = Math.ceil(this.value);
        let max = Math.floor(this.max);
        this.password = Math.floor(Math.random() * (max - min) + min);
        console.log(this.password);
        return;
      }
      if (this.options.uppercase) {}
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
        max="24">
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