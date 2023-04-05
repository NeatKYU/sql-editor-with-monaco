<template>
    <!-- theme="vs" -->
    <div class="monaco-inner-wrapper">
        <MonacoEditor
            ref="monacoRef"
            theme="vs"
            :options="options"
            language="sql"
            v-model:value="editorStr"
            @change="(value: string) => { handleChange(value) }"
            @mouseup="handleSelectText"
        />
        <!-- <button :onClick="handleRun">run</button> -->
    </div>
</template>
  
<script setup lang="ts" name="Monaco">
import MonacoEditor from 'monaco-editor-vue3';
import { ref, defineEmits, defineProps } from 'vue';

const props = defineProps({
    content: {
        type: String,
        default: '',
    }
})
const editorStr = ref(props.content);
const monacoRef = ref();
const emits = defineEmits(['changeContent', 'selectContent']);

const scrollOptions = {
    // Scroll Options
    // Render vertical arrows. Defaults to false.
    // verticalHasArrows: false,   
    // Render horizontal arrows. Defaults to false.
    // horizontalHasArrows: false,
    verticalScrollbarSize: 5,
    horizontalScrollbarSize: 5,
    // arrowSize: 0,

    horizontal: 'hidden',

    verticalScrollbarBackground: '#000'
}

const options = {
    colorDecorators: true,
    lineHeight: 24,
    tabSize: 2,

    scrollbar: scrollOptions,
}

const handleChange = (value: string) => {
    editorStr.value = value;
    emits('changeContent', value);
}

const handleSelectText = () => {
    const selectText = window.getSelection()?.toString();
    emits('selectContent', selectText)
}

// const handleRun = () => {
//     console.log(editorStr.value)
// }

// import * as monaco from "monaco-editor";
// import { ref, onMounted, onBeforeUnmount, defineProps } from 'vue'

// const props = defineProps({
//     code: '',
// })
// const editorContainer = ref<any>(null);
// let editor: any;

// onMounted(() => {
//     editor.value = monaco.editor.create(editorContainer.value, {
//         value: 'hello world',
//         language: "typescript",
//         theme: "vs-dark",
//     })
//     editor.onDidChangeModelContent(() => {
//         // emit change value
//         // this.$emit("change", this.editor.getValue());
//     })
// })

// onBeforeUnmount(() => {
//     editor.dispose();
// })

</script>
  
<style lang="scss" scoped>
button {
    position: absolute;
    bottom: 1rem;
    right: 1.5rem;
    width: 100px;
    height: 30px;
    color: white;
    background-color: black;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    &:hover {
        background-color: rgb(58, 58, 58);
    }
}
.monaco-inner-wrapper {
    width: 100%;
    height: 100%;
    
    position: relative;
}
/* Make horizontal scrollbar, decorations overview ruler and vertical scrollbar arrows opaque */
.monaco-editor .monaco-scrollable-element .scrollbar.horizontal,
.monaco-editor .decorationsOverviewRuler,
.monaco-editor
	.monaco-scrollable-element
	.scrollbar.vertical
	.arrow-background {
	background: rgba(23, 23, 23, 255);
}
/* Make vertical scrollbar transparent to allow decorations overview ruler to be visible */
.monaco-editor .monaco-scrollable-element .scrollbar.vertical {
	background: rgb(250, 54, 54);
}
</style>