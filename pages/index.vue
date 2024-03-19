<template>
  <!-- fluid ... コンテナ領域を画面いっぱいにする。color="transparent" ... カラーを透明に指定しています。デフォルトは白である -->
  <v-container fluid>
    <v-card
      flat
      tile
      color="transparent"
    >
      <v-card-title>
        Usersテーブルの取得
      </v-card-title>
      <v-card-text>
        <v-simple-table >
          <template
            v-if="users.length"
            v-slot:default
          >
            <thead>
              <tr>
                <th
                  v-for="(key, i) in userKeys"
                  :key="`key-${i}`"
                >
                  {{ key }}
                </th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(user, i) in users"
                :key="`user-${i}`"
              >
                <td>{{ user.id }}</td>
                <td>{{ user.name }}</td>
                <td>{{ user.email }}</td>
                <td>{{ dateFormat(user.created_at) }}</td>
              </tr>
            </tbody>
          </template>
          <template v-else>
            ユーザーが存在しません
          </template>
        </v-simple-table>
      </v-card-text>
      <v-card-title>
        Vuetifyの導入（オリジナルカラーの確認）
      </v-card-title>
      <v-card-text>
        <v-btn
          v-for="(color, i) in colors"
          :key="`color-${i}`"
          :color="color"
          class="mr-2"
        >
          {{ color }}
        </v-btn>

        <v-row>
          <v-col cols="12" sm="4">
            <v-btn icon color="primary" class="mr-2"><v-icon>mdi-home</v-icon></v-btn>
            <span>ホーム</span>
          </v-col>
          <v-col cols="12" sm="4">
            <v-btn icon color="info" class="mr-2"><v-icon>mdi-email</v-icon></v-btn>
            <span>メール</span>
          </v-col>
          <v-col cols="12" sm="4">
            <v-btn icon color="success" class="mr-2"><v-icon>mdi-check</v-icon></v-btn>
            <span>完了</span>
          </v-col>
          <v-col cols="12" sm="4">
            <v-btn icon color="warning" class="mr-2"><v-icon>mdi-alert</v-icon></v-btn>
            <span>警告</span>
          </v-col>
          <v-col cols="12" sm="4">
            <v-btn icon color="error" class="mr-2"><v-icon>mdi-close</v-icon></v-btn>
            <span>閉じる</span>
          </v-col>
          <v-col cols="12" sm="4">
            <v-btn icon color="background" class="mr-2"><v-icon>mdi-heart</v-icon></v-btn>
            <span>お気に入り</span>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  async asyncData ({ $axios }) {
    let users = []
    await $axios.$get('/api/v1/users').then(res => (users = res))
    const userKeys = Object.keys(users[0] || {}) // 追加
    return { users, userKeys }
  },
  // data () 追加
  data () {
    return {
      colors: ['primary', 'info', 'success', 'warning', 'error', 'background']
    }
  },
  computed: {
    dateFormat () {
      return (date) => {
        const dateTimeFormat = new Intl.DateTimeFormat(
          'ja', { dateStyle: 'medium', timeStyle: 'short' }
        )
        return dateTimeFormat.format(new Date(date))
      }
    }
  }
}
</script>
