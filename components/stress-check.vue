<template>
  <div>
    <v-card-text style="min-height: 150px">
      <span class="text-h6">
        {{ question.id }} : {{ question.text }}
      </span>
    </v-card-text>
    <v-chip-group v-model="selected" active-class="primary white--text" @change="selectPoint">
      <div v-if="question.reverse === true">
        <v-chip value="4" style="width: 40%">
          そうだ:1
        </v-chip>
        <v-chip value="3" style="width: 40%">
          まあそうだ:2
        </v-chip>
        <v-chip value="2" style="width: 40%">
          ややちがう:3
        </v-chip>
        <v-chip value="1" style="width: 40%">
          ちがう:4
        </v-chip>
      </div>
      <div v-else>
        <v-chip value="1" style="width: 40%">
          そうだ:1
        </v-chip>
        <v-chip value="2" style="width: 40%">
          まあそうだ:2
        </v-chip>
        <v-chip value="3" style="width: 40%">
          ややちがう:3
        </v-chip>
        <v-chip value="4" style="width: 40%">
          ちがう:4
        </v-chip>
      </div>
    </v-chip-group>
  </div>
</template>

<script>

export default {
  name: 'StressCheck',
  props: {
    question: {
      type: Object, default: null
    }
  },
  data () {
    return {
      selected: ''
    }
  },
  methods: {
    selectPoint () {
      this.$emit('selectPoint', { id: this.question.id, point: this.selected })
    }
  }
}
</script>

<style>

</style>
