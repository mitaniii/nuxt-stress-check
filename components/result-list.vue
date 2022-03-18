<template>
  <v-app>
    <div>
      ResultListPage
    </div>
    <div v-for="list in resultList" :key="list.id">
      <div>
        {{ list.userDisplayName }}
      </div>
    </div>
    <ResultHistory />
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
      resultList: ''
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
