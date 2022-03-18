<template>
  <v-app>
    <v-container class="pt-15 text-center">
      <div>
        ストレスチェックは以上です。
      </div>
      <div>
        お疲れさまでした。
      </div>
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
      <v-btn class="mx-5" to="/" color="primary">
        トップページへ
      </v-btn>
    </v-container>
  </v-app>
</template>

<script>
// import { collection, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth } from '../plugins/firebase'
// const stressCheckCollectionRef = collection(db, 'stresschecks')

export default {
  name: 'ResultPage',
  props: {
    selected: { type: Array, default: null }
  },
  data () {
    return {
      user: '',
      stressCheckArr: ''
      // stressChecklist: []
      // questionsA: [],
      // questionsApoint: 0,
      // questionsB: [],
      // questionsBpoint: 0,
      // questionsC: [],
      // questionsCpoint: 0,
      // questionsD: [],
      // questionsDpoint: 0
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
    // this.selected.forEach((item) => {
    //   if (item.group === 'A') {
    //     this.questionsApoint += Number(item.point)
    //   } else if (item.group === 'B') {
    //     this.questionsBpoint += Number(item.point)
    //   } else if (item.group === 'C') {
    //     this.questionsCpoint += Number(item.point)
    //   } else if (item.group === 'D') {
    //     this.questionsDpoint += Number(item.point)
    //   }
    // })
    // onSnapshot(stressCheckCollectionRef, (querySnapshot) => {
    //   this.stressCheckArr = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    //   this.stressCheckArr.forEach((item) => {
    //     this.stressChecklist = item.questionpoint
    //   })
    //   this.stressChecklist.forEach((item) => {
    //     if (item.group === 'A') {
    //       this.questionsApoint += Number(item.point)
    //     } else if (item.group === 'B') {
    //       this.questionsBpoint += Number(item.point)
    //     } else if (item.group === 'C') {
    //       this.questionsCpoint += Number(item.point)
    //     } else if (item.group === 'D') {
    //       this.questionsDpoint += Number(item.point)
    //     }
    //     // item.forEach((i) => {
    //     //   if (i.group === 'A') {
    //     //     this.questionsA.push(i.point)
    //     //     this.questionsA = this.questionsA.map(Number)
    //     //     this.questionsApoint = this.questionsA.reduce((sum, element) => {
    //     //       return sum + element
    //     //     }, 0)
    //     //   } else if (i.group === 'B') {
    //     //     this.questionsB.push(i.point)
    //     //     this.questionsB = this.questionsB.map(Number)
    //     //     this.questionsBpoint = this.questionsB.reduce((sum, element) => {
    //     //       return sum + element
    //     //     }, 0)
    //     //   } else if (i.group === 'C') {
    //     //     this.questionsC.push(i.point)
    //     //     this.questionsC = this.questionsC.map(Number)
    //     //     this.questionsCpoint = this.questionsC.reduce((sum, element) => {
    //     //       return sum + element
    //     //     }, 0)
    //     //   } else if (i.group === 'D') {
    //     //     this.questionsD.push(i.point)
    //     //     this.questionsD = this.questionsD.map(Number)
    //     //     this.questionsDpoint = this.questionsD.reduce((sum, element) => {
    //     //       return sum + element
    //     //     }, 0)
    //     //   }
    //     // })
    //   })
    // })
  }
}
</script>

<style>

</style>
