<template>
  <div id="app">
    <div class="row no-gutters">
    <!-- 選擇所在區域 -->
      <div class="toolbox col-sm-3 p-2 bg-white">
        <div class="form-group d-flex">
          <select id="cityName" class="form-control" v-model="select.city">
            <option value="">請選擇縣市</option>
            <option :value="c.CityName" v-for="c in cityName" :key="c.CityName">
              {{ c.CityName }}
            </option>
          </select>
        </div>

        <div class="form-group d-flex">
          <select id="area" class="form-control" v-model="select.area">
            <option value="">請選擇地區</option>
            <option :value="a.AreaName" v-for="a in
            cityName.find((city) => city.CityName === select.city).AreaList" :key="a.AreaName">
              {{ a.AreaName }}
            </option>
          </select>
        </div>
      </div>

      <!-- 顯示藥局位置 -->
      <div class="col-sm-9">
        <div id="map"></div>
      </div>
    </div>
  </div>

</template>

<script>
// L 代表 Leaflet
import L from 'leaflet';

// 導入內部檔案
import cityName from './assets/cityName.json';

// 設定空物件
let openStreetMap = {};

export default {
  name: 'App',
  // 製作元件
  data: () => ({
    // 空陣列 data
    data: [],
    cityName,
    select: {
      // 筆者有改過下載的區域資料，故 city 是簡寫的台北市，正常下載是臺北市
      city: '臺北市',
      area: '中正區',
    },
  }),
  computed: {
    pharmacies() {
      return this.data.filter((pharmacy) => {
        if (!this.select.area) {
          return pharmacy.properties.county === this.select.city;
        }
        return pharmacy.properties.town === this.select.area;
      });
    },
  },
  watch: {
    pharmacies(value) {
      console.log(value);
    },
  },
  components: {
  },
  mounted() {
    const api = 'https://raw.githubusercontent.com/kiang/pharmacies/master/json/points.json';
    this.$http.get(api).then((response) => {
      // 將結果印出來看看
      console.log(response);
      // 將空陣列 data 指定為遠端獲得的資料，資料僅需取得藥局資訊即可
      this.data = response.data.features;
    });

    openStreetMap = L.map('map', {
      center: [25.0313775, 121.5148631],
      // 可以嘗試改變 zoom 的數值
      // 筆者嘗試後覺得 18 的縮放比例是較適當的查詢範圍
      zoom: 19,
    });

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 19,
    }).addTo(openStreetMap);
  },
};
</script>

<style lang="scss">
  @import 'bootstrap/scss/bootstrap';

  #map {
    position: relative;
    height: 100vh;
  }
</style>
