<template>
  <div class="app-container">
    <a-layout class="layout">
      <a-layout-header class="header">
        <div class="header-content">
          <h1 class="title">🌀 数据展示系统</h1>
          <div class="header-right">
            <div class="nav-buttons">
              <a-button 
                type="default" 
                @click="prevFile" 
                :disabled="fileList.length <= 1 || loading"
                class="nav-button">
                ⬅️ 上一个
              </a-button>
              <a-button type="primary" @click="loadData" :loading="loading" class="refresh-button">
                🔄 刷新数据
              </a-button>
              <a-button 
                type="default" 
                @click="nextFile" 
                :disabled="fileList.length <= 1 || loading"
                class="nav-button">
                下一个 ➡️
              </a-button>
            </div>
          </div>
        </div>
      </a-layout-header>

      <a-layout-content class="content">
        <a-row :gutter="[12, 12]" type="flex">
    
          <a-col :style="{ flex: '0 0 40%', maxWidth: '40%' }">
            <WindFarmInfo :wind-farm-data="data.风场" />
          </a-col>

          <a-col :style="{ flex: '0 0 20%', maxWidth: '20%' }">
            <TurbineInfo :turbine-data="data.机组" />
          </a-col>

          <a-col :style="{ flex: '0 0 40%', maxWidth: '40%' }">
            <FileInfoCard 
              :file-name="fileName" 
              :current-index="currentIndex" 
              :total-files="fileList.length" 
              :date="data.日期" 
            />
          </a-col>
          <a-col :xs="24" :md="8">
            <SoundStatus :sound-status="data.扫风声" />
          </a-col>
          <a-col :xs="24" :md="16">
            <SoundChart :sound-types="data.声音类型" />
          </a-col>
        </a-row>
      </a-layout-content>

      <a-layout-footer class="footer">
        <div>设备健康智能监测实验室 ©2026</div>
      </a-layout-footer>
    </a-layout>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { message } from 'ant-design-vue'
import WindFarmInfo from './components/WindFarmInfo.vue'
import TurbineInfo from './components/TurbineInfo.vue'
import SoundStatus from './components/SoundStatus.vue'
import SoundChart from './components/SoundChart.vue'
import FileInfoCard from './components/FileInfoCard.vue'

const loading = ref(false)
const fileList = ref([])
const fileModules = ref({})
const currentIndex = ref(0)

const fileName = computed(() => fileList.value[currentIndex.value])

const data = ref({
  风场: {
    名称: '',
    位置: '',
    额定功率: '',
    状态: ''
  },
  机组: {
    数量: 0,
    组号: ''
  },
  日期: '',
  扫风声: '',
  声音类型: []
})

const loadFileList = async () => {
  try {
    const modules = import.meta.glob('../data/*.json')
    fileModules.value = modules
    
    const fileNames = Object.keys(modules).map(path => {
      const fileName = path.split('/').pop().split('\\').pop()
      return fileName
    }).sort()
    
    fileList.value = fileNames
    
    if (fileList.value.length > 0) {
      currentIndex.value = 0
      loadData()
    } else {
      message.warning('data 文件夹下没有找到 JSON 文件', 1)
    }
  } catch (error) {
    console.error('加载文件列表失败:', error)
    message.error('加载文件列表失败', 1)
  }
}

const loadData = async () => {
  if (fileList.value.length === 0) return
  
  loading.value = true
  try {
    const modulePath = `../data/${fileName.value}`
    const module = await fileModules.value[modulePath]()
    data.value = module.default
    message.success(`数据刷新成功！当前文件：${fileName.value}`, 1.5)
  } catch (error) {
    console.error('加载数据失败:', error)
    message.error(`数据加载失败，请检查 ${fileName.value} 文件`, 1.5)
  } finally {
    loading.value = false
  }
}

const prevFile = () => {
  if (fileList.value.length <= 1) return
  currentIndex.value = currentIndex.value > 0 ? currentIndex.value - 1 : fileList.value.length - 1
  loadData()
}

const nextFile = () => {
  if (fileList.value.length <= 1) return
  currentIndex.value = currentIndex.value < fileList.value.length - 1 ? currentIndex.value + 1 : 0
  loadData()
}

onMounted(() => {
  loadFileList()
})
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #309b3e 0%, #5a85cc 100%);
}

.layout {
  min-height: 100vh;
}

.header {
  background: linear-gradient(90deg, #910996 0%, #3d279c 100%);
  padding: 0 32px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
  position: relative;
  z-index: 10;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1400px;
  margin: 0 auto;
}

.title {
  color: rgb(255, 255, 255);
  font-size: 28px;
  font-weight: 700;
  margin: 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.nav-buttons {
  display: flex;
  gap: 12px;
  align-items: center;
}

.nav-button {
  border-radius: 8px;
  font-weight: 500;
  transition: all 0.3s;
}

.nav-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.refresh-button {
  border-radius: 8px;
  font-weight: 500;
  transition: all 0.3s;
}

.refresh-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.content {
  padding: 16px;
  max-width: 1400px;
  margin: 0 auto;
  width: 100%;
}

.footer {
  text-align: center;
  background: rgba(255, 255, 255, 0.95);
  padding: 20px;
  color: #666;
  font-weight: 500;
}
</style>