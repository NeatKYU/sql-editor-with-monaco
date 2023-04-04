
<template>
  <div class='editor-wrapper'>
    <div class="monaco">
      <TabSide/>
    </div>
    <div class="grid-view">
      <Table :headers="headers" :items="body" @update-items="addItem"/>
    </div>
  </div>
</template>

<script setup lang="ts" name="App">
import TabSide from '@/components/TabSide.vue'
import Table from '@/components/Table.vue'
import { reactive, ref } from 'vue'

const headers = reactive(['이름', '키', '몸무게', 'other1', 'other2', 'other3', 'other4', 'other5', 'ohter6', 'ohter7', 'other8', 'ohter9'])

const body = reactive([
  { name: 'park', height: '121', weight: '23'},
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23213123123' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23' },
  { name: 'park', height: '121', weight: '23'},
])
const addItem = (aObj: any) => {
  body.push(aObj);
}
</script>

<style lang="scss" scoped>
.editor-wrapper {
  width: 100%;
  height: 100vh;

  .monaco {
    width: 100%;
    height: 50%;
  }
  .grid-view {
    width: 100%;
    height: 50%;

    padding: 5px;
    background-color: lightgray;
  }
}

</style>
