<template>
  <v-container>
    <div>
      <div v-if="question.area === '1'">
        次の人たちはどのくらい気軽に話ができますか?
      </div>
      <div v-if="question.area === '2'">
        あなたが困った時、次の人たちはどのくらい頼りになりますか?
      </div>
      <div v-if="question.area === '3'">
        あなたの個人的な問題を相談したら、次の人たちはどのくらいきいてくれますか?
      </div>
      <div v-else />
      <v-card-text style="min-height: 150px">
        <span class="text-h6">
          {{ question.id }} : {{ question.text }}
        </span>
      </v-card-text>
      <v-chip-group v-model="selected" active-class="primary white--text" @change="selectPoint">
        <div v-if="question.reverse === true && question.group === 'A'">
          <div>
            <v-chip value="4" style="width: 100%">
              そうだ
            </v-chip>
          </div>
          <div>
            <v-chip value="3" style="width: 100%">
              まあそうだ
            </v-chip>
          </div>
          <div>
            <v-chip value="2" style="width: 100%">
              ややちがう
            </v-chip>
          </div>
          <div>
            <v-chip value="1" style="width: 100%">
              ちがう
            </v-chip>
          </div>
        </div>
        <div v-if="question.reverse ===false && question.group === 'A'">
          <div>
            <v-chip value="1" style="width: 100%">
              そうだ
            </v-chip>
          </div>
          <div>
            <v-chip value="2" style="width: 100%">
              まあそうだ
            </v-chip>
          </div>
          <div>
            <v-chip value="3" style="width: 100%">
              ややちがう
            </v-chip>
          </div>
          <div>
            <v-chip value="4" style="width: 100%">
              ちがう
            </v-chip>
          </div>
        </div>
        <div v-if="question.reverse === true && question.group === 'B'">
          <div>
            <v-chip value="4" style="width: 100%">
              ほとんどなかった
            </v-chip>
          </div>
          <div>
            <v-chip value="3" style="width: 100%">
              ときどきあった
            </v-chip>
          </div>
          <div>
            <v-chip value="2" style="width: 100%">
              しばしばあった
            </v-chip>
          </div>
          <div>
            <v-chip value="1" style="width: 100%">
              ほとんどいつもあった
            </v-chip>
          </div>
        </div>
        <div v-if="question.reverse ===false && question.group === 'B'">
          <div>
            <v-chip value="1" style="width: 100%">
              ほとんどなかった
            </v-chip>
          </div>
          <div>
            <v-chip value="2" style="width: 100%">
              ときどきあった
            </v-chip>
          </div>
          <div>
            <v-chip value="3" style="width: 100%">
              しばしばあった
            </v-chip>
          </div>
          <div>
            <v-chip value="4" style="width: 100%">
              ほとんどいつもあった
            </v-chip>
          </div>
        </div>
        <div v-if="question.group === 'C'">
          <div>
            <v-chip value="1" style="width: 100%">
              非常に
            </v-chip>
          </div>
          <div>
            <v-chip value="2" style="width: 100%">
              かなり
            </v-chip>
          </div>
          <div>
            <v-chip value="3" style="width: 100%">
              多少
            </v-chip>
          </div>
          <div>
            <v-chip value="4" style="width: 100%">
              全くない
            </v-chip>
          </div>
        </div>
        <div v-if="question.group === 'D'">
          <div>
            <v-chip value="1" style="width: 100%">
              満足
            </v-chip>
          </div>
          <div>
            <v-chip value="2" style="width: 100%">
              まあ満足
            </v-chip>
          </div>
          <div>
            <v-chip value="3" style="width: 100%">
              やや不満
            </v-chip>
          </div>
          <div>
            <v-chip value="4" style="width: 100%">
              不満足
            </v-chip>
          </div>
        </div>
      </v-chip-group>
    </div>
  </v-container>
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
      this.$emit('selectPoint', { id: this.question.id, group: this.question.group, point: this.selected })
    }
  }
}
</script>

<style>

</style>
