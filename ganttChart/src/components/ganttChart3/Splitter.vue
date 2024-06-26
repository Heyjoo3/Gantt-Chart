<template>
    <div class="splitter" @mousedown="onMouseDown"><p>&nbsp;</p></div>
  </template>
  
  <script>
  export default {
    methods: {
      onMouseDown(e) {
        this.$emit('drag-start', e);
      }
    }
  }
  </script>
  
  <style scoped>
  .splitter {
    width: 10px;
    background-color: #bedd2c;
    cursor: ew-resize;
    height: 100%;
    min-height: 100%;
    position: relative;
    z-index: 10;
  }
  </style>