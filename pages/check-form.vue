<template>
  <v-app>
    <v-container>
      <div v-show="questionsPage === 1">
        A.あなたの仕事についてうかがいます。最もあてはまるものに○を付けてください。
        <StressCheck v-for="question in questionsA" :key="question.id" :question="question" @selectPoint="points" />
      </div>
      <div v-show="questionsPage === 2">
        B.最近1 か月間のあなたの状態についてうかがいます。最もあてはまるものに○を付けてください。
        <StressCheck v-for="question in questionsB" :key="question.id" :question="question" @selectPoint="points" />
      </div>
      <div v-show="questionsPage === 3">
        C.あなたの周りの方々についてうかがいます。最もあてはまるものに○を付けてください。
        <StressCheck v-for="question in questionsC" :key="question.id" :question="question" @selectPoint="points" />
      </div>
      <div v-show="questionsPage === 4">
        D.満足度について
        <StressCheck v-for="question in questionsD" :key="question.id" :question="question" @selectPoint="points" />
      </div>
      <div v-show="questionsPage === 5">
        <Result :selected="selected" />
      </div>
      <div v-show="questionsPage < 5" class="Page-Btn" @click="scrollTop">
        <i class="fas fa-chevron-up Page-Btn-Icon">
          ↑
        </i>
      </div>
      <v-footer v-show="questionsPage < 5">
        <v-btn v-show="questionsPage > 1 && questionsPage < 5" depressed color="primary" @click="questionsPage = questionsPage - 1">
          戻る
        </v-btn>
        <v-spacer />
        <v-btn v-show="questionsPage < 4" depressed color="primary" @click="scrollTop();questionsPage = questionsPage + 1">
          次へ
        </v-btn>
        <v-btn v-show="questionsPage > 3 && questionsPage < 5" depressed color="primary" @click="addStressCheck">
          診断結果へ
        </v-btn>
      </v-footer>
      <div>{{ message }}</div>
    </v-container>
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
      questionsPage: 1,
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
      if (this.selected.length === 57) {
        addDoc(stressCheckCollectionRef, {
          userUid: this.user.uid,
          userDisplayName: this.user.displayName,
          timestamp: serverTimestamp(),
          questionpoint: this.selected
        }).then(() => {
          this.questionsPage = this.questionsPage + 1
        })
      } else {
        this.message = '未記入箇所があります'
      }
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
    },
    scrollTop: () => {
      window.scrollTo({
        top: 0,
        behavior: 'auto'
      })
    }
  }
}
</script>

<style>
.Page-Btn{
  position: fixed;
  right: 14px;
  bottom: 90px;
  width: 32px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  border-radius: 50%;
  background: #5bc8ac;
}
.Page-Btn-Icon{
  color: #fff;
  font-size: 16px;
}
</style>
