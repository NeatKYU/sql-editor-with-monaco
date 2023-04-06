<template>
    <div class="tab-wrapper">
        <div 
            v-for="tab, index in tabs"
            class="tab" 
            :class="activeTabId === tab.id ? 'tab-active' : ''" 
            @click="handleActive(tab.id)"
        >
            <div class="left-shape"></div>
            <span>{{ tab.label }}</span>
            <div class="remove-tab" @click="(e) => handleRemoveTab(e, tab.id)">
                <font-awesome-icon :icon="['fas', 'xmark']" />
            </div>
            <div class="right-shape"></div>
        </div>
        <div class="add-tab" @click="handleAddTab">
            <font-awesome-icon :icon="['fas', 'plus']" color="#fff" />
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
  width: 110px;
  /* padding: 0 20px 0 5px; */
  /* padding-left: 5px; */
  position: relative;

  display: flex;
  justify-content: center;
  align-items: center;
  
  color: white;

  span {
    max-width: 70px;
    display: inline-block;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-right: 10px;
  }

  cursor: pointer;

  &:hover {
    background-color: #787878;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;

    &:before,
    &:after {
        content: "";
        position: absolute;
        height: 10px;
        width: 20px;
        background-color: transparent;
    }

    &:before {
        left: -20px;
        bottom: 0;
        border-bottom-right-radius: 20px;
        box-shadow: #787878 12px 1px 0px 1px;
    }

    &:after {
        right: -20px;
        bottom: 0;
        border-bottom-left-radius: 20px;
        box-shadow: #787878 -12px 1px 0px 1px;
        z-index: 10;
    }
  }

  // tab active
  &-active {
    background-color: #2e2e2e;
    border: 1px solid #2e2e2e;
    border-bottom: none;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;

    position: relative;

    &:hover {
      background-color: #2e2e2e;

      &:before {
        box-shadow: #2e2e2e 12px 1px 0px 1px;
      }
      &:after {
        box-shadow: #2e2e2e -12px 1px 0px 1px;
      }
    }

    &:before,
    &:after {
        content: "";
        position: absolute;
        height: 10px;
        width: 20px;
        background-color: transparent;
    }

    &:before {
        left: -20px;
        bottom: 0;
        border-bottom-right-radius: 20px;
        box-shadow: #2e2e2e 12px 1px 0px 1px;
    }

    &:after {
        right: -20px;
        bottom: 0;
        border-bottom-left-radius: 20px;
        box-shadow: #2e2e2e -12px 1px 0px 1px;
        z-index: 10;
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
    /* border: 1px solid gray; */
    /* background-color: white; */

    &:hover {
        /* background-color: #e8e8e8; */
        background-color: #2e2e2e;
        transition: background-color 0.5s;
    }
    transition: background-color 0.5s;
}

.remove-tab {
  position: absolute;
  right: 2px;
  top: 2px;

  width: 15px;
  height: 15px;

  font-size: 13px;

  display: flex;
  justify-content: center;
  align-items: center;

  border-radius: 50%;
  
  cursor: pointer;

  &:hover {
    background-color: #444444;
  }
}

.monaco-wrapper {
  width: 100%;
  height: calc(100% - 65px);
}

/* .divider {
    width: 1px;
    height: 70%;

    display: flex;
    justify-content: center;
    align-items: center;

    margin: 0 10px 0 0;

    background-color: #787878;
} */
</style>