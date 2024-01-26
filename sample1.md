```vue
<template>
  <div class="flex justify-center gap-12">
    <div class="mt-10 w-3/5">
      <div class="flex flex-col gap-3 bg-gray-200 p-4">
        <div class="flex items-center gap-3">
          <label for="number" class="inline-block w-20 mr-6
                                 font-bold text-gray-600 whitespace-nowrap">교과분류</label>
          <div class="flex justify-between gap-3 w-full">
            <select id="countries"
                    class="w-1/4 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5"
                    v-model="store.contentL"
                    @change="store.subjectChange('contentM', 'contentS', 'topic', 'achivStd')">
              <option value="" selected>대단원</option>
              <option v-for="(list, index) in store.subjectLists.contentL" :key="index" :value="list.contentLId">
                {{ list.contentL }}
              </option>
            </select>
            <select id="countries"
                    class="w-1/4 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5"
                    v-model="store.contentM" :disabled="!store.contentL"
                    @change="store.subjectChange('contentS', 'topic', 'achivStd')">
              <option value="" selected>중단원</option>
              <option v-for="(list, index) in store.subjectLists.contentM" :key="index" :value="list.contentMId"
                      v-show="store.contentL === list.contentMParent">{{ list.contentM }}
              </option>
            </select>
            <select id="countries"
                    class="w-1/4 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5"
                    v-model="store.contentS" :disabled="!store.contentM"
                    @change="store.subjectChange('topic', 'achivStd')">
              <option value="" selected>소단원</option>
              <option v-for="(list, index) in store.subjectLists.contentS" :key="index" :value="list.contentSId"
                      v-show="store.contentM === list.contentSParent">{{ list.contentS }}
              </option>
            </select>
            <select id="countries"
                    class="w-1/4 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5"
                    v-model="store.topic" :disabled="!store.contentL"
                    @change="store.subjectChange('achivStd')">
              <option value="" selected>차시</option>
              <option v-for="(list, index) in store.subjectLists.topic" :key="index" :value="list.topicId"
                      v-show="(store.contentL === list.topicParent && store.contentM === '' && store.contentS === '') || (store.contentM === list.topicParent && store.contentS === '') || store.contentS === list.topicParent">
                {{ list.topicName }}
              </option>
            </select>
          </div>
        </div>
        <div class="flex items-center gap-3">
          <label for="number" class="inline-block w-20 mr-6
                                 font-bold text-gray-600 whitespace-nowrap">성취기준</label>
          <div class="flex justify-between gap-3 w-full">
            <select id="countries"
                    class="w-full border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5"
                    v-model="store.achivStd" :disabled="!store.topic">
              <option value="" selected>성취기준</option>
              <option v-for="(list, index) in store.subjectLists.achivStd" :key="index" :value="list.achivStdId"
                      v-show="store.topic === list.achivTopic">{{ list.achivStdCode }} {{ list.achivStdName }}
              </option>
            </select>
            <div class="rounded-l whitespace-nowrap">
              <button type="button" class="rounded-lg bg-blue-500 px-5 py-2 text-white hover:bg-blue-600" @click="getQuestionList">
                검색
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="flex flex-col gap-3 mt-5">
        <div class="flex items-center gap-3">
          <label for="number" class="inline-block w-20 mr-6
                                 font-bold text-gray-600">시험지 명</label>
          <input id="countries"
                 class="w-96 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5">
        </div>
        <!--        <div class="flex items-center gap-3">
                  <label for="number" class="inline-block w-20 mr-6
                                         font-bold text-gray-600">시험 유형</label>
                  <select id="countries"
                          class="w-96 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5">
                    <option value="" selected></option>
                  </select>
                </div>-->
        <div class="flex items-center gap-3">
          <label for="number" class="inline-block w-20 mr-6
                                 font-bold text-gray-600">응시 시간</label>
          <input id="countries"
                 class="w-96 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5">
        </div>
      </div>
      <div class="flex justify-center mt-16">
        <button type="button" class="ml-2 rounded-lg bg-blue-500 px-8 py-1 text-white hover:bg-blue-600" @click="">등록
        </button>
        <button type="button" class="ml-2 rounded-lg bg-gray-400 px-8 py-1 text-white hover:bg-gray-500"
                @click="store.goMain">취소
        </button>
        <button type="button" class="ml-2 rounded-lg bg-blue-400 px-8 py-1 text-white hover:bg-blue-500">미리보기</button>
      </div>

      <ul role="list" class="divide-y divide-gray-100">
        <li v-for="question in selectedQuestionList" :key="question.qId"
            class="flex justify-between gap-x-6 py-5 bg-gray-200 rounded-2xl p-5 my-3">
          <div class="flex gap-x-4">
            <div class="flex-auto">
              <div class="mb-3 flex gap-2">
                <p class="text-xs font-semibold leading-6 text-gray-900 border border-gray-400 px-1 py-0.5">교과분류 : {{ question.info['subjectType'] }}</p>
                <p class="text-xs font-semibold leading-6 text-gray-900 border border-gray-400 px-1 py-0.5">성취기준 : {{ question.info['achivStdName'] }}</p>
              </div>
              <p class="font-semibold leading-6 text-gray-900 mb-3">[{{ question.info['questionType'] }}] {{ question.info['content'] }}</p>
              <div class="flex justify-between items-center">
                <p class="text-xs font-semibold leading-6 text-gray-900 border border-gray-400 px-1 py-0.5">난이도 : {{ question.info['learnLvl'] }}</p>
                <div class="flex items-center gap-3">
                  <p class="text-xs font-semibold leading-6 text-gray-900">점수배점</p>
                  <input id="countries"
                         class="w-16 h-8 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5" v-model="question.info['score']">
                </div>
              </div>
            </div>
          </div>
          <div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end sm:justify-center">
            <div class="flex gap-1">
              <button>수정</button>
              <span>|</span>
              <button @click="removeSelectedQuestion(question.qId)">삭제</button>
            </div>
          </div>
        </li>
      </ul>

    </div>
    <div class="p-10 max-w-md bg-gray-200 overflow-y-scroll" style="height: 80vh;">
      <div class="flex items-center gap-3 mb-4">
        <h2 class="font-bold">문항 결과</h2>
        <span class="text-gray-600 bg-yellow-400 rounded-lg px-1 py-0.5 text-sm">문항타입:전체</span>
        <span class="text-gray-600 bg-yellow-400 rounded-lg px-1 py-0.5 text-sm">난이도:전체</span>
      </div>
      <div class="h-36 border-b-2 border-dotted border-gray-500 mb-3 flex flex-col justify-center"
           v-for="(question, index) in questionList" :key="index">
        <span class="mb-2 inline-block">{{ question['content'] }}</span>
        <div class="mb-5">
          <p class="text-xs text-white bg-blue-600 px-3 py-2 rounded-xl">교과분류: {{ question['subjectType'] }}</p>
        </div>
        <div class="flex justify-between items-center">
          <span class="font-bold text-orange-500">{{ question['questionType'] }}</span>
          <span>난이도: {{ question['learnLvl'] }}</span>
          <button type="button" class="ml-2 rounded-lg bg-gray-500 px-2 py-1 text-white hover:bg-gray-600" @click="selectQuestion(question['questionId'])" :disabled="selectedQuestionList.some(obj => obj.qId === question.questionId)"
          >
            문항 추가
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useTestPaperStore } from '@/stores/testPaper/testPaperStore'
import axios from 'axios'
import { PROXY_URL } from '@/config'
import { ref } from 'vue'

const store = useTestPaperStore()

store.title = '시험지 등록'
store.breadCrumb = ['콘텐츠 관리', '문제은행', '시험지 관리', '시험지 등록']

const questionList = ref()

const selectedQuestionList = ref([])

const getQuestionList = async () => {

  if (selectedQuestionList.value.length >= 1) {
    alert('선택된 문항을 모두 삭제 후 검색해주세요.')
    return
  }

  if (store.achivStd === '') {
    alert('성취기준을 선택 후 검색해주세요.')
    return
  }

  try {
    const params = {
      contentLId: store.contentL,
      contentMId: store.contentM,
      contentSId: store.contentS,
      topicId: store.topic,
      achivStdId: store.achivStd,
      firstIndex: 1,
      recordCountPerPage: 50
    }

    const result = await axios.get(`${PROXY_URL}/admin-service/question.do`, { params })
    questionList.value = result.data

  } catch (e) {
    console.error(e)
  }
}

const selectQuestion = (qId) => {
  const question = questionList.value.filter(question => {
    return question.questionId === qId
  })
  question[0].score = null
  selectedQuestionList.value.push({ qId: qId, info: question[0] })
}

const removeSelectedQuestion = (qId) => {
  selectedQuestionList.value = selectedQuestionList.value.filter(obj => obj['qId'] !== qId)
}
</script>
```