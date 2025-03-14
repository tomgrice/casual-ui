<script setup lang="ts">
import { provide } from 'vue'
import CTr from './CTr.vue'
import CTd from './CTd.vue'
import CTh from './CTh.vue'

interface TableColumn {
  title: string
  field: string
  width?: string
}
interface TableProps {
  /**
   * 是否为条纹表格
   */
  striped?: boolean
  /**
   * 表格列配置
   */
  columns?: TableColumn[]
  /**
   * 表格数据
   */
  data?: Array<Record<string, string | number>>
  /**
   * 行数据唯一键
   */
  rowKey?: string
}

const props = withDefaults(defineProps<TableProps>(), {
  columns: () => [],
  data: () => [],
  rowKey: 'id',
  striped: false,
})

provide('columns', props.columns)
</script>
<template>
  <div class="c-table" :class="[{ 'c-table--striped': striped }]">
    <table class="c-table--table">
      <colgroup>
        <col v-for="{ field } in columns" :key="field" />
      </colgroup>
      <thead>
        <c-tr>
          <c-th
            v-for="{ field, title, width } in columns"
            :key="field"
            :width="width"
          >
            <slot :name="`th-${field}`" v-bind="{ field, title }">
              {{ title }}
            </slot>
          </c-th>
        </c-tr>
      </thead>
      <tbody>
        <c-tr v-for="row in data" :key="row[rowKey]">
          <c-td v-for="({ field, width }, idx) in columns" :key="field">
            <!-- 
              @slot 自定义单元格
              @name td-[field] - 自定义对应<code>field</code>单元格渲染
              @binding {any} row - 当前行数据
              @binding {any} val - 当前单元格数据
              @binding {number} idx - 当前单元格所在行下标
              @binding {string} width - 当前单元格所在列宽度
            -->
            <slot
              :name="`td-${field}`"
              v-bind="{ val: row[field], idx, row, width }"
            >
              {{ row[field] }}
            </slot>
          </c-td>
        </c-tr>
      </tbody>
    </table>
    <div v-if="data.length === 0" class="c-table--no-data">No Data</div>
  </div>
</template>
