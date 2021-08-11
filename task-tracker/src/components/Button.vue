<template>
    <button @click="consi()"
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
    consi() {
      console.log('hedi');
    },
  },
};
</script>
