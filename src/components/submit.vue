<template>
<v-btn
  @click="handleSubmit"
  v-if="!$disabled"
  v-bind="props"
>
  <div :class="{'transparent': status}">
    <slot>{{humanName}}</slot>
  </div>
  <transition name="fade">
    <v-icon color="white" class="icon-success" v-if="status === 'success'">mdi-check</v-icon>
    <v-icon color="white" class="icon-success" v-if="status === 'error'">mdi-alert-circle</v-icon>
  </transition>
</v-btn>
</template>

<script lang="coffee">


pick = (object, keys) ->
  keys.reduce(
    (obj, key) ->
      if object && key of object
        obj[key] = object[key]

      obj
   {}
  )

vuetifyBooleanProps = [
  'fab'
  'large'
  'small'
  'xLarge'
  'xSmall'
  'rounded'
  'shaped'
  'depressed'
]

export default {
  vrfParent: 'submit'

  props:
    vuetifyBooleanProps.reduce(
      (props, name) ->
        props[name] = Boolean
        props
      {}
    )

  data: ->
    status: null
    color: 'primary'

  watch:
    $saving: ->
      if not @$saving
        if @$lastSaveFailed
          @status = 'error'
          @color = 'error'
        else
          @status = 'success'
          @color = 'green'

        setTimeout(
          =>
            @status = null
            setTimeout(
              => @color = 'primary'
              100
            )
          2000
        )

  computed:
    props: ->
      {
        color: @color
        loading: @$saving
        disabled: @$disabled
        class: {'not-clickable': !!@status, 'submit-btn': true}
        ...pick(@, vuetifyBooleanProps)
      }


  methods:
    handleSubmit: ->
      return if @status

      @$submit()

}

</script>

<style lang="css" scoped>
.transparent{
  opacity: 0 !important;
}

.icon-success{
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
