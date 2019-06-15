<template>
  <div>
    <v-layout row wrap>
      <v-flex>
        <v-checkbox v-model="isRandom" label="Random"></v-checkbox>
      </v-flex>
      <v-flex>
        <v-checkbox v-model="isArray" label="Array"></v-checkbox>
      </v-flex>
      <v-flex>
        <v-radio-group v-model="selectedMode" row>
          <v-radio
            v-for="n in mode"
            :key="n"
            :label="`${n.label}`"
            :value="n.value"
            :disabled="n.disabled"
          ></v-radio>
        </v-radio-group>
      </v-flex>
    </v-layout>

    <!-- integer -->
    <div v-if="selectedMode === 'integer'">
      <div v-if="isRandom">
        <h1>when random</h1>
        <v-btn @click="min = 1; max = Number.MAX_SAFE_INTEGER">Natural Number</v-btn>
        <v-btn @click="min = 0; max = Number.MAX_SAFE_INTEGER">Whole Number</v-btn>
        <v-btn @click="min = Number.MIN_SAFE_INTEGER; max = 0">Negative Integer</v-btn>

        sparse number percentage

        <v-text-field
          v-model="min"
          :rules="number"
          label="Minumum"
          required
        ></v-text-field>
        int min
        <v-text-field
          v-model="max"
          :rules="number"
          label="Maximum"
          required
        ></v-text-field>
        int max
      </div>
      <div v-if="isArray">
        <h1>when array</h1>
        <v-text-field
          v-model="howMany"
          :rules="number"
          label="Maximum"
          required
        ></v-text-field>
      </div>
      integer mode
    </div>
    <!-- float -->
    <div v-if="selectedMode === 'float'">
      float mode
    </div>
    <!-- string -->
    <div v-if="selectedMode === 'string'">
      string mode
    </div>
    <v-btn @click="generate()">Generate</v-btn>
    <pre>
      {{ JSON.stringify(output) }}
    </pre>
  </div>
</template>

<style>
</style>

<script>
export default {
  components: {},
  data() {
    return {
      mode: [
        { value: 'integer', label: 'integer' },
        { value: 'float', label: 'float', disabled: true },
        { value: 'string', label: 'string', disabled: true }
      ],
      selectedMode: 'integer',
      isArray: true,
      isRandom: true,
      min: 0,
      max: 10000,
      howMany: 10,
      output: null
    }
  },
  methods: {
    generate() {
      const randoms = []
      switch (this.selectedMode) {
        case 'integer':
          if (this.isArray) {
            for (let i = 0; i < this.howMany; i++) {
              randoms.push(this.getRandomInt(this.min, this.max))
            }
            /* eslint no-console:0 */
            console.log('randoms', randoms)
            this.output = randoms
          } else {
            this.output = this.getRandomInt(this.min, this.max)
          }
      }
      return this.output
    },
    random() {
      switch (this.selectedMode) {
        case 'integer':
          return this.getRandomInt(this.min, this.max)
      }
      return true
    },
    getRandomInt(min, max) {
      min = Math.ceil(min)
      max = Math.floor(max)
      return Math.floor(Math.random() * (max - min + 1)) + min
    }
  }
}
</script>

<!--function getRandomArbitrary(min, max) {-->
<!--return Math.random() * (max - min) + min;-->
<!--}-->
