<script setup >
import { useElementSize } from '@vueuse/core'
const editor = ref()
const { width, height } = useElementSize(editor)

const originalText = ref("original")
const modifiedText = ref("modified")

</script>

<template>
    <div style="height:30vh">
        <div ref="editor" style="height:100%; background-color: #90A4AE;">
            <monaco-diff :original="originalText" v-model="modifiedText" :height="height" :width="width" />
        </div>
    </div>
    <button @click="modifiedText += 'update'">UpdateText</button>
</template>

<style>

</style>