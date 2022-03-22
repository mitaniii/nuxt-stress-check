<template>
  <v-app>
    <v-container class="text-center">
      <div>
        A: {{ questionsApoint }}
      </div>
      <div>
        B: {{ questionsBpoint }}
      </div>
      <div>
        C: {{ questionsCpoint }}
      </div>
      <div>
        D: {{ questionsDpoint }}
      </div>
      <div class="my-5">
        <div v-if="questionsBpoint >= 77 || questionsBpoint >= 63 && questionsApoint + questionsCpoint >= 76">
          判定結果：異常あり
        </div>
        <div v-else>
          判定結果：異常なし
        </div>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import { onAuthStateChanged } from '@firebase/auth'
import { auth } from '../plugins/firebase'

export default {
  name: 'ResultHistoryPage',
  props: {
    selected: { type: Array, default: null }
  },
  data () {
    return {
      user: '',
      stressCheckArr: ''
    }
  },
  computed: {
    questionsApoint () {
      return this.selected.filter(item => item.group === 'A').reduce((sum, item) => (sum += Number(item.point)), 0)
    },
    questionsBpoint () {
      return this.selected.filter(item => item.group === 'B').reduce((sum, item) => (sum += Number(item.point)), 0)
    },
    questionsCpoint () {
      return this.selected.filter(item => item.group === 'C').reduce((sum, item) => (sum += Number(item.point)), 0)
    },
    questionsDpoint () {
      return this.selected.filter(item => item.group === 'D').reduce((sum, item) => (sum += Number(item.point)), 0)
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
  }
}
</script>

<style>

</style>
