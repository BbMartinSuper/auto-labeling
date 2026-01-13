<template>
  <div class="app-container">
    <a-layout class="layout">
      <a-layout-header class="header">
        <div class="header-content">
          <h1 class="title">🌀 数据展示系统</h1>
          <div class="header-right">
            <a-button 
              type="default" 
              @click="prevFile" 
              :disabled="fileList.length <= 1 || loading"
              style="margin-right: 8px;">
              ⬅️ 上一个
            </a-button>
            <a-button type="primary" @click="loadData" :loading="loading">
              🔄 刷新数据
            </a-button>
            <a-button 
              type="default" 
              @click="nextFile" 
              :disabled="fileList.length <= 1 || loading"
              style="margin-left: 8px;">
              下一个 ➡️
            </a-button>
            <a-tag color="blue" style="font-size: 14px; margin-left: 16px;">
              📊 {{ currentIndex + 1 }}/{{ fileList.length }}
            </a-tag>
            <a-tag color="green" style="font-size: 14px; margin-left: 8px;">
              📄 {{ fileName }}
            </a-tag>
            <a-tag color="red" style="font-size: 14px; margin-left: 8px;">
              📅 {{ data.日期 }}
            </a-tag>
          </div>
        </div>
      </a-layout-header>

      <a-layout-content class="content">
        <a-row :gutter="[16, 16]">
          <a-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
            <WindFarmInfo :wind-farm-data="data.风场" />
          </a-col>

          <a-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
            <TurbineInfo :turbine-data="data.机组" />
          </a-col>

          <a-col :xs="24" :sm="24" :md="8" :lg="8" :xl="8">
            <SoundStatus :sound-status="data.扫风声" />
          </a-col>

          <!-- 声音类型 -->
          <a-col :xs="24" :sm="24" :md="16" :lg="16" :xl="16">
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
    型号: ''
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
    message.success(`数据刷新成功！当前文件：${fileName.value}`, 1)
  } catch (error) {
    console.error('加载数据失败:', error)
    message.error(`数据加载失败，请检查 ${fileName.value} 文件`, 1)
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
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.layout {
  min-height: 100vh;
}

.header {
  background: linear-gradient(90deg, #1890ff 0%, #096dd9 100%);
  padding: 0 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1400px;
  margin: 0 auto;
}

.title {
  color: white;
  font-size: 24px;
  font-weight: 600;
  margin: 0;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
}

.date-display {
  display: flex;
  align-items: center;
}

.header-right {
  display: flex;
  align-items: center;
}

.content {
  padding: 24px;
  max-width: 1400px;
  margin: 0 auto;
  width: 100%;
}

.footer {
  text-align: center;
  background: rgba(255, 255, 255, 0.9);
  padding: 16px;
  color: #666;
}
</style>