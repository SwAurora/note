```js
import { defineStore } from 'pinia'
import { ref } from 'vue'
import axios from 'axios'
import { PROXY_URL } from '@/config'

export const useTestPaperStore = defineStore('testPaperStore', () => {
  const pageNum = ref(1)
  const title = ref('시험지 관리')
  const breadCrumb = ref(['콘텐츠 관리', '문제은행', '시험지 관리'])
  const subjectTypes = ref({ contentLId: null })
  const subjectLists = {
    contentL: ref([]),
    contentM: ref([]),
    contentS: ref([]),
    topic: ref([]),
    achivStd: ref([])
  }
  const questionType = ref()
  const selectedQuestionType = ref('')
  const contentL = ref('')
  const contentM = ref('')
  const contentS = ref('')
  const topic = ref('')
  const achivStd = ref('')
  const learnLvl = ref('')

  const goMain = () => {
    pageNum.value = 1
    title.value = '시험지 관리'
    breadCrumb.value.pop()
    reset()
  }

  const reset = () => {

  }

  const subjectChange = (...fieldsToReset) => {
    fieldsToReset.forEach(field => {
      useTestPaperStore()[field] = ''
    })
  }

  const getQuestionType = async () => {
    try {
      const result = await axios.get(`${PROXY_URL}/admin-service/getQuestionType.do`)
      questionType.value = result.data
    } catch (e) {
      console.error(e)
    }
  }

  const getSubjectTypes = async () => {
    try {
      const result = await axios.get(`${PROXY_URL}/admin-service/getSubjectTypes.do`)
      subjectTypes.value = result.data

      const tmpSets = {
        contentL: new Set(),
        contentM: new Set(),
        contentS: new Set(),
        topic: new Set(),
        achivStd: new Set()
      }

      const addToSetAndList = (list, set, key, value) => {
        if (!set.has(value[key])) {
          set.add(value[key])
          list.value.push(value)
        }
      }

      for (const item of subjectTypes.value) {
        addToSetAndList(subjectLists.contentL, tmpSets.contentL, 'contentLId', {
          contentLId: item.contentLId,
          contentL: item.contentL
        })

        addToSetAndList(subjectLists.contentM, tmpSets.contentM, 'contentMId', {
          contentMId: item.contentMId,
          contentM: item.contentM,
          contentMParent: item.contentMParent
        })

        addToSetAndList(subjectLists.contentS, tmpSets.contentS, 'contentSId', {
          contentSId: item.contentSId,
          contentS: item.contentS,
          contentSParent: item.contentSParent
        })

        addToSetAndList(subjectLists.topic, tmpSets.topic, 'topicId', {
          topicId: item.topicId,
          topicName: item.topicName,
          topicParent: item.topicParent
        })

        if (item.achivStdId && item.achivStdName.trim() !== '') {
          addToSetAndList(subjectLists.achivStd, tmpSets.achivStd, 'achivStdId', {
            achivStdId: item.achivStdId,
            achivStdName: item.achivStdName,
            achivStdCode: item.achivStdCode,
            achivTopic: item.achivTopic
          })
        }
      }
    } catch (e) {
      console.error(e)
    }
  }

  return {
    pageNum,
    title,
    breadCrumb,
    goMain,
    subjectChange,
    subjectLists,
    questionType,
    selectedQuestionType,
    contentL,
    contentM,
    contentS,
    topic,
    achivStd,
    learnLvl,
    getQuestionType,
    getSubjectTypes
  }
})
```