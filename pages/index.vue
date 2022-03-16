<template>
  <v-app>
    <div v-if="user">
      ユーザー：{{ user.uid }}
      <v-btn class="mx-5" @click="logout">
        ログアウト
      </v-btn>
      <v-btn class="mx-5" to="/check-form">
        ストレスチェック開始
      </v-btn>
    </div>
    <div v-else>
      <v-btn class="mx-5" to="/login">
        ログイン
      </v-btn>
    </div>
  </v-app>
</template>

<script>
import { onAuthStateChanged, signOut } from '@firebase/auth'
import { auth } from '../plugins/firebase'

export default {
  name: 'IndexPage',
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
