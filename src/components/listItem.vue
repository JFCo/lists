<template>
<div class="field has-addons">
    <div class="control is-expanded">
     <input class="input" type="text" v-model="todo.text">
    </div>
    <div class="control">
    <a class="button is-warning is-pulled-right" @click="$emit('remove', todo.id)">
      <i class="fa fa-trash" aria-hidden="true"></i>
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
