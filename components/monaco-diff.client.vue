<template>
  <div ref="editorElement" id="editorElement" >
  </div>
</template>

<script lang="ts" setup>
import * as monaco from 'monaco-editor'
import type * as Monaco from 'monaco-editor'

interface Props {
  original?: string;
  modelValue?: string;
  height?: number;
  width?: number;
}

const props = withDefaults(defineProps<Props>(), {
  original: () => '',
  modelValue: () => '',
  height: () => 100,
  width: () => 100,
})

const editorElement = ref()

let editor: Monaco.editor.IStandaloneDiffEditor
let originalModel: Monaco.editor.ITextModel
let modifiedModel: Monaco.editor.ITextModel


watch(() => [props.original, props.modelValue], () => {
  if (originalModel.getValue() !== props.original || modifiedModel.getValue() !== props.modelValue) {
    originalModel.setValue(props.original)
    modifiedModel.setValue(props.modelValue)
  }
})

watch(() => [props.width, props.height], () => {
  editor.layout({width: props.width, height: props.height})
})

onMounted (async () => {
  await nextTick()

//   const myThemeColors = {
//   "diffEditor.insertedTextBackground": "#0000",
//   "diffEditor.removedTextBackground": "#325ea8",
// };

// // Define your new theme
// monaco.editor.defineTheme("my-theme", {
//   base: "vs-dark",
//   colors: myThemeColors,
//   rules:[],
//   inherit: false
// });


  if(editorElement.value){
  originalModel = monaco.editor.createModel(props.original, 'xml')
  modifiedModel = monaco.editor.createModel(props.modelValue, 'xml')

  editor = monaco.editor.createDiffEditor(editorElement.value, {theme: "vs-dark", readOnly: true})
  editor.setModel({
    original: originalModel,
    modified: modifiedModel
  })
  }
})
</script>


