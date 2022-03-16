<template>
  <v-main>
    <v-card>
      <v-toolbar>
        <v-toolbar-title class="text-subtitle-1">
          あなたのことを聞かせてください。
        </v-toolbar-title>
      </v-toolbar>
      <v-card-text style="min-height: 150px">
        <span class="text-h6">
          1.非常にたくさんの仕事をしなければならない
        </span>
      </v-card-text>
      <v-chip-group v-model="selected1" active-class="primary white--text">
        <v-chip value="1" style="width: 100%">
          1
        </v-chip>
        <v-chip pill large value="2" style="width: 100%">
          2
        </v-chip>
        <v-chip pill large value="3" style="width: 100%">
          3
        </v-chip>
        <v-chip pill large value="4" style="width: 100%">
          4
        </v-chip>
      </v-chip-group>
      <div>
        selected : {{ selected1 }}
      </div>
      <v-btn @click="selected1 = null">
        Clear
      </v-btn>
      <v-card-text style="min-height: 150px">
        <span class="text-h6">
          1.非常にたくさんの仕事をしなければならない
        </span>
      </v-card-text>
      <v-chip-group v-model="selected2" active-class="primary white--text">
        <v-chip value="1" style="width: 100%">
          1
        </v-chip>
        <v-chip pill large value="2" style="width: 100%">
          2
        </v-chip>
        <v-chip pill large value="3" style="width: 100%">
          3
        </v-chip>
        <v-chip pill large value="4" style="width: 100%">
          4
        </v-chip>
      </v-chip-group>
      <div>
        selected : {{ selected2 }}
      </div>
      <v-btn @click="selected2 = null">
        Clear
      </v-btn>
    </v-card>
  </v-main>
</template>

<script>
export default {
  name: 'TestPage',
  data () {
    return {
      selected1: '',
      selected2: ''
    }
  }
}
</script>

<style>

</style>
