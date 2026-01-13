<template>
  <a-card title="机组信息" :bordered="true" class="info-card">
    <a-row :gutter="16">
      <a-col :span="12">
        <a-statistic
          title="机组数量"
          :value="turbineData.数量"
          suffix="台"
          :value-style="{ color: '#3f8600' }"
        >
          <template #prefix>
            <span style="font-size: 20px;">🏭</span>
          </template>
        </a-statistic>
      </a-col>
      <a-col :span="12">
        <a-statistic
          title="机组型号"
          :value="turbineData.型号"
          :value-style="{ color: '#1890ff' }"
        >
          <template #prefix>
            <span style="font-size: 20px;">⚙️</span>
          </template>
        </a-statistic>
      </a-col>
    </a-row>
  </a-card>
</template>

<script setup>
const props = defineProps({
  turbineData: {
    type: Object,
    required: true
  }
})
</script>

<style scoped>
.info-card {
  margin-bottom: 20px;
}
</style>