<template>
    <div class="tool-bar">
        <button @click="handleRun">
            <font-awesome-icon :icon="['fas', 'play']" />
        </button>
        <button @click="handleSave">
            <font-awesome-icon :icon="['fas', 'floppy-disk']" />
        </button>
    </div>
</template>

<script setup lang="ts" name="ToolBar">
import { defineEmits } from 'vue'

const emits = defineEmits(['sqlRun', 'sqlSave']);

const handleRun = () => {
    emits('sqlRun')
}

const handleSave = () => {
    emits('sqlSave');
}
</script>

<style lang="scss" scoped>
.tool-bar {
    width: 100%;
    height: 30px;

    padding: 5px 10px;

    display: flex;
    align-items: center;

    gap: 5px;

    background-color: #2e2e2e;

    button {
        width: 30px;
        height: 20px;

        display: flex;
        justify-content: center;
        align-items: center;

        cursor: pointer;

        box-shadow: rgba(0, 0, 0, 0.12) 0px 1px 3px, rgba(0, 0, 0, 0.24) 0px 1px 2px;

        img {
            font-size: 11px;
        }
    }
}
</style>