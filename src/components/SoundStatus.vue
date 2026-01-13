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
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border: none;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.sound-status {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 18px;
  padding: 16px 0;
  color: white;
}

.status-text {
  font-weight: 600;
  font-size: 20px;
}

.status-icon {
  font-size: 28px;
}
</style>