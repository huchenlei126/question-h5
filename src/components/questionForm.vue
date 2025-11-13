<template>
  <div class="questionnaire-container">
    <!-- Header -->
    <div class="header">
      <h1 class="title">{{ t.title }}</h1>
      <button @click="toggleLang" class="lang-toggle">
        <!-- <img src="@/assets/icon-translate.png" alt="lang-icon" /> -->
        <img :src="iconTranslate" alt="lang-icon" />
      </button>
    </div>

    <!-- Content -->
    <div class="content-area">
      <!-- Tabs -->
      <div class="tabs-container">
        <button
          v-for="(tab, idx) in t.tabs"
          :key="idx"
          @click="changeStep(idx)"
          :class="['tab-btn', { active: step === idx }]"
        >
          {{ tab }}
        </button>
      </div>

      <!-- Step 0: 企业基本信息 -->
      <div v-show="step === 0" class="form-container">
        <div class="welcome-card">
          <p class="welcome-text">{{ t.welcome.desc1 }}</p>
        </div>

        <van-form ref="form0" @submit="handleNext">
          <div class="form-section">
            <div class="van-label">{{ t.step1.companyName }}</div>
            <van-field
              v-model="formData.companyName"
              name="companyName"
              :placeholder="t.step1.companyNamePlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              
            />
            <div class="van-label">{{ t.step1.industry }}</div>
            <van-field
              v-model="formData.industry"
              is-link
              readonly
              name="industry"
              :placeholder="t.step1.industryPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('industry')"
            />
            <div class="van-label">{{ t.step1.area }}</div>
            <van-field
              v-model="formData.area"
              is-link
              readonly
              name="area"
              :placeholder="t.step1.areaPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('area')"
            />

            <div class="van-label">{{ t.step1.employeeCount }}</div>

            <van-field
              v-model="formData.employeeCount"
              is-link
              readonly
              name="employeeCount"
              :placeholder="t.step1.employeeCountPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('employeeCount')"
            />
          </div>

          <div class="button-container">
            <van-button round block type="primary" native-type="submit">
              {{ t.buttons.next }}
            </van-button>
          </div>
        </van-form>
      </div>

      <!-- Step 1: 现有监控系统情况 -->
      <div v-show="step === 1" class="form-container">
        <div class="welcome-card">
          <p class="welcome-text">{{ t.welcome.desc2 }}</p>
        </div>
        <van-form ref="form1" @submit="handleNext">
          <div class="form-section">
            <div class="van-label">{{ t.step2.hasSystem }}</div>

            <van-field
              v-model="formData.hasSystem"
              is-link
              readonly
              name="hasSystem"
              :placeholder="t.step2.hasSystemPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('hasSystem')"
            />

            <template v-if="formData.hasSystem === (lang === 'zh' ? '是' : 'Yes')">
              <div class="van-label">{{ t.step2.systemBrand }}</div>

              <van-field
                v-model="formData.systemBrand"
                name="systemBrand"
                :placeholder="t.step2.systemBrandPlaceholder"
              />

              <div class="van-label">{{ t.step2.cameraCount }}</div>

              <van-field
                v-model="formData.cameraCount"
                name="cameraCount"
                type="number"
                :placeholder="t.step2.cameraCountPlaceholder"
              />
              <div class="van-label">{{ t.step2.deployTime }}</div>

              <van-field
                v-model="formData.deployTime"
                is-link
                readonly
                name="deployTime"
                :placeholder="t.step2.deployTime"
                @click="showDatePicker"
              />
              <div class="van-label">{{ t.step2.keepDevice }}</div>

              <van-field
                v-model="formData.keepDevice"
                is-link
                readonly
                name="keepDevice"
                :placeholder="t.step2.keepDevicePlaceholder"
                @click="showPicker('keepDevice')"
              />
            </template>
          </div>

          <div class="button-container">
            <van-button round block type="primary" native-type="submit">
              {{ t.buttons.next }}
            </van-button>
          </div>
        </van-form>
      </div>

      <!-- Step 2: AI视觉监控功能需求 -->
      <div v-show="step === 2" class="form-container">
        <div class="welcome-card">
          <p class="welcome-text">{{ t.welcome.desc3 }}</p>
        </div>
        <van-form ref="form2" @submit="handleNext">
          <div class="form-section">
            <div class="van-label">{{ t.step3.high }}</div>
            <van-field
              v-model="formData.high"
              is-link
              readonly
              name="high"
              :placeholder="t.step3.highPlaceholder"
              @click="showMulPicker('high')"
            />

            <div class="van-label">{{ t.step3.middle }}</div>
            <van-field
              v-model="formData.middle"
              is-link
              readonly
              name="middle"
              :placeholder="t.step3.middlePlaceholder"
              @click="showMulPicker('middle')"
            />

            <div class="van-label">{{ t.step3.low }}</div>
            <van-field
              v-model="formData.low"
              is-link
              readonly
              name="low"
              :placeholder="t.step3.lowPlaceholder"
              @click="showMulPicker('low')"
            />

            <div class="van-label">{{ t.step3.camera }}</div>
            
            <van-field
              v-model="formData.camera"
              is-link
              readonly
              name="camera"
              :placeholder="t.step3.cameraPlaceholder"
              @click="showPicker('camera')"
              :rules="[{ required: true, message: t.validation.required }]"
            />

            <div class="van-label">{{ t.step3.location }}</div>
            <van-field
              v-model="formData.location"
              is-link
              readonly
              name="location"
              :placeholder="t.step3.locationPlaceholder"
              @click="showMulPicker('location')"
              :rules="[{ required: true, message: t.validation.required }]"
            />

            <div class="van-label">{{ t.step3.environment }}</div>
            <van-field
              v-model="formData.environment"
              is-link
              readonly
              name="environment"
              :placeholder="t.step3.environmentPlaceholder"
              @click="showPicker('environment')"
              :rules="[{ required: true, message: t.validation.required }]"
            />

            <div class="van-label">{{ t.step3.integration }}</div>
            <van-field
              v-model="formData.integration"
              is-link
              readonly
              name="integration"
              :placeholder="t.step3.integrationPlaceholder"
              @click="showPicker('integration')"
              :rules="[{ required: true, message: t.validation.required }]"
            />

            <div class="van-label">{{ t.step3.mobile }}</div>
            <van-field
              v-model="formData.mobile"
              is-link
              readonly
              name="mobile"
              :placeholder="t.step3.mobilePlaceholder"
              @click="showPicker('mobile')"
              :rules="[{ required: true, message: t.validation.required }]"
            />

            <div class="van-label" v-if="formData.mobile && formData.mobile.includes(lang === 'zh' ? '是' : 'Yes')">{{ t.step3.platform }}</div>
            <van-field
              v-if="formData.mobile && formData.mobile.includes(lang === 'zh' ? '是' : 'Yes')"
              v-model="formData.platform"
              name="platform"
              :placeholder="t.step3.platformPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
            />
          </div>

          <div class="button-container">
            <van-button round block type="primary" native-type="submit">
              {{ t.buttons.next }}
            </van-button>
          </div>
        </van-form>
      </div>

      <!-- Step 3: 项目预算与时间规划 -->
      <div v-show="step === 3" class="form-container">
        <div class="welcome-card">
          <p class="welcome-text">{{ t.welcome.desc4 }}</p>
        </div>
        <van-form ref="form3" @submit="handleComplete">
          <div class="form-section">
            <div class="van-label">{{ t.step4.budget }}</div>
            <van-field
              v-model="formData.budget"
              is-link
              readonly
              name="budget"
              :placeholder="t.step4.budgetPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('budget')"
            />

            <div class="van-label">{{ t.step4.startTime }}</div>

            <van-field
              v-model="formData.startTime"
              is-link
              readonly
              name="startTime"
              :placeholder="t.step4.startTimePlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('startTime')"
            />

            <div class="van-label">{{ t.step4.training }}</div>

            <van-field
              v-model="formData.needTraining"
              is-link
              readonly
              name="needTraining"
              :placeholder="t.step4.trainingPlaceholder"
              :rules="[{ required: true, message: t.validation.required }]"
              @click="showPicker('needTraining')"
            />

            <div class="van-label" v-if="formData.needTraining && formData.needTraining.includes(lang === 'zh' ? '是' : 'Yes')">{{ t.step4.trainingCount }}</div>

            <van-field
              v-if="formData.needTraining && formData.needTraining.includes(lang === 'zh' ? '是' : 'Yes')"
              v-model="formData.trainingCount"
              name="trainingCount"
              type="number"
              :rules="[{ required: true, message: t.validation.required }]"
              :placeholder="t.step4.trainingCountPlaceholder"
            />
            <div class="van-label">{{ t.step4.otherNeeds }}</div>

            <van-field
              v-model="formData.otherNeeds"
              rows="3"
              autosize
              type="textarea"
              :placeholder="t.step4.otherNeedsPlaceholder"
            />
          </div>

          <div class="button-container">
            <van-button round block type="primary" native-type="submit">
              {{ t.buttons.complete }}
            </van-button>
          </div>
        </van-form>
      </div>
    </div>

    <!-- Picker Popup -->
    <van-popup v-if="showPickerPopup" v-model:show="showPickerPopup" position="bottom">
      <van-picker
        :columns="pickerColumns"
        @confirm="onPickerConfirm"
        @cancel="showPickerPopup = false"
        style="height: 300px;"
      />
    </van-popup>

    <van-popup v-if="showMulPickerPopup" v-model:show="showMulPickerPopup" position="bottom" style="height: 300px;">
      <van-checkbox-group v-if="currentMulPickerField == 'high'" style="padding: 10px" v-model="highGroup" @change="onHighChange">
          <van-checkbox shape="square" style="margin: 20px 5px;font-size: 14px;" v-for="(item,index) in mulPickerColumns" :key="index" :name="item">
            {{item}}
          </van-checkbox>
        </van-checkbox-group>
      <van-checkbox-group v-if="currentMulPickerField == 'middle'" style="padding: 10px" v-model="middleGroup" @change="onMiddleChange">
          <van-checkbox shape="square" style="margin: 20px 5px;font-size: 14px;" v-for="(item,index) in mulPickerColumns" :key="index" :name="item">
            {{item}}
          </van-checkbox>
        </van-checkbox-group>
      <van-checkbox-group v-if="currentMulPickerField == 'low'" style="padding: 10px" v-model="lowGroup" @change="onLowChange">
          <van-checkbox shape="square" style="margin: 20px 5px;font-size: 14px;" v-for="(item,index) in mulPickerColumns" :key="index" :name="item">
            {{item}}
          </van-checkbox>
        </van-checkbox-group>
      <van-checkbox-group v-if="currentMulPickerField == 'location'" style="padding: 10px" v-model="locationGroup" @change="onLocationChange">
          <van-checkbox shape="square" style="margin: 20px 5px;font-size: 14px;" v-for="(item,index) in mulPickerColumns" :key="index" :name="item">
            {{item}}
          </van-checkbox>
        </van-checkbox-group>
    </van-popup>

    <!-- Date Picker Popup -->
    <van-popup v-if="showDatePickerPopup" v-model:show="showDatePickerPopup" position="bottom">
      <van-date-picker
        v-model="currentDate"
        :min-date="minDate"
        :max-date="maxDate"
        @confirm="onDateConfirm"
        @cancel="showDatePickerPopup = false"
      />
    </van-popup>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ref, reactive, computed, onMounted } from 'vue';
import { showToast } from 'vant';
import iconTranslate from '@/assets/img/icon-translate.png';
import dayjs from 'dayjs';

const translations = {
  zh: {
    title: 'CV调查问卷',
    tabs: ['企业信息', '现有系统', '功能需求', '预算规划'],
    validation: {
      required: '此项为必填项'
    },
    welcome: {
      desc1: '您好！为协助贵公司初步评估AI视觉监控智能系统的部署方案及预算，我们设计了以下问卷。请根据实际需求填写，以便我们提供精准的初步报价建议。后续将安排技术团队现场勘查，进一步细化方案。感谢您的支持！',
      desc2: '现有监控系统情况（如无则跳过）',
      desc3: 'AI视觉监控功能需求，可多选',
      desc4: '项目预算与时间规划，其他补充说明'
    },
    step1: {
      companyName: '企业名称',
      companyNamePlaceholder: '请输入企业名称',
      industry: '所属行业',
      industryPlaceholder: '请选择',
      industryOptions: ['制造业', '物流仓储', '能源化工', '工业园区', '其他'],
      area: '厂区/仓库面积',
      areaPlaceholder: '请选择',
      areaOptions: ['<1万㎡', '1-5万㎡', '5-10万㎡', '>10万㎡'],
      employeeCount: '现有员工规模',
      employeeCountPlaceholder: '请选择',
      employeeCountOptions: ['<100人', '100-500人', '500-1000人', '>1000人']
    },
    step2: {
      hasSystem: '是否已部署传统监控系统',
      hasSystemPlaceholder: '请选择',
      hasSystemOptions: ['是', '否'],
      systemBrand: '品牌型号',
      systemBrandPlaceholder: '请输入品牌型号',
      cameraCount: '摄像头数量',
      cameraCountPlaceholder: '请输入数量',
      deployTime: '部署时间',
      keepDevice: '是否需要保留现有设备',
      keepDevicePlaceholder: '请选择',
      keepDeviceOptions: ['全部保留', '部分保留', '完全替换'],
    },
    step3: {
      high: '高优先级核心AI算法功能（可多选）',
      highPlaceholder: '请选择',
      highOptions:[
        '周界入侵检测（围墙/禁区闯入）',
        '人员聚集预警（车间/通道拥堵）',
        '危险行为识别（攀爬/摔倒/奔跑）',
        '物料堆放合规检测（高度/倾斜/宽度）',
      ],
      middle: '中等优先级核心AI算法功能（可多选）',
      middlePlaceholder: '请选择',
      middleOptions: [
        '安全帽/反光衣佩戴检测',
        '吸烟/打电话等违规行为识别',
        '设备状态监测（异常停机/异响）',
        '物料错放/混放识别（放错区域/货架）',
        '工具/工装在位监测（指定区域）',
        '生产工序合规（工序遗漏/操作顺序错误）',
        '外来人员/车辆未授权进入',
        '托盘/物料自动计数',
        '叉车/牵引车运行轨迹合规',
      ],
      low: '低优先级核心AI算法功能（可多选）',
      lowPlaceholder: '请选择',
      lowOptions: [
        '烟火识别（火焰/烟雾实时报警）',
        '温湿度/粉尘浓度超标预警',
      ],
      camera: '预计摄像头数量',
      cameraPlaceholder: '请选择',
      cameraOptions: ['<20路', '21-60路', '61-100路', '>100路'],
      location: '安装位置（可多选）',
      locationPlaceholder: '请选择',
      locationOptions: ['厂区周界', '生产车间', '仓储区域', '办公区域', '出入口', '其他'],
      environment: '环境适应性需求',
      environmentPlaceholder: '环境适应性需求',
      environmentOptions: ['室外防水防尘（IP66+）', '夜视功能（红外/星光级）', '防爆防腐', '高温/低温环境', '其他'],
      integration: '是否需要与现有系统集成',
      integrationPlaceholder: '请选择',
      integrationOptions: ['门禁系统', '消防系统', 'ERP/MES系统', '无', '其他'],
      mobile: '是否需要移动端访问',
      mobilePlaceholder: '请选择',
      mobileOptions: ['是', '否'],
      platform: '需支持平台',
      platformPlaceholder: '如：iOS、Android、微信小程序等'
      
    },
    step4: {
      title: '项目预算与时间规划',
      budget: '预算范围（万元）',
      budgetPlaceholder: '请选择',
      budgetOptions: ['<20', '21-50', '51-80', '81-100', '>100', '暂不确定'],
      startTime: '期望项目启动时间',
      startTimePlaceholder: '请选择',
      startTimeOptions: ['1个月内', '2-3个月内', '半年内', '其他'],
      training: '是否需要驻场培训',
      trainingPlaceholder: '请选择',
      trainingOptions: ['是', '否'],
      trainingCount: '需培训人次',
      trainingCountPlaceholder: '请输入人次',
      otherNeeds: '其他补充说明',
      otherNeedsPlaceholder: '其他未提及的需求或疑问'
    },
    buttons: {
      next: '下一步',
      complete: '完成'
    }
  },
  en: {
    title: 'CV Questionnaire',
    tabs: ['Company', 'System', 'Features', 'Planning'],
    validation: {
      required: 'This field is required'
    },
    welcome: {
      desc1: 'Hello! To assist your company in preliminary evaluation of AI vision monitoring system deployment plan and budget, we have designed the following questionnaire. Please fill in according to actual needs so that we can provide accurate preliminary quotation suggestions. We will arrange technical team on-site survey later to further refine the plan. Thank you for your support!',
      desc2: 'Current monitoring system status (skip if none)',
      desc3: 'AI visual monitoring functional requirements (multiple selections allowed)',
      desc4: 'Project budget and timeline, other supplementary notes'
    },
    step1: {
      companyName: 'Company Name',
      companyNamePlaceholder: 'Please enter company name',
      industry: 'Industry',
      industryPlaceholder: 'Please select',
      industryOptions: ['Manufacturing', 'Logistics & Warehousing', 'Energy & Chemical', 'Industrial Park', 'Other'],
      area: 'Factory/Warehouse Area',
      areaPlaceholder: 'Please select',
      areaOptions: ['<10,000㎡', '10,000-50,000㎡', '50,000-100,000㎡', '>100,000㎡'],
      employeeCount: 'Current Employee Scale',
      employeeCountPlaceholder: 'Please select',
      employeeCountOptions: ['<100', '100-500', '500-1000', '>1000']
    },
    step2: {
      hasSystem: 'Traditional monitoring system deployed',
      hasSystemPlaceholder: 'Please select',
      hasSystemOptions: ['Yes', 'No'],
      systemBrand: 'Brand & Model',
      systemBrandPlaceholder: 'Please enter brand and model',
      cameraCount: 'Number of Cameras',
      cameraCountPlaceholder: 'Please enter number',
      deployTime: 'Deployment Time',
      keepDevice: 'Keep existing equipment',
      keepDevicePlaceholder: 'Please select',
      keepDeviceOptions: ['Keep all', 'Partial keep', 'Complete replacement'],
    },
    step3: {
     high: 'High-priority core AI algorithm functions (multiple selections allowed)',
     highPlaceholder: 'Please select',
     highOptions:[
       'Perimeter intrusion detection (wall/restricted area breaches)',
       'Crowd gathering alerts (workshop/passage congestion)',
       'Hazardous behavior recognition (climbing/fall/running)',
       'Material stacking compliance check (height/tilt/width)',
     ],
     middle: 'Medium-priority core AI algorithm functions (multiple selections allowed)',
     middlePlaceholder: 'Please select',
     middleOptions: [
       'Helmet/reflection vest wearing detection',
       'Violation behavior recognition (smoking/using phone, etc.)',
       'Equipment status monitoring (unexpected shutdown/abnormal noise)',
       'Material misplacement/mixing detection (wrong area/shelf)',
       'Tool/workstation presence monitoring (specified area)',
       'Production process compliance (missing steps/incorrect operation sequence)',
       'Unauthorized entry by outsiders/vehicles',
       'Automatic pallet/material counting',
       'Forklift/trailer movement compliance',
     ],
     low: 'Low-priority core AI algorithm functions (multiple selections allowed)',
     lowPlaceholder: 'Please select',
     lowOptions: [
       'Fire/smoke detection (real-time alarm)',
       'Temperature/humidity or dust concentration alert',
     ],
     camera: 'Estimated number of cameras',
     cameraPlaceholder: 'Please select',
     cameraOptions: ['<20 channels', '21-60 channels', '61-100 channels', '>100 channels'],
     location: 'Installation locations (multiple selections allowed)',
     locationPlaceholder: 'Please select',
     locationOptions: ['Perimeter', 'Production workshop', 'Storage area', 'Office area', 'Entrance/Exit', 'Other'],
     environment: 'Environmental adaptability requirements',
     environmentPlaceholder: 'Environmental adaptability requirements',
     environmentOptions: ['Outdoor waterproof/dustproof (IP66+)', 'Night vision (infrared/starlight)', 'Explosion-proof/corrosion-proof', 'High/low temperature environment', 'Other'],
     integration: 'Need to integrate with existing systems?',
     integrationPlaceholder: 'Please select',
     integrationOptions: ['Access control system', 'Fire system', 'ERP/MES system', 'None', 'Other'],
     mobile: 'Require mobile access?',
     mobilePlaceholder: 'Please select',
     mobileOptions: ['Yes', 'No'],
     platform: 'Supported platforms',
     platformPlaceholder: 'e.g., iOS, Android, WeChat Mini Program'
    },
    step4: {
      title: 'Budget & Planning',
      budget: 'Budget range (10k CNY)',
      budgetPlaceholder: 'Please select',
      budgetOptions: ['<20', '21-50', '51-80', '81-100', '>100', 'Uncertain'],
      startTime: 'Expected start time',
      startTimePlaceholder: 'Please select',
      startTimeOptions: ['Within 1 month', 'Within 2-3 months', 'Within half year', 'Other'],
      training: 'On-site training needed',
      trainingPlaceholder: 'Please select',
      trainingOptions: ['Yes', 'No'],
      trainingCount: 'Training participants',
      trainingCountPlaceholder: 'Please enter number',
      otherNeeds: 'Other notes',
      otherNeedsPlaceholder: 'Other requirements or questions'
    },
    buttons: {
      next: 'Next',
      complete: 'Complete'
    }
  }
};

const lang = ref('en');
const step = ref(0);
const form0 = ref(null);
const form1 = ref(null);
const form2 = ref(null);
const form3 = ref(null);

const formData = reactive({
  companyName: '',
  industry: '',
  area: '',
  employeeCount: '',
  hasSystem: '',
  systemBrand: '',
  cameraCount: '',
  deployTime: '',
  keepDevice: '',
  high: [],
  middle: [],
  low: [],
  camera: '',
  location: [],
  environment: '',
  integration: '',
  mobile: '',
  platform: '',
  budget: '',
  startTime: '',
  needTraining: '',
  trainingCount: '',
  otherNeeds: ''
});

const highGroup = ref([]);
const middleGroup = ref([]);
const lowGroup = ref([]);
const locationGroup = ref([]);

// Picker
const showPickerPopup = ref(false);
const showMulPickerPopup = ref(false);
const pickerColumns = ref([]);
const mulPickerColumns = ref([]);
const currentPickerField = ref('');
const currentMulPickerField = ref('');

// Date Picker
const showDatePickerPopup = ref(false);
const today = dayjs().format('YYYY-MM-DD')
const currentDate = ref(today.split('-'));

const minDate = new Date(2020, 0, 1);
const maxDate = new Date(2030, 11, 31);

const t = computed(() => translations[lang.value]);

const toggleLang = () => {
  lang.value = lang.value === 'zh' ? 'en' : 'zh';
};

const changeStep = (idx) => {
  // 允许向前切换，向后需要验证
  console.log(idx, step.value);
  if (idx < step.value) {
    step.value = idx;
  } else if (idx === step.value + 1) {
    handleNext();
  }
};

const showPicker = (field) => {
  currentPickerField.value = field;
  
  const optionsMap = {
    industry: t.value.step1.industryOptions,
    area: t.value.step1.areaOptions,
    employeeCount: t.value.step1.employeeCountOptions,
    hasSystem: t.value.step2.hasSystemOptions,
    keepDevice: t.value.step2.keepDeviceOptions,
    camera: t.value.step3.cameraOptions,
    environment: t.value.step3.environmentOptions,
    integration: t.value.step3.integrationOptions,
    mobile: t.value.step3.mobileOptions,
    budget: t.value.step4.budgetOptions,
    startTime: t.value.step4.startTimeOptions,
    needTraining: t.value.step4.trainingOptions
  };
  
  const options = optionsMap[field];
  pickerColumns.value = options.map(option => {
    return { text: option, value: option }
  }) || [];
  console.log(field,pickerColumns.value);
  showPickerPopup.value = true;
};

const showMulPicker = (field) => {
  console.log('field',field);
  currentMulPickerField.value = field;
  
  const optionsMap = {
    high: t.value.step3.highOptions,
    middle: t.value.step3.middleOptions,
    low: t.value.step3.lowOptions,
    location: t.value.step3.locationOptions,
  };
  
  const options = optionsMap[field];
  // showMulPickerPopup.value = options.map(option => {
  //   return { text: option, value: option }
  // }) || [];
  mulPickerColumns.value = options || []
  console.log(field,pickerColumns.value);
  showMulPickerPopup.value = true;
};

const onHighChange = (names) => {
  formData.high = highGroup.value
};
const onMiddleChange = (names) => {
  formData.middle = middleGroup.value
};
const onLowChange = (names) => {
  formData.low = lowGroup.value
};
const onLocationChange = (names) => {
  formData.location = locationGroup.value
};
const onPickerConfirm = (selectedValues) => {
  console.log('selectedValues', selectedValues);
  if (currentPickerField.value === 'hasSystem'&&(selectedValues.selectedValues[0] == '否' || selectedValues.selectedValues[0] == 'No')) {
    formData.systemBrand = '';
    formData.cameraCount = '';
    formData.deployTime = '';
    formData.keepDevice = '';
  }
  formData[currentPickerField.value] = selectedValues.selectedValues[0];
  showPickerPopup.value = false;
};

const showDatePicker = () => {
  if (formData.deployTime) {
    const date = new Date(formData.deployTime);
    currentDate.value = [
      date.getFullYear().toString(),
      (date.getMonth() + 1).toString().padStart(2, '0'),
      date.getDate().toString().padStart(2, '0')
    ];
  }
  showDatePickerPopup.value = true;
};

const onDateConfirm = ({ selectedValues }) => {
  formData.deployTime = `${selectedValues[0]}-${selectedValues[1]}-${selectedValues[2]}`;
  showDatePickerPopup.value = false;
};

const handleNext = async () => {
  const formRefs = [form0, form1, form2, form3];
  const currentForm = formRefs[step.value];
  
  if (currentForm.value) {
    try {
      await currentForm.value.validate();
      if (step.value < 3) {
        step.value++;
        console.log('Step', step.value);
        // 滚动到顶部
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }
    } catch (error) {
      console.log('Validation failed:', error);
    }
  }
};


const submitData = async (formObj) => {
  try {
    const payload = {
      data: formObj  // key 为 data，对应值是你的对象
    };

    const res = await axios.post('/api/v1/submit', payload, {
      headers: {
        'Content-Type': 'application/json'
      }
    });

    console.log('提交成功:', res.data);
    showToast({message: '提交成功'})
    return res.data;
  } catch (err) {
    console.error('提交失败:', err);
  }
};

const handleComplete = async () => {
  try {
    await form3.value.validate();
    
    const dataToSave = {
      ...formData,
      timestamp: new Date().toISOString(),
    };

    submitData(dataToSave)
    console.log('Data to save:', dataToSave);
    
    // 重置表单
    formData.companyName = '';
    formData.industry = '';
    formData.area = '';
    formData.employeeCount = '';
    formData.hasSystem = '';
    formData.systemBrand = '';
    formData.cameraCount = '';
    formData.deployTime = '';
    formData.keepDevice = '';
    formData.high = [];
    formData.middle = [];
    formData.low = [];
    formData.camera = '';
    formData.location = [];
    formData.environment = '';
    formData.integration = '';
    formData.mobile = '';
    formData.platform = '';
    formData.budget = '';
    formData.startTime = '';
    formData.needTraining = '';
    formData.trainingCount = '';
    formData.otherNeeds = '';

    highGroup.value = [];
    middleGroup.value = [];
    lowGroup.value = [];
    locationGroup.value = [];

    step.value = 0;
    window.scrollTo({ top: 0, behavior: 'smooth' });
  } catch (error) {
    console.log('Validation failed:', error);
  }
};

</script>

<style scoped lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.questionnaire-container {
  min-height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  background: linear-gradient(226.22deg, #2855AE 0%, #7292CF 100%);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
}

/* Header */
.header {
  padding: 50px 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.title {
  color: white;
  font-size: 18px;
  font-weight: 500;
}

.lang-toggle {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  background: transparent;
  border: none;
  border-radius: 6px;
  padding: 6px;
  cursor: pointer;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  // transition: background 0.2s;
}

// .lang-toggle:acitve {
//   background: rgba(255, 255, 255, 0.3);
// }

/* Tabs */
.tabs-container {
  background: white;
  display: flex;
  padding: 0;
  border-bottom: 1px solid #e8e8e8;
  margin-top: 20px;
}

.tab-btn {
  flex: 1;
  padding: 14px 8px;
  border: none;
  background: none;
  color: #666;
  font-size: 14px;
  cursor: pointer;
  position: relative;
  transition: color 0.2s;
}

.tab-btn.active {
  color: #5b7fc7;
  font-weight: 500;
}

.tab-btn.active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 60%;
  height: 3px;
  background: #5b7fc7;
  border-radius: 3px 3px 0 0;
}

/* Content */
.content-area {
  background-color: #ffffff;
  flex: 1;
  padding: 0 0 20px 0;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
}

.form-container {
  /* padding: 12px; */
}

.welcome-card {
  background: #FAFAFA;
  padding: 16px;
  text-align: left;
}

.welcome-text {
  color: #6789CA;
  font-size: 13px;
  line-height: 2;
}

.section-header {
  padding: 16px 16px 8px;
}

.section-title {
  font-size: 14px;
  color: #333;
  font-weight: 500;
}

.section-category {
  font-size: 14px;
  color: #333;
  font-weight: 500;
  margin: 16px 0 12px;
}

.form-section {
  background: white;
  border-radius: 12px;
  padding: 0;
  margin-bottom: 16px;
}
.van-label {
  font-size: 12px;
  color: #646566;
  text-align: left;
  padding: 10px 0px 10px 16px;
}

.section-subtitle {
  color: #666;
  font-size: 14px;
  font-weight: 500;
  margin: 20px 16px 12px;
}

.field-label {
  color: #666;
  font-size: 14px;
  margin-bottom: 12px;
  padding: 0 16px;
}

.checkbox-section {
  padding: 12px 16px;
  border-bottom: 1px solid #f7f8fa;
}

.checkbox-section:last-child {
  border-bottom: none;
}

.button-container {
  padding: 20px 16px;
}

/* Vant component overrides */
:deep(.van-field__label) {
  color: #646566;
  font-size: 14px;
}

:deep(.van-field__control) {
  font-size: 14px;
  color: #323233;
}

:deep(.van-field__control::placeholder) {
  color: #c8c9cc;
}

:deep(.van-checkbox) {
  margin-bottom: 12px;
  align-items: flex-start;
}


:deep(.van-button--primary) {
  background: #5b7fc7;
  border-color: #5b7fc7;
}

:deep(.van-button--primary:active) {
  background: #4a6bb5;
  border-color: #4a6bb5;
}

:deep(.van-cell) {
  border-bottom: 1px solid #cccccc;
  padding: 0 16px 12px 16px;
}

:deep(.van-cell::after) {
  border-color: #f7f8fa;
}

:deep(.van-popup) {
  border-radius: 16px 16px 0 0;
}

:deep(.van-checkbox__label) {
  font-size: 16px;
  text-align: center;
  color: #323233;
  width: 100%;
}
</style>