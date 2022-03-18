<template>
  <v-app>
    <div>
      ストレスチェックは以上です。お疲れさまでした。
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
    <v-btn class="mx-5" to="/">
      トップページへ
    </v-btn>
  </v-app>
</template>

<script>
import { collection, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
const stressCheckCollectionRef = collection(db, 'stresschecks')

export default {
  name: 'ResultPage',
  data () {
    return {
      stressCheckArr: [],
      stressChecklist: '',
      questionsA: [],
      questionsApoint: '',
      questionsB: [],
      questionsBpoint: '',
      questionsC: [],
      questionsCpoint: '',
      questionsD: [],
      questionsDpoint: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(stressCheckCollectionRef, (querySnapshot) => {
      this.stressCheckArr = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.stressCheckArr.forEach((item) => {
        this.stressChecklist = item.questionpoint
      })
      this.stressChecklist.forEach((item) => {
        if (item.group === 'A') {
          this.questionsA.push(item.point)
          this.questionsA = this.questionsA.map(Number)
          this.questionsApoint = this.questionsA.reduce((sum, element) => {
            return sum + element
          }, 0)
        } else if (item.group === 'B') {
          this.questionsB.push(item.point)
          this.questionsB = this.questionsB.map(Number)
          this.questionsBpoint = this.questionsB.reduce((sum, element) => {
            return sum + element
          }, 0)
        } else if (item.group === 'C') {
          this.questionsC.push(item.point)
          this.questionsC = this.questionsC.map(Number)
          this.questionsCpoint = this.questionsC.reduce((sum, element) => {
            return sum + element
          }, 0)
        } else if (item.group === 'D') {
          this.questionsD.push(item.point)
          this.questionsD = this.questionsD.map(Number)
          this.questionsDpoint = this.questionsD.reduce((sum, element) => {
            return sum + element
          }, 0)
        }
      })
      // this.questionsA = this.questionsTotalPoint.filter(value => value.point.group === 'A')
      // this.questionsB = this.questionsTotalPoint.filter(value => value.point.group === 'B')
      // this.questionsC = this.questionsTotalPoint.filter(value => value.point.group === 'C')
      // this.questionsD = this.questionsTotalPoint.filter(value => value.point.group === 'D')
    })
  }
}
</script>

<style>

</style>
