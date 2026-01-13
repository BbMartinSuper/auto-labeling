<template>
  <a-card title="风场信息" :bordered="true" class="info-card">
    <a-descriptions :column="2" bordered>
      <a-descriptions-item label="名称">
        <a-tag color="blue">{{ windFarmData.名称 }}</a-tag>
      </a-descriptions-item>
      <a-descriptions-item label="位置">
        <a-tag color="green">{{ windFarmData.位置 }}</a-tag>
      </a-descriptions-item>
      <a-descriptions-item label="额定功率">
        <a-tag color="orange">{{ windFarmData.额定功率 }}</a-tag>
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
  margin-bottom: 20px;
}
</style>