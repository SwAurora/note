# vue 체크박스(computed)

```vue
<template>
    <input type="checkbox" class="form-check-input" v-model="allChecked">
    
    <tr v-for="about in abouts">
      <td><input type="checkbox" class="form-check-input"
                 v-model="checkedAbouts" v-bind:value="about"></td>
    </tr>
</template>

<script>
  export default {
    data() {
      return {
        abouts : [],
        checkedAbouts : [],
      }
    },
    computed: { // 계속 돌고있으면서 안의 데이터의 값이 변하면 그때그때 체크해서 동작
      allChecked: {
        // getter
        get: function () {
          return this.abouts.length === this.checkedAbouts.length;
        },

        // setter
        set: function (e) {
          return this.checkedAbouts = e ? this.abouts : [];
        }
      }
    },
  }
</script>
```