<template>
  <a-card title="风场信息" :bordered="true" class="info-card">
    <a-descriptions :column="2" bordered class="wind-farm-descriptions">
      <a-descriptions-item label="名称">
        <a-tag color="#52c41a">{{ windFarmData.名称 }}</a-tag>
      </a-descriptions-item>
      <a-descriptions-item label="位置">
        <a-tag color="#52c41a">{{ windFarmData.位置 }}</a-tag>
      </a-descriptions-item>
      <a-descriptions-item label="额定功率">
        <a-tag color="#faad14">{{ windFarmData.额定功率 }}</a-tag>
      </a-descriptions-item>
      <a-descriptions-item label="状态">
        <a-badge :status="statusType" :text="windFarmData.状态" />
      </a-descriptions-item>
    </a-descriptions>
  </a-card>
</template>

<script setup>
import { computed } from 'vue'
const props = defineProps({
  windFarmData: {
    type: Object,
    required: true
  }
})

const statusType = computed(() => {
  return props.windFarmData.状态 === '运行中' ? 'processing' : 'default'
})
</script>

<style scoped>
.info-card {
  height: 250px;
  width: 100%;
  margin-bottom: 5px;
  /* background: white; */
  background: linear-gradient(135deg, #b9fa20 0%, #eee126 70%);
  border-radius: 12px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
}


.wind-farm-descriptions {
  background: #ffffff;
  border-radius: 8px;
  padding: 16px;
}

.wind-farm-descriptions :deep(.ant-descriptions-item-label) {
  color: #666;
  font-weight: 500;
}

.wind-farm-descriptions :deep(.ant-descriptions-item-content) {
  background: rgb(255, 255, 255);
}
</style>