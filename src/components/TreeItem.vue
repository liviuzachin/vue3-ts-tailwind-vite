<script setup lang="ts">
import { ref, computed } from 'vue'

import TreeItem from './TreeItem.vue'

interface Tree {
  name: string;
  children?: Tree[];
}

const props = defineProps<{
  model: Tree,
}>()

const open = ref(false)
const isFolder = computed(() => props.model.children && props.model.children.length)

const toggle = () => {
  if (isFolder.value) {
    open.value = !open.value
  }
}

const changeType = () => {
  if (!isFolder.value) {
    props.model.children = []
    addChild()
    open.value = true
  }
}

const addChild = () => {
  props.model.children.push({
    name: 'new stuff'
  })
}

</script>

<template>
  <div :class="{ bold: isFolder }" @click="toggle" @dblclick="changeType">
    <span>{{ model.name }}</span>
    <span v-if="isFolder">[{{open ? '-' : '+'}}]</span>
  </div>
  <ul v-show="open" v-if="isFolder">
    <TreeItem v-for="m in model.children" :model="m" />
    <li class="add">
      <button  @click="addChild">+ Add child</button>
    </li>
  </ul>
</template>
