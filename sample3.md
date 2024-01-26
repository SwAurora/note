```vue
<template>
  <div class="w-5/6 m-auto">
    <div v-if="store.pageNum === 1">
      <TestPaperHeader />
      <div class="flex justify-end mt-4">
        <button class="ml-2 rounded-lg bg-blue-500 px-8 py-1 text-white hover:bg-blue-600" @click="editPage">시험지 생성
        </button>
      </div>
    </div>
    <div v-if="store.pageNum === 2">
      <TestPaperHeader />
      <TestPaperEdit />
    </div>
  </div>
</template>

<script setup>
import TestPaperEdit from '@/pages/contentMng/testPaper/TestPaperEdit.vue'
import { useTestPaperStore } from '@/stores/testPaper/testPaperStore'
import TestPaperHeader from '@/pages/contentMng/testPaper/TestPaperHeader.vue'

const store = useTestPaperStore()

store.getQuestionType()
store.getSubjectTypes()

const editPage = () => {
  store.pageNum = 2
}
</script>
```