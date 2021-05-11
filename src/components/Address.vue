<template>
  <!--居住地址三级联动选项-->
  <section class="showChose" v-show="showChose">
    <section class="address">
      <section class="title">
        <h4>居住地址</h4>
        <span @click="closeAdd()" class="close">×</span>
      </section>
      <section class="title">
        <div class="area" @click="provinceSelected()">{{ Province ? Province : '' }}</div>
        <div class="area" @click="citySelected()" :class="City ? '' : 'active'">{{ City ? City : '请选择' }}</div>
        <div class="area" @click="districtSelected()" :class="District ? '' : 'active'" v-show="City">
          {{ District ? District : '请选择' }}
        </div>
      </section>
      <ul>
        <li
          class="addList"
          v-for="(v, k) in info"
          @click="getProvinceId(v.id, v.name, k)"
          v-show="showProvince"
          :class="v.selected ? 'active' : ''"
          :key="v.name"
        >
          {{ v.name }}
        </li>
        <li
          class="addList"
          v-for="(v, k) in showCityList"
          @click="getCityId(v.id, v.name, k)"
          v-show="showCity"
          :class="v.selected ? 'active' : ''"
          :key="v.name"
        >
          {{ v.name }}
        </li>
        <li
          class="addList"
          v-for="(v, k) in showDistrictList"
          @click="getDistrictId(v.id, v.name, k)"
          v-show="showDistrict"
          :class="v.selected ? 'active' : ''"
          :key="v.name"
        >
          {{ v.name }}
        </li>
      </ul>
    </section>
  </section>
</template>
<script>
import data from '../data/address.json';
export default {
  name: 'myAddress',
  data() {
    return {
      showProvince: true,
      showCity: false,
      showDistrict: false,
      showCityList: false,
      showDistrictList: false,
      province: 1,
      city: 3,
      district: 57,
      GetProvinceId: 2,
      District: false,
      Province: false,
      City: false,
      // v-for循环判断是否为当前
      selected: false,
      info: data,
    };
  },
  props: {
    showChose: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    closeAdd() {
      this.$emit('closeAddress', false);
    },
    _filter(add, name, code) {
      let result = [];
      for (let i = 0; i < add.length; i++) {
        if (code == add[i].id) {
          result = add[i][name];
        }
      }
      return result;
    },
    getProvinceId(code, input, index) {
      this.province = code;
      this.Province = input;
      this.showProvince = false;
      this.showCity = true;
      this.showDistrict = false;
      this.showCityList = this._filter(this.info, 'city', this.province);
      // 点击选择当前
      this.info.map((a) => (a.selected = false));
      this.info[index].selected = true;
    },
    provinceSelected() {
      // 清除市级和区级列表
      this.showCityList = false;
      this.showDistrictList = false;
      // 清除市级和区级选项
      this.City = false;
      this.District = false;
      // 选项页面的切换
      this.showProvince = true;
      this.showCity = false;
      this.showDistrict = false;
    },
    getCityId(code, input, index) {
      this.city = code;
      this.City = input;
      this.showProvince = false;
      this.showCity = false;
      this.showDistrict = true;
      this.showDistrictList = this._filter(this.showCityList, 'district', this.city);
      // 选择当前添加active
      this.showCityList.map((a) => (a.selected = false));
      this.showCityList[index].selected = true;
    },
    citySelected() {
      this.showProvince = false;
      this.showCity = true;
      this.showDistrict = false;
    },
    getDistrictId(code, input, index) {
      this.district = code;
      this.District = input;
      // 选择当前添加active
      this.showDistrictList.map((a) => (a.selected = false));
      this.showDistrictList[index].selected = true;
      // 选取市区选项之后关闭弹层
      // this.closeAdd();
      console.log(this.province, this.Province, this.city, this.City, this.district, this.District);
    },
    districtSelected() {
      this.showProvince = false;
      this.showCity = false;
      this.showDistrict = true;
    },
  },
};
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  list-style: none;
}
.showChose {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 120;
  background: rgba(77, 82, 113, 0.8);
}
.address {
  position: absolute;
  bottom: 0;
  left: 0;
  z-index: 121;
  background: #fff;
  width: 100%;
}
.title {
  position: relative;
}

.title h4 {
  display: block;
  font-size: 0.32rem;
  line-height: 0.88rem;
  font-weight: normal;
  color: #000;
  text-align: center;
}
.title span.close {
  font-size: 0.6rem;
  line-height: 0.6rem;
  width: 0.6rem;
  color: #d8d8d8;
  position: absolute;
  right: 5px;
  top: 0.175rem;
  text-align: center;
}
.area {
  display: inline-block;
  font-size: 0.24rem;
  line-height: 0.88rem;
  margin-left: 0.42rem;
  color: #333;
}
.addList {
  width: 100%;
  padding-left: 0.32rem;
  font-size: 0.34rem;
  line-height: 0.88rem;
  color: #333;
}
/* 修改的格式 */
.address ul {
  width: 95%;
  height: 100%;
  max-height: 4.4rem;
  overflow: auto;
}
.address ul li {
  margin-left: 5%;
}
.address .title .active {
  color: #0071b8;
  border-bottom: 0.02rem solid #0071b8;
}
.address ul .active {
  color: #0071b8;
}
</style>
