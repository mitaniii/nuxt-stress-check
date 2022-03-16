<template>
  <v-app>
    <div v-if="user">
      ユーザー：{{ user.uid }}
    </div>
    <div v-else />
    <div v-for="question in questions" :key="question.id">
      <div v-if="question.group === 'A'">
        {{ question.text }}
      </div>
      <div v-else-if="question.group === 'B'">
        {{ question.text }}
      </div>
      <div v-else-if="question.group === 'C'">
        {{ question.text }}
      </div>
      <div v-else-if="question.group === 'D'">
        {{ question.text }}
      </div>
    </div>
  </v-app>
</template>

<script>
import { collection, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
const questionsCollectionRef = collection(db, 'questions')

export default {
  name: 'StressCheck',
  data () {
    return {
      questions: []
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(questionsCollectionRef, (querySnapshot) => {
      this.questions = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
  }
}
</script>

<style>

</style>
