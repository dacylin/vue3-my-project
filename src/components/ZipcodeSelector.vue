<template>
  <div class="zipcode-selector">
    <!-- 郵遞區號輸入框 -->
    <label>郵遞區號：</label>
    <input
      type="text"
      v-model="zipcodeInput"
      @input="onZipcodeInput"
      maxlength="3"
      placeholder="輸入郵遞區號"
    />

    <!-- 縣市下拉選單 -->
    <label>縣市：</label>
    <select v-model="selectedCity" @change="onCityChange">
      <option value="" disabled>選擇縣市</option>
      <option v-for="(districts, city) in zipcodeData['縣市查詢']" :key="city" :value="city">
        {{ city }}
      </option>
    </select>

    <!-- 行政區下拉選單 -->
    <label>行政區：</label>
    <select v-model="selectedDistrict" @change="onDistrictChange">
      <option value="" disabled>選擇行政區</option>
      <option v-for="(zipcode, district) in availableDistricts" :key="district" :value="district">
        {{ district }}
      </option>
    </select>
  </div>
</template>

<script>
import zipcodeData from "../assets/zipcodeData.json";

export default {
  data() {
    return {
      zipcodeData,        // 郵遞區號資料
      selectedCity: "",   // 選擇的縣市
      selectedDistrict: "", // 選擇的行政區
      zipcodeInput: "",   // 用戶輸入的郵遞區號
    };
  },
  computed: {
    // 根據選擇的縣市取得對應的行政區列表
    availableDistricts() {
      return this.selectedCity ? this.zipcodeData["縣市查詢"][this.selectedCity] : {};
    },
  },
  methods: {
    // 當縣市變更時清空行政區並更新郵遞區號
    onCityChange() {
      this.selectedDistrict = "";
      this.updateZipcode();
    },
    // 當行政區變更時更新郵遞區號
    onDistrictChange() {
      this.updateZipcode();
    },
    // 根據縣市與行政區更新郵遞區號
    updateZipcode() {
      if (this.selectedCity && this.selectedDistrict) {
        this.zipcodeInput = this.zipcodeData["縣市查詢"][this.selectedCity][this.selectedDistrict];
      }
    },
    // 根據輸入的郵遞區號反向查詢縣市和行政區
    onZipcodeInput() {
      const inputZipcode = this.zipcodeInput.trim();
      if (inputZipcode.length === 3) {
        const data = this.zipcodeData["郵遞區號查詢"][inputZipcode];
        if (data) {
          this.selectedCity = data.city;
          this.selectedDistrict = data.district;
        } else {
          this.selectedCity = "";
          this.selectedDistrict = "";
        }
      }
    },
  },
};
</script>

<style scoped>
.zipcode-selector {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 20px;
}

input, select {
  padding: 5px;
  margin: 5px;
}
</style>
