<script setup lang="ts">
import { usePageFrontmatter } from '@vuepress/client'
import type { Ref } from 'vue'
import { computed } from 'vue'

const frontmatter = usePageFrontmatter() as unknown as Ref<any>
const funcName = frontmatter.value.hooksPath.split('/').pop()
const items = computed(() => frontmatter.value.hooksInfo.children)

const getTypeDisplayString = (type: any) => {
  if (type.type === 'literal') return type.value === false ? 'false' : 'true'
  if (type.type === 'union') {
    return type.types.map(getTypeDisplayString).join(' | ')
  }
  if (!type.typeArguments || !type.typeArguments.length) {
    return type.name
  }
  return (
    type.name +
    '<' +
    type.typeArguments.map(getTypeDisplayString).join(', ') +
    '>'
  )
}
const getParamDisplayString = (parameters: any[]) => {
  return parameters
    .map(param => {
      const type = getTypeDisplayString(param.type)
      return `${param.name}: ${type}`
    })
    .join(', ')
}
</script>
<template>
  <div class="hooks-api">
    <c-list :items="items" size="sm">
      <template #item="{ item }">
        <div v-if="item.kindString === 'Interface'">
          <code> {{ item.kindString }} {{ item.name }} </code>
          {{ item.comment?.shortText }} <br />
          <div class="c-pl-lg c-py-md">
            <c-list :items="item.children">
              <template #item="{ item: propertyItem }">
                <div class="c-flex c-items-center c-gutter-x-md">
                  <code>{{ propertyItem.name }}</code>
                  <code v-if="propertyItem.kindString === 'Property'">
                    {{ getTypeDisplayString(propertyItem.type) }}
                  </code>
                  <code v-if="propertyItem.kindString === 'Method'">
                    ({{
                      getParamDisplayString(
                        propertyItem.signatures[0].parameters || []
                      )
                    }}) =>
                    {{ propertyItem.signatures[0].type.name }}
                  </code>
                  <span
                    v-if="!propertyItem.flags.isOptional"
                    class="c-text-negative"
                  >
                    *
                  </span>
                  <span v-if="propertyItem.kindString === 'Property'">
                    {{ propertyItem.comment.shortText }}
                  </span>
                  <span v-else-if="propertyItem.kindString === 'Method'">
                    {{ propertyItem.signatures[0].comment.shortText }}
                  </span>
                </div>
              </template>
            </c-list>
          </div>
        </div>
        <div v-else-if="item.kindString === 'Function'">
          <code>
            function {{ funcName }} ({{
              getParamDisplayString(item.signatures[0].parameters || [])
            }}): {{ item.signatures[0].type.name }}
          </code>
          {{ item.signatures[0].comment.shortText }}
        </div>
        <div v-else-if="item.kindString === 'Type alias'">
          <code> {{ item.name }} </code>:
          <code>{{
            getTypeDisplayString(item.type.declaration.signatures[0].type)
          }}</code>
        </div>
      </template>
    </c-list>
  </div>
</template>
<style lang="scss" scoped>
.hooks-api {
  background-color: var(--casual-light-bg);
}
</style>
