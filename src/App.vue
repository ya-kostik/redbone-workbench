<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
require('./fonts/PT_Sans-Web-Bold.ttf');
require('./fonts/PT_Sans-Web-Regular.ttf');
export default {
  name: 'app'
}
</script>

<style lang='stylus'>
$color-fluent = #f3f6fb;
body
  margin 0
  padding 0
  font-size 14px
  & *
    font-family: 'PT Sans', sans-serif;
  pre
    font-family: 'Menlo', monospace;
    font-size 12px
.block
  display block
.fluent
  border none
  background darken($color-fluent, 4%)
  color darken($color-fluent, 45%)
  border-radius 4px
  padding 4px 8px
  outline none
  font-weight normal
  &_fw
    width 100%
    display block
  &_transparent
    background transparent
  &:focus
    padding 2px 6px
    border 2px solid darken($color-fluent, 10%)

.fluentLabel
  display block
  &_fw
    width 100%

$disabled = darken(white, 15%);

.fluentButton
  min-width 28px
  min-height 28px
  padding-bottom 2px
  border-bottom 2px solid darken($color-fluent, 15%)
  cursor pointer
  &:active
    border-bottom 2px solid darken($color-fluent, 4%)
  &_sq
    position relative
    .fa
      position relative
      top 2px
  &:disabled
    cursor default
    background-color $disabled
    color darken($disabled, 25%);
    border-bottom 2px solid darken($disabled, 15%)

.fluentArea
  max-width 100%
.fluentCode
  background-color $color-fluent
  padding 8px
  font-family 'Menlo', monospace;
  & .CodeMirror *
    font-size 12px
    font-family 'Menlo', monospace;
  &_fw
    display block
    width 100%
label
  font-weight bold
</style>
