<template>
  <v-app>
    <div v-show="questionsPage === 1">
      <StressCheck v-for="question in questionsA" :key="question.id" :question="question" @selectPoint="points" />
    </div>
    <div v-show="questionsPage === 2">
      <StressCheck v-for="question in questionsB" :key="question.id" :question="question" @selectPoint="points" />
    </div>
    <div v-show="questionsPage === 3">
      <StressCheck v-for="question in questionsC" :key="question.id" :question="question" @selectPoint="points" />
    </div>
    <div v-show="questionsPage === 4">
      <StressCheck v-for="question in questionsD" :key="question.id" :question="question" @selectPoint="points" />
    </div>
    <div v-show="questionsPage === 5">
      <Result />
    </div>
    <v-btn v-show="questionsPage < 4" class="my-5" depressed color="primary" @click="questionsPage = questionsPage + 1">
      次へ
    </v-btn>
    <v-btn v-show="questionsPage > 1" class="my-5" depressed color="primary" @click="questionsPage = questionsPage - 1">
      戻る
    </v-btn>
    <v-btn v-show="questionsPage > 3" class="my-5" depressed color="primary" @click="addStressCheck">
      診断結果へ
    </v-btn>
    <div>{{ message }}</div>
  </v-app>
</template>

<script>
import { addDoc, collection, onSnapshot, serverTimestamp } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
import StressCheck from '../components/stress-check'
import result from '../components/result'
const questionsCollectionRef = collection(db, 'questions')
const stressCheckCollectionRef = collection(db, 'stresschecks')

export default {
  name: 'CheckForm',
  components: {
    StressCheck,
    result
  },
  data () {
    return {
      questions: [],
      questionsA: [],
      questionsB: [],
      questionsC: [],
      questionsD: [],
      questionsPage: 5,
      selected: [],
      message: ''
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
  },
  methods: {
    addStressCheck () {
      // if (this.selected.length === 57) {
      addDoc(stressCheckCollectionRef, {
        userUid: this.user.uid,
        userDisplayName: this.user.displayName,
        timestamp: serverTimestamp(),
        questionpoint: this.selected
      }).then(() => {
        this.questionsPage = this.questionsPage + 1
      })
      // } else {
      //   this.message = '未記入箇所があります'
      // }
    },
    points (value) {
      const index = this.selected.findIndex(el => el.id === value.id)
      if (index < 0) {
        this.selected.push({ ...value })
      } else if (value.point) {
        this.selected[index] = { ...value }
      } else {
        this.selected.splice(index, 1)
      }
    }
  }
}
</script>

<style>

</style>
