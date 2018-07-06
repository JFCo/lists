<template>
<div class="panel-block">
  <p class="control is-expanded">
    {{ todo.text }}
    <button class="button is-danger is-pulled-right" @click="$emit('remove', todo.id)">
      X
    </button>
  </p>
</div>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true
    }
  }
}
</script>
