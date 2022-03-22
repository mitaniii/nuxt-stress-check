<template>
  <v-app>
    <v-container class="pt-15 text-center">
      <div v-if="user">
        <div class="my-1">
          {{ user.displayName }} さん
        </div>
        <div>
          <v-btn class="my-10" @click="logout">
            ログアウト
          </v-btn>
        </div>
        <v-btn class="my-10" to="/check-form" color="primary">
          ストレスチェック開始
        </v-btn>
        <ResultList />
      </div>
      <div v-else>
        <v-btn class="mx-5" to="/login">
          ログイン
        </v-btn>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { auth } from '../plugins/firebase'
// import ResultList from '../components/result-list'

export default {
  name: 'IndexPage',
  // components: {
  //   ResultList
  // },
  data () {
    return {
      user: ''
    }
  },
  mounted () {
    onAuthStateChanged(auth, (user) => {
      this.user = user
    })
  },
  methods: {
    logout () {
      signOut(auth).then(() => (this.user = null))
    }
  }
}
</script>

<style>

</style>
