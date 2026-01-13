<template>
  <a-card title="文件信息" :bordered="true" class="file-info-card">
    <div class="file-info-content">
      <div class="info-item">
        <span class="info-label">📄 当前文件</span>
        <span class="info-value">{{ fileName }}</span>
      </div>
      <div class="info-item">
        <span class="info-label">📊 文件序号</span>
        <span class="info-value">{{ currentIndex + 1 }} / {{ totalFiles }}</span>
      </div>
      <div class="info-item">
        <span class="info-label">📅 文件日期</span>
        <span class="info-value">{{ date }}</span>
      </div>
    </div>
  </a-card>
</template>

<script setup>
const props = defineProps({
  fileName: {
    type: String,
    default: ''
  },
  currentIndex: {
    type: Number,
    default: 0
  },
  totalFiles: {
    type: Number,
    default: 0
  },
  date: {
    type: String,
    default: ''
  }
})
</script>

<style scoped>
.file-info-card {
  height: 250px;
  width: 100%;
  background: linear-gradient(135deg, #53d7ff 0%, #f84e95 100%);
  border-radius: 12px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
}

.file-info-content {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 10px;
}

.info-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  /* background: #fafafa; */
  background: linear-gradient(135deg, #53d7ff 0%, #f84e95 100%);
  border-radius: 8px;
  border-bottom: 1px solid #f0f0f0;
}

.info-item:last-child {
  border-bottom: none;
}

.info-label {
  font-size: 14px;
  color: #000000;
  font-weight: 500;
}

.info-value {
  font-size: 16px;
  color: #ffffff;
  font-weight: 600;
}
</style>