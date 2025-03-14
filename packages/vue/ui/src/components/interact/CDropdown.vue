<script setup lang="ts">
import { useClickOutside, useVModel } from 'casual-ui-vue'
import { toRefs, ref } from 'vue'

interface CDropdownProps {
  /**
   * 下拉是否展示，用于<code>v-model</code>绑定用
   */
  modelValue: boolean
  /**
   * 是否禁用
   */
  disabled?: boolean
  /**
   * 是否自动与默认内容保持一致宽度
   */
  widthWithinParent?: boolean
  /**
   * 是否手动控制
   */
  manual?: boolean
}

const props = withDefaults(defineProps<CDropdownProps>(), {
  disabled: false,
  widthWithinParent: true,
  manual: false,
})

const emit = defineEmits<{
  /**
   * 下拉状态变化时触发
   */
  (e: 'update:modelValue', newValue: boolean): void
}>()

const { modelValue } = toRefs(props)

const { innerValue } = useVModel(modelValue, modelValue.value, newValue => {
  emit('update:modelValue', newValue)
})

const dropdownDom = ref<HTMLDivElement | null>(null)
useClickOutside({
  elRef: dropdownDom,
  cbOutside: () => {
    if (props.disabled || props.manual) return
    innerValue.value = false
  },
  cbInside: () => {
    if (props.disabled || props.manual) return
    innerValue.value = true
  },
})
</script>
<template>
  <div
    ref="dropdownDom"
    :class="['c-dropdown', { 'c-dropdown--dropped': innerValue }]"
  >
    <div class="c-dropdown--trigger">
      <!-- @slot 默认内容 -->
      <slot />
    </div>
    <div
      :class="[
        'c-dropdown--drop-content',
        { 'c-dropdown--drop-content-auto-width': !widthWithinParent },
      ]"
    >
      <!-- @slot 下拉内容 -->
      <slot name="drop-content" />
    </div>
  </div>
</template>
