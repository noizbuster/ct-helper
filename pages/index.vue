<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <Variable></Variable>
    </v-flex>
  </v-layout>
</template>

<script>
import Variable from '~/components/Variable.vue'

export default {
  components: {
    Variable
  }
}
</script>
