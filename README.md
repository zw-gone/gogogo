<template>
  <div ref="wrapper" class="relative select-none">
    <input
      type="text"
      :value="value"
      class="w-full"
      readonly
      @click="keyboardShow = true"
    >
    <div
      v-if="keyboardShow"
      class="absolute bg-white w-screen h-20 grid grid-cols-6 gap-0.5"
      :style="{ left: -wrapperLeft + 'px' }"
    >
      <div
        v-for="key in keyList.slice(0, 5)"
        :key="key"
        class="flex items-center justify-center bg-red-500 text-white"
        @click="handleKeyClick(key)"
      >{{ key }}</div>
      <div
        class="flex items-center justify-center bg-red-500 text-white"
        @click="handleKeyClick('backspace')"
      >退格</div>
      <div
        v-for="key in keyList.slice(5)"
        :key="key"
        class="flex items-center justify-center bg-red-500 text-white"
        @click="handleKeyClick(key)"
      >{{ key }}</div>
      <div
        class="flex items-center justify-center bg-red-500 text-white"
        @click="handleKeyClick('confirm')"
      >确定</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'

const props = withDefaults(defineProps<{
  value?: string;
}>(), {
  value: undefined,
})
const emit = defineEmits<{
  (e: 'update:value', val: string): void;
}>()

const wrapper = ref<HTMLElement | null>(null)
const wrapperLeft = ref(0)
onMounted(() => {
  wrapperLeft.value = wrapper.value?.getBoundingClientRect().left || 0
})
const keyboardShow = ref(false)

const keyList = [
  '1', '2', '3', '4', '5', '6', '7', '8', '9', '0',
]

const handleKeyClick = (key: string) => {
  if (key === 'backspace') {
    emit('update:value', props.value?.slice(0, -1) || '')
  } else if (key === 'confirm') {
    keyboardShow.value = false
  } else {
    emit('update:value', (props.value || '') + key)
  }
}

document.body.addEventListener('click', e => {
  if (wrapper.value && !wrapper.value.contains(e.target as Node)) {
    keyboardShow.value = false
  }
})
</script>
