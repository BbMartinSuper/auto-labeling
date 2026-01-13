<template>
  <a-card title="扫风声状态" :bordered="true" class="info-card">
    <div class="sound-status">
      <a-badge :status="soundStatus === '有' ? 'success' : 'default'" />
      <span class="status-text">{{ soundStatus }}</span>
      <span class="status-icon">{{ soundStatus === '有' ? '🔊' : '🔇' }}</span>
    </div>
  </a-card>
</template>

<script setup>
const props = defineProps({
  soundStatus: {
    type: String,
    required: true
  }
})
</script>

<style scoped>
.info-card {
  margin-bottom: 20px;
}

.sound-status {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 18px;
  padding: 10px 0;
}

.status-text {
  font-weight: 500;
  color: #333;
}

.status-icon {
  font-size: 24px;
}
</style>