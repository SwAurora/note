# VUE 구조

```vue

<script>
  export default {
    data() {
      return {
        message: 'Hello World!',
        ttt: '',
      }
    },
    methods: {
      test: function () {
        console.log('test')
      }
    },
    mounted: function (string) { /* document.ready() */
      this.test(string)
      console.log(this.$options.filters.plus('test'))
    },
    created: { // 제일 먼저 동작 (created -> DOM이 mount -> mounted)
        
    },
    watch: {
        message: {
            handler: function (value, oldValue) {
                // 로그 찍어보거나 computed와 비슷하게 사용
                // data가 변하는걸 감시
                // computed는 해당 데이터 안의 요소 감시 watch는 본인 감시
            },
            deep: true // 깊은 객체도 감시
        }
    },
    filter: {
        plus: function (value) {
          return value + "!";
        }
    }
  }
</script>
<template>
  <h1>{{ message | plus }}</h1>
</template>
<style>

</style>
```