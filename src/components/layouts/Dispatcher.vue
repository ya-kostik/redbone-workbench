<template lang='pug'>
.Dispatcher.Dispatcher_fix-w
  div
    label.fluentLabel Выберите сервер:
      input.fluent.fluent_fw(v-model='server')
    button.fluent.fluentButton(type='button' @click='onConnect') Подключиться
    button.fluent.fluentButton(type='button' @click='onRemember') Запомнить
  div
    label Пример:
    pre.fluentCode.fluentCode_fw.
      {{sample}}
  form.Dispatcher__form(@submit='onDispatch')
    div
      label Тело диспатча
      div.fluentCode.fluentCode_fw
        codemirror(v-model="body" :options='bodyOptions')
      button.fluent.fluentButton(type='submit' :disabled='!io') Отправить
      button.fluent.fluentButton(type='button' @click='onSnippetAdd') Добавить снипет
    div.Dispatcher__snippets
      template(v-for='(snippet, index) of snippets')
        pre.fluent.fluentCode.fluentCode_fw.fluentButton(@click='onSnippetClick(index)') {{snippet}}
          button.fluent.fluentButton(type='button' @click.stop.prevent='onRemoveSnippet(index)') Удалить

  .Dispatcher__logs
    label Логи сервера
    div
      button.fluent.fluentButton(type='button' @click='onReset' :disabled='!logs.length') Сбросить
      button.fluent.fluentButton(type='button' @click='onRemoveLast' :disabled='!logs.length') Удалить последний
    div
      pre.fluentCode.fluentCode_fw(v-for='log of logs').
        {{log}}
</template>
<script>
import io from 'socket.io-client';
import { codemirror } from 'vue-codemirror'
export default {
  name: 'Dispatcher',
  data() {
    let server = localStorage.getItem('server');
    if (!server) server = 'http://localhost:2000';
    let snippets = localStorage.getItem('snippets');
    if (!snippets) snippets = [];
    else snippets = JSON.parse(snippets);
    const options = {
      tabSize: 2,
      mode: 'application/json',
      undoDepth: 0,
      viewportMargin: Infinity
    }
    return {
      server,
      body: `{\n  "type": ""\n}`,
      bodyOptions: options,
      logs: [],
      snippets,
      io: null,
      sample: `{\n  "type": "@@server/PING"\n}`,
      sampleOptions: Object.assign({ readOnly: true }, options)
    }
  },
  methods: {
    onConnect() {
      if (this.io) this.io.disconnect();
      this.io = io.connect(this.server);
      this.io.on('dispatch', (action) => {
        this.logs.push(action);
      });
    },
    onDispatch(e) {
      e.preventDefault();
      e.stopPropagation();
      if (!this.io) return;
      console.log(this.body);
      this.io.emit('dispatch', JSON.parse(this.body));
    },
    onReset() {
      this.logs = [];
    },
    onRemoveLast() {
      this.logs.pop();
    },
    onRemember() {
      if (!this.server) return;
      window.localStorage.setItem('server', this.server);
    },
    onSnippetAdd() {
      this.snippets.push(this.body);
      this.saveSnippets();
    },
    onSnippetClick(index) {
      this.body = this.snippets[index];
    },
    onRemoveSnippet(index) {
      const snippets = this.snippets;
      snippets.splice(index, 1);
      this.snippets = snippets;
      this.saveSnippets();
    },
    saveSnippets() {
      window.localStorage.setItem('snippets', JSON.stringify(this.snippets));
    }
  },
  components: { codemirror }
}
</script>
<style scoped lang='stylus'>
.Dispatcher
  width 100%
  height 100%
  &_fix-w
    width 350px
    padding 16px
  &__snippets
    pre
      color black
</style>
<style lang='stylus'>
.Dispatcher .CodeMirror
  height auto
.Dispatcher label .CodeMirror
  font-weight normal
</style>
