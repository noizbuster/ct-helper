<template>
  <div>
    <v-layout row wrap>
      <v-flex>
        <v-checkbox v-model="isRandom" label="Random" />
      </v-flex>
      <v-flex>
        <v-checkbox v-model="isArray" label="Array" />
      </v-flex>
      <v-flex>
        <v-radio-group v-model="selectedMode" row>
          <v-radio
            v-for="n in mode"
            :key="n"
            :label="`${n.label}`"
            :value="n.value"
            :disabled="n.disabled"
          />
        </v-radio-group>
      </v-flex>
    </v-layout>

    <!-- integer -->
    <div v-if="selectedMode === 'integer'">
      <div v-if="!isRandom">
        <h1>Manual Mode</h1>
        <v-text-field
          v-model="manualInput"
          :rules="number"
          label="ManualInput"
          required
        />
      </div>
      <div v-if="isRandom">
        <h1>when random</h1>

        <v-radio-group v-model="isRandomInRange" row>
          <v-radio label="In range" :value="true" />
          <v-radio label="In Domain" :value="false" />
        </v-radio-group>

        <div v-if="isRandomInRange">
          <v-btn
            @click="
              min = 1;
              max = Number.MAX_SAFE_INTEGER;
            "
          >
            Natural Number
          </v-btn>
          <v-btn
            @click="
              min = 0;
              max = Number.MAX_SAFE_INTEGER;
            "
          >
            Whole Number
          </v-btn>
          <v-btn
            @click="
              min = Number.MIN_SAFE_INTEGER;
              max = 0;
            "
          >
            Negative Integer
          </v-btn>

          <v-text-field
            v-model="min"
            :rules="number"
            label="Minumum"
            required
          />
          <v-text-field
            v-model="max"
            :rules="number"
            label="Maximum"
            required
          />
        </div>

        <div v-if="!isRandomInRange">
          <v-text-field
            v-model="domains"
            :rules="number"
            label="Custom Domain"
            required
          />
        </div>
        <v-checkbox v-model="isSparse" label="Sparse" />
        <div v-if="isSparse">
          <h2>Sparse</h2>
          <v-text-field
            v-model="sparseDensity"
            :rules="number"
            label="Sparse Density"
            required
          />
          <v-text-field
            v-model="sparseBase"
            :rules="number"
            label="Sparse Base Value"
            required
          />
        </div>
      </div>
      <div v-if="isArray && isRandom">
        <h1>when array</h1>
        <v-text-field
          v-model="howMany"
          :rules="number"
          label="Number of Integer"
          required
        />
        <v-checkbox v-model="is2DArray" label="2D Array" />
        <v-text-field
          v-if="is2DArray"
          v-model="howManyY"
          :rules="number"
          label="Y-Axis of 2D array"
          required
        />
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
    <v-btn @click="generate()">
      Generate
    </v-btn>
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
      is2DArray: false,
      isRandom: true,
      isRandomInRange: true,
      isSparse: false,
      sparseDensity: 0.1,
      sparseBase: 0,
      min: 0,
      max: 10000,
      howMany: 10,
      howManyY: 10,
      output: null
    }
  },
  methods: {
    generate() {
      const randoms = []
      switch (this.selectedMode) {
        case 'integer':
          if (!this.isRandom) {
            try {
              this.output = JSON.parse(this.manualInput)
            } catch (e) {
              this.output = { msg: 'failed to parse into json from ' + this.manualInput }
            }
            break
          }
          if (this.isArray) {
            if (this.is2DArray) {
              for (let y = 0; y < this.howManyY; y++) {
                const subRandoms = []
                for (let i = 0; i < this.howMany; i++) {
                  subRandoms.push(this.getRandomInt(this.min, this.max))
                }
                randoms.push(subRandoms)
              }
            } else {
              for (let i = 0; i < this.howMany; i++) {
                randoms.push(this.getRandomInt(this.min, this.max))
              }
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
