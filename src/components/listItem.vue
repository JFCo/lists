<template>
<div class="field has-addons">
    <div class="control is-expanded">
     <input class="input" type="text" v-model="todo.text" readonly>
    </div>
    <div class="control">
    <a class="button is-danger is-pulled-right" @click="$emit('remove', todo.id)">
      x
    </a>
    </div>
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
