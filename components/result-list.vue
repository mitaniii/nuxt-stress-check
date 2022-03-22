<template>
  <v-app>
    <div>
      検査履歴
    </div>
    <div v-for="list in resultList" :key="list.id">
      <div>
        {{ list.userDisplayName }}さん : {{ list.timestamp.toDate() }}
        <v-icon color="blue" @click="selected = list.questionpoint">
          mdi-eye-circle-outline
        </v-icon>
      </div>
    </div>
    <div v-if="selected !== ''">
      <ResultHistory :selected="selected" />
    </div>
    <div v-else />
  </v-app>
</template>

<script>
import { collection, onSnapshot } from '@firebase/firestore'
import { onAuthStateChanged } from '@firebase/auth'
import { auth, db } from '../plugins/firebase'
import ResultHistory from '../components/result-history'
const stressCheckCollectionRef = collection(db, 'stresschecks')
export default {
  name: 'ResultListPage',
  components: {
    ResultHistory
  },
  data () {
    return {
      user: '',
      resultList: '',
      selected: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(stressCheckCollectionRef, (querySnapshot) => {
      this.resultList = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
  }
}
</script>

<style>

</style>
