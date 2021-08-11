<template>
    <button @click="displayForm()"
    :style="{background: color}"
    class="btn">{{text}}
    </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text: String,
    color: String,
  },
  methods: {
    displayForm() {
      this.$emit('btn-click');
    },
  },
};
</script>
