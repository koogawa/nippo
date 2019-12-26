<template>
  <div class="container">
    <div>
      <logo />
      <table>
          <tr v-for="todo in todos" v-bind:key="todo.start">
              <td>
                  <input type="checkbox" id="checkbox" v-model="todo.checked">
                  <input type="time" v-model="todo.start" step="300"> 〜 <input type="time" v-model="todo.end" step="300">
                  <input type="text" v-model="todo.task" size="60">
              </td>
          </tr>
      </table>

      <ul><li>チェックを入れていない行は無視されるわよ</li></ul>

      <p>
          <textarea id="copyTarget" v-model="output" rows="20" cols="80"></textarea>
      </p>
      <p>
          <button v-on:click="copyToClipboard()">クリップボードにコピー</button>
          <button v-on:click="copyToClipboardAndFinish()">クリップボードにコピーして退勤</button>
      </p>

    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  mounted () {
  },
  asyncData (context) {
    // コンポーネントをロードする前に毎回呼び出されます
    // 名前の通り非同期にすることができます
    // また、返されたオブジェクトはデータオブジェクトとマージされます
    return { name: 'World' }
  },
  fetch () {
    // `fetch` メソッドはページの描画前にストアを満たすために使用されます
  },
  head () {
    // このページ向けにメタタグを設定します
  },
  data() {
    return {
      todos: [
        { checked: true, start: '08:00', end: "09:00", task: "コードレビュータイム" },
        { checked: true, start: '09:00', end: "10:00", task: "" },
        { checked: false, start: '10:00', end: "11:00", task: "" },
        { checked: true, start: '10:10', end: "10:25", task: "デイリースクラム" },
        { checked: true, start: '10:25', end: "11:00", task: "" },
        { checked: true, start: '11:00', end: "12:00", task: "" },
        { checked: true, start: '12:00', end: "13:00", task: "昼休憩" },
        { checked: true, start: '13:00', end: "14:00", task: "" },
        { checked: true, start: '14:00', end: "15:00", task: "" },
        { checked: true, start: '15:00', end: "16:00", task: "" },
        { checked: true, start: '16:00', end: "17:00", task: "" },
        { checked: false, start: '17:00', end: "18:00", task: "" },
        { checked: false, start: '18:00', end: "19:00", task: "" },
        { checked: false, start: '19:00', end: "20:00", task: "" },
      ]
    }
  },
  computed: {
      output: function() {
          var result = "```\n"
          result += "--------------\n"
          result += "今日の作業ログ\n"
          result += "--------------\n"
          for ( var i in this.todos ) {
              if ( !this.todos[i].checked ) { continue }
              result += this.todos[i].start + "-" + this.todos[i].end + ' ' + this.todos[i].task + "\n"
          }
          result += "```\n"
          return result
      }
  },
  methods: {
      copyToClipboard: function() {
          var copyTarget = document.getElementById("copyTarget");
          copyTarget.select();
          document.execCommand("Copy");
          alert("コピーできました！ : " + copyTarget.value);
      },
      copyToClipboardAndFinish: function() {
          this.copyToClipboard();
          location.href = "https://attendance.moneyforward.com/my_page";
      }
  }
}
</script>

<style>
.container {
  margin: 8px;
}
</style>
