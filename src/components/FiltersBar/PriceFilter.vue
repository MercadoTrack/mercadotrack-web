<template>
  <v-layout wrap>
    <v-flex xs6 class="pr-2">
      <v-text-field
        label="Desde"
        type="number"
        clearable
        hide-details
        v-model="priceMin"
        prefix="$"
      ></v-text-field>
    </v-flex>
    <v-flex xs6 class="pl-2">
      <v-text-field
        label="Hasta"
        type="number"
        clearable
        hide-details
        v-model="priceMax"
        prefix="$"
      ></v-text-field>
    </v-flex>
    <v-flex xs12 v-if="error">
      <p class="mt-2 mb-0 red--text">{{ error }}</p>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  name: 'PriceFilter',
  props: ['price', 'error'],
  computed: {
    priceMin: {
      get () {
        return this.price.min
      },
      set (min) {
        const { max } = this.price
        if (min && max && +min > +max) {
          this.$emit('update:error', 'El precio minimo es mayor al precio máximo.')
          return
        }
        this.$emit('update:error', '')
        this.$emit('update:price', { min, max })
      }
    },
    priceMax: {
      get () {
        return this.price.max
      },
      set (max) {
        const { min } = this.price
        if (min && max && +max < +min) {
          this.$emit('update:error', 'El precio máximo es menor al precio mínimo.')
          return
        }
        this.$emit('update:error', '')
        this.$emit('update:price', { min, max })
      }
    }
  },
}
</script>
