<template>
    <div class="tab-wrapper">
        <div 
            v-for="tab, index in tabs"
            class="tab" 
            :class="activeTabId === tab.id ? 'tab-active' : ''" 
            @click="handleActive(tab.id)"
        >
            <span>{{ tab.label }}</span>
            <div class="remove-tab" @click="(e) => handleRemoveTab(e, tab.id)">
                <img src="@/assets/cancel.svg" />
            </div>
        </div>
        <div class="add-tab" @click="handleAddTab">
            <img src="@/assets/plus.svg"/>
        </div>
    </div>
    <div class="tool-bar-wrapper">
        <Toolbar @sqlRun="sqlRun" @sqlSave="sqlSave"/>
    </div>
    <div v-for="tab in tabs" :class="tab.id === activeTabId ? 'monaco-wrapper': ''">
        <Monaco 
            v-if="tab.id === activeTabId" 
            :content="tab.contents" 
            @changeContent="changeContent" 
            @selectContent="selectContent"
        />
    </div>
</template>

<script setup lang="ts" name="TabSide">
import Monaco from '@/components/Monaco.vue'
import Toolbar from '@/components/Toolbar.vue'
import { reactive, ref } from 'vue'

const activeTabId = ref(0);
const tabId = ref(0);
const tabs = reactive([
    { id: 0, label: 'monaco', contents: '', select: ''}
])
const handleActive = (id: number) => {
    activeTabId.value = id
}
const handleAddTab = () => {
    tabId.value++;
    const newTab = { id: Date.now(), label: 'new editor' + tabId.value, contents: '', select: ''};
    activeTabId.value = newTab.id;
    tabs.push(newTab)
}
const handleRemoveTab = (e: MouseEvent, removeId: number) => {
    e.preventDefault();
    // const modifyTabs = tabs.filter((item) => item.id !== removeIndex);
    tabs.splice(tabs.findIndex((item) => item.id === removeId), 1)
}
const changeContent = (value: string) => {
    tabs.map((item) => {
        if (item.id === activeTabId.value) {
            item.contents = value;
        }
    })
}
const selectContent = (value: string) => {
    tabs.map((item) => {
        if (item.id === activeTabId.value) {
            item.select = value;
        }
    })
}
const sqlRun = () => {
    const tab = tabs[tabs.findIndex((item) => item.id === activeTabId.value)];
    if (window.getSelection()?.toString()) {
        console.log('select text = ', tab.select)
    } else {
        console.log('contents = ', tab.contents)
    }
}
const sqlSave = () => {
    
}
</script>

<style lang="scss" scoped>
.tab-wrapper {
  width: 100%;
  height: 35px;
  display: flex;

  background-color: rgb(72, 72, 72);

  /* padding: 0 20px; */

  overflow-x: scroll;
  overflow-y: hidden;
}
.tab-wrapper::-webkit-scrollbar {
  /* width: 2px; */
  height: 1px;
}
.tab-wrapper::-webkit-scrollbar-thumb {
  background-color: rgb(118, 113, 113);
}

.tab {
  /* min-width: 100px; */
  width: 100px;
  padding: 0 20px 0 5px;

  position: relative;

  display: flex;
  justify-content: start;
  align-items: center;
  
  color: white;
  
  span {
    display: inline-block;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  cursor: pointer;

  &:hover {
    background-color: #787878;
  }

  &-active {
    background-color: #2e2e2e;
    border: 1px solid #2e2e2e;
    border-bottom: none;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;

    &:hover {
      background-color: #575757;
    }
  }
}
.add-tab {
  width: 30px;
  margin: 5px;

  display: flex;
  justify-content: center;
  align-items: center;

  cursor: pointer;

  border-radius: 5px;
  border: 1px solid gray;
  background-color: white;

  &:hover {
    background-color: #e8e8e8;
  }
}

.remove-tab {
  position: absolute;
  right: 0;
  top: 0;

  width: 15px;
  height: 12px;

  display: flex;
  justify-content: center;
  align-items: center;
  
  cursor: pointer;
}

.monaco-wrapper {
  width: 100%;
  height: calc(100% - 65px);
}
</style>