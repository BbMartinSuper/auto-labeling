<template>
  <a-card title="机组信息" :bordered="true" class="info-card">
    <div class="turbine-info">
      <div class="info-item">
        <a-statistic
          title="机组数量"
          :value="turbineData.数量"
          suffix="台"
          :value-style="{ color: '#ffffff' }"
        >
          <template #prefix>
            <span style="font-size: 20px;">🏭</span>
          </template>
        </a-statistic>
      </div>
      <div class="info-item">
        <a-statistic
          title="机组组号"
          :value="turbineData.组号"
          :value-style="{ color: '#ffffff' }"
        >
          <template #prefix>
            <span style="font-size: 20px;">⚙️</span>
          </template>
        </a-statistic>
      </div>
    </div>
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
  height: 250px;
  width: 100%;
  margin-bottom: 20px;
  background: linear-gradient(135deg, #53d7ff 0%, #268dee 100%);
  border: none;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.turbine-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 6px 0;
}

.info-item {
  flex: 1;
  padding: 2px 0;
}
</style>