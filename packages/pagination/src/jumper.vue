<template>
  <span class="el-pagination__jump">
    {{ t('el.pagination.goto') }}
    <el-input
      class="el-pagination__editor is-in-pagination"
      :min="1"
      :max="pageCount"
      :disabled="disabled"
      :model-value="innerValue"
      type="number"
      @update:modelValue="handleInput"
      @change="handleChange"
    />
    {{ t('el.pagination.pageClassifier') }}
  </span>
</template>

<script lang="ts">
import {
  computed,
  defineComponent,
  ref,
} from 'vue'
import { t } from '@element-plus/locale'
import { Input as ElInput } from '@element-plus/input'
import { usePagination } from './usePagination'

export default defineComponent({
  components: {
    ElInput,
  },
  setup() {
    const { pagination, pageCount, disabled, currentPage } = usePagination()
    const userInput = ref<Nullable<number>>(null)
    const innerValue = computed(() => userInput.value ?? currentPage.value)

    function handleInput(val: number | string) {
      userInput.value = Number(val)
    }

    function handleChange(val: number | string) {
      pagination?.changeEvent(Number(val))
      userInput.value = null
    }

    return {
      t,
      userInput,
      pageCount,
      disabled,
      handleInput,
      handleChange,
      innerValue,
    }
  },
})
</script>
