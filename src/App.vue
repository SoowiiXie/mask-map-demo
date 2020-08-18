<template>
    <div id="app">
      <div class="row no-gutters">
      <!-- 選擇所在區域 -->
        <div class="toolbox col-sm-3 p-2 bg-white">
          <div class="form-group d-flex">
            <label for="cityName" class="col-form-label mr-2 text-right">縣市</label>
            <div class="flex-fill">
              <select id="cityName" class="form-control"></select>
            </div>
          </div>
          <div class="form-group d-flex">
            <label for="area" class="col-form-label mr-2 text-right">地區</label>
            <div class="flex-fill">
              <select id="area" class="form-control"></select>
            </div>
          </div>
        </div>

      <!-- 顯示藥局位置 -->
      <div class="col-sm-9">
        <div id="map"></div>
      </div>
    </div>
  </div>

  <!-- // 選擇縣市選單 -->
  <select id="cityName" class="form-control" v-model="select.city">
    <option value="">請選擇縣市</option>
    <option :value="c.CityName" v-for="c in cityName" :key="c.CityName">
      {{ c.CityName }}
    </option>
  </select>

  <!-- // 選擇地區選單 -->
  <select id="area" class="form-control" v-model="select.area">
    <option value="">請選擇地區</option>
    <!-- // 建立專案時若有選擇 Airbnb 相關規範設定的話
    // 需留意過長的程式碼會造成 line length error，換行即可解決此問題 -->
    <option :value="a.AreaName" v-for="a in cityName.find((city) => city.CityName === select.city).AreaList" :key="a.AreaName">
      {{ a.AreaName }}
    </option>
  </select>
</template>

<script>
// 導入內部檔案
import cityName from './assets/cityName.json';

export default {
  name: 'App',
  // 製作元件
  data: () => ({
    cityName,
    // select 先暫時設定為空物件
    select: {},
  }),
  // ...
};
</script>

<style lang="scss">
  @import 'bootstrap/scss/bootstrap';

  #map {
    position: relative;
    height: 100vh;
  }
</style>
