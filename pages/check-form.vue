<template>
  <v-app>
    <div v-show="questionsPage === 1">
      <StressCheck v-for="question in questionsA" :key="question.id" :question="question" />
    </div>
    <div v-show="questionsPage === 2">
      <StressCheck v-for="question in questionsB" :key="question.id" :question="question" />
    </div>
    <div v-show="questionsPage === 3">
      <StressCheck v-for="question in questionsC" :key="question.id" :question="question" />
    </div>
    <div v-show="questionsPage === 4">
      <StressCheck v-for="question in questionsD" :key="question.id" :question="question" />
    </div>
    <v-btn v-show="questionsPage < 4" @click="questionsPage = questionsPage + 1">
      次へ
    </v-btn>
    <v-btn v-show="questionsPage > 1" @click="questionsPage = questionsPage - 1">
      戻る
    </v-btn>
  </v-app>
</template>

<script>
import { collection, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
import StressCheck from '../components/stress-check'
const questionsCollectionRef = collection(db, 'questions')

export default {
  name: 'CheckForm',
  components: {
    StressCheck
  },
  data () {
    return {
      questions: [],
      questionsA: [],
      questionsB: [],
      questionsC: [],
      questionsD: [],
      questionsPage: 1
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(questionsCollectionRef, (querySnapshot) => {
      this.questions = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.questionsA = this.questions.filter(value => value.group === 'A')
      this.questionsB = this.questions.filter(value => value.group === 'B')
      this.questionsC = this.questions.filter(value => value.group === 'C')
      this.questionsD = this.questions.filter(value => value.group === 'D')
    })
  }
}
</script>

<style>

</style>
