<template>

<v-text-field
  v-model="$value"
  :disabled="$disabled"
  :readonly="$readonly"
  :label="noLabel ? '' : humanName"
  :error-messages="$errors[name] && $errors[name][0]"
  :required="required"
  :autofocus="autofocus"
  :suffix="suffix"
  :placeholder="placeholder"
  @keyup.enter="$submit"
  @input="onInput"
  @change="onChange"
  :name="name"
  :tabindex="tabindex"
  :messages="messages"
  :type="type"
  :append-icon="appendIconCumputed"
  :prepend-icon="prependIcon"
  :prepend-inner-icon="prependInnerIcon"
  @click:append="visible = !visible"
  :autocomplete="autocomplete"
  :hint="hint"
  :persistent-hint="persistentHint"
>
  <template v-slot:append-outer>
    <v-btn icon ref="copyButton" @click="onCopy" v-show="copyToClipboard">
      <v-icon color="green" v-if="showSuccessCopyIcon">mdi-check</v-icon>
      <transition name="fade" v-else>
      <v-icon class="icon">mdi-content-copy</v-icon>
      </transition>
    </v-btn>
  </template>
  <template v-if="htmlHint" v-slot:message="{ message }">
    <span v-html="message"></span>
  </template>
  <template v-for="(_, name) in $scopedSlots" v-slot:[name]>
    <slot :name="name" />
  </template>
</v-text-field>


</template>

<script lang="coffee">
import ClipboardJS from 'clipboard'

export default {
  vrfParent: 'input'
  props:
    messages: Array
    autofocus: Boolean
    suffix: String
    noLabel: Boolean
    toggleVisibility: Boolean
    copyToClipboard: Boolean
    autocomplete: String
    hint: String
    persistentHint: Boolean
    htmlHint: Boolean
    appendIcon: String
    prependIcon: String
    prependInnerIcon: String

  data: ->
    visible: false
    showSuccessCopyIcon: false

  mounted: ->
    new ClipboardJS(@$refs.copyButton.$el, {
      text: (trigger) => @$value
    })
  methods:
    onCopy: ->
      @showSuccessCopyIcon = true

      setTimeout(
        => @showSuccessCopyIcon = false
        1000
      )

  computed:
    type: ->
      return 'text' unless @password

      if @visible
        'text'
      else
        'password'

    appendIconCumputed: ->
      return this.appendIcon unless @toggleVisibility

      if @visible
        'mdi-eye-off'
      else
        'mdi-eye'
}


</script>

<style lang="css" scoped>
.transparent{
  opacity: 0 !important;
}

.icon{
  position: absolute!important;
  margin: 0 auto!important;
}

.fade-enter-active{
  transition: opacity 1s;
}

.fade-enter, .fade-leave-active, .fade-leave-to, .fade-leave{
  opacity: 0 !important;
}

.submit-btn{
  color: white !important;
}

.not-clickable:hover {
    cursor: unset !important;
}
</style>
