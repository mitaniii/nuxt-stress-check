<template>
  <div>
    <v-card-text style="min-height: 150px">
      <span class="text-h6">
        {{ question.id }} : {{ question.text }}
      </span>
    </v-card-text>
    <v-chip-group v-model="selected" active-class="primary white--text" @change="selectPoint">
      <v-chip value="1" style="width: 20%">
        そうだ:1
      </v-chip>
      <v-chip value="2" style="width: 20%">
        まあそうだ:2
      </v-chip>
      <v-chip value="3" style="width: 20%">
        ややちがう:3
      </v-chip>
      <v-chip value="4" style="width: 20%">
        ちがう:4
      </v-chip>
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
