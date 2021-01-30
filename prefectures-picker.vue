<template>
  <view>
    <picker
      mode="multiSelector"
      @change="onPickerChange"
      @columnchange="onColumnChange"
      :value="multiIndex"
      :range="multiArray"
      range-key="name"
    >
      <view class="picker">
        <text v-for="index in column" :key="index">
          {{ multiArray[index][multiIndex[index]].name }}
          <text v-if="index !== column-1" class="margin"> {{separator}} </text>
        </text>
      </view>
    </picker>
  </view>
</template>

<script>
  import provinces from './province.js';
  import cities from './city.js';
  import prefectures from './prefectures.js';

export default {
  props: [
    'sep',
  ],
  data() {
    return {
      column: 3,
      multiArray: [],
      multiIndex: [0, 0, 0],
    };
  },
  computed: {
    separator() {
      return this.sep ? this.sep : '-'
    }
  },

  mounted() {
    this.init();
  },

  methods: {
    init() {
      const code = provinces[0].code;
      const city = cities[code];
      const prefecture = prefectures[city[0].code];

      this.multiArray = [provinces, city, prefecture]
      console.log('this mutli array', this.multiArray)
    },

    onPickerChange(e) {
      const arr = e.detail.value;
      const province = {...provinces[arr[0]]};
      const city = {...cities[province.code][arr[1]]};
      const prefecture = {...prefectures[city.code][arr[2]]};
      this.$emit('change', {
        province,
        city,
        prefecture
      })
    },

    onColumnChange(e) {
      const column = e.detail.column;
      const index = e.detail.value;
      
      if(column === 0) {
        const province = provinces[index];
        const city = cities[province.code];
        const prefecture = prefectures[city[0].code];
        
        this.multiArray = [provinces, city, prefecture]
        
        // 清除后面的选择
        this.$set(this.multiIndex, 1, 0);
        this.$set(this.multiIndex, 2, 0);
      } else if (column === 1) {
        const province = provinces[this.multiIndex[0]];
        const city = cities[province.code]
        
        const prefecture = prefectures[city[index].code];
        
        this.$set(this.multiIndex, 2, 0);
      } else if (column === 2) {
        console.log('column change', column, index);
      }
      
      this.$set(this.multiIndex, column, index);
    },
  },
};
</script>

<style lang="scss">
.margin {
  margin-left: 10upx;
  margin-right: 10upx;
}
</style>
