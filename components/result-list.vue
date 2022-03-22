<template>
  <v-app>
    <div>
      検査履歴
    </div>
    <v-list class="overflow-y-auto" height="150">
      <div v-for="list in resultList" :key="list.id">
        <div>
          {{ list.userDisplayName }}さん : {{ $dateFns.format(list.timestamp.toDate(), 'yyyy-MM-dd HH:mm') }}
          <v-icon color="blue" @click="selected = list.questionpoint;time = list.timestamp">
            mdi-eye-circle-outline
          </v-icon>
        </div>
      </div>
    </v-list>
    <div v-if="time !== ''" class="my-10">
      <v-card>
        {{ user.displayName }}さん
        {{ $dateFns.format(time.toDate(), 'yyyy-MM-dd HH:mm') }}
        <div>
          のストレスチェックの結果
        </div>
      </v-card>
    </div>
    <div v-if="selected !== ''" class="my-5">
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
      selected: '',
      time: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
    onSnapshot(stressCheckCollectionRef, (querySnapshot) => {
      this.resultList = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
      this.resultList.sort((a, b) => b.timestamp - a.timestamp)
    })
  }
}
</script>

<style>

</style>
