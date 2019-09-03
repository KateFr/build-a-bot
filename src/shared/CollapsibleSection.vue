<template>
  <div>
    <div class="header">
      <span v-if="open" @click="open =false">&#x25B2; Collapse</span>
      <span v-if="!open" @click="open =true">&#x25BC; Expand</span>
    </div>
    <slot v-if="open"></slot>
  </div>
</template>
<script>
export default {
  name: 'CollapsibleSection',
  data() {
    return { open: true };
  },
};
</script>

<style scoped>
.header {
  background-color: grey;
  padding: 3px;
  cursor: pointer;
}
</style>
