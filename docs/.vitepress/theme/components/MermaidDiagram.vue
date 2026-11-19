<script setup lang="ts">
import { useData } from 'vitepress'
import { onMounted, ref, useId, watch } from 'vue'

const props = defineProps<{ source: string }>()
const { isDark, lang } = useData()
const svg = ref('')
const failed = ref(false)
const diagramId = useId()
let renderCount = 0

onMounted(() => {
  watch([() => props.source, isDark], async ([source, dark], _, onCleanup) => {
    let active = true
    onCleanup(() => { active = false })

    try {
      const { default: mermaid } = await import('mermaid')
      if (!active) return
      mermaid.initialize({
        startOnLoad: false,
        securityLevel: 'strict',
        theme: dark ? 'dark' : 'neutral',
        fontFamily: 'system-ui, sans-serif',
        htmlLabels: false,
        flowchart: { curve: 'linear' }
      })
      const diagram = await mermaid.render(`mermaid-${diagramId}-${++renderCount}`, source)
      if (!active) return
      svg.value = diagram.svg
      failed.value = false
    } catch (error) {
      if (!active) return
      svg.value = ''
      failed.value = true
      console.error('Unable to render Mermaid diagram', error)
    }
  }, { immediate: true })
})
</script>

<template>
  <div class="mermaid-diagram" :aria-busy="!svg && !failed">
    <div v-if="svg" v-html="svg" />
    <p v-else-if="failed" role="status">
      {{ lang.startsWith('zh') ? '图示加载失败，关系说明见下表。' : 'The diagram could not load. See the table below for the same relationships.' }}
    </p>
  </div>
</template>
