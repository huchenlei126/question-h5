<template>
  <div class="questionnaire-container">
    <!-- Header -->
    <div class="header">
      <h1 class="title">CV调查问卷</h1>
      <button @click="toggleLang" class="lang-toggle">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="10"/>
          <line x1="2" y1="12" x2="22" y2="12"/>
          <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
        </svg>
      </button>
    </div>



    <!-- Content -->
    <div class="content-area">
        <!-- Tabs -->
    <div class="tabs-container">
      <button
        v-for="(tab, idx) in t.tabs"
        :key="idx"
        @click="step = idx"
        :class="['tab-btn', { active: step === idx }]"
      >
        {{ tab }}
      </button>
    </div>
      <!-- Step 0: Basic Info -->
      <div v-show="step === 0" class="form-container">
        <div class="welcome-card">
          <p class="welcome-text">{{ t.welcome.desc }}</p>
        </div>

        <div class="form-section">
          <div class="form-group">
            <label class="form-label">{{ t.step1.phone }}</label>
            <input 
              v-model="formData.phone"
              type="text"
              class="form-input"
              :placeholder="t.step1.phonePlaceholder"
            />
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step1.industry }}</label>
            <div class="select-container">
              <input 
                v-model="formData.industry"
                type="text"
                class="form-input"
                :placeholder="t.step1.industryPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step1.salary }}</label>
            <div class="select-container">
              <input 
                v-model="formData.salary"
                type="text"
                class="form-input"
                :placeholder="t.step1.salaryPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step1.experience }}</label>
            <div class="select-container">
              <input 
                v-model="formData.experience"
                type="text"
                class="form-input"
                :placeholder="t.step1.experiencePlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>
        </div>

        <button @click="handleNext" class="next-btn">{{ t.buttons.next }}</button>
      </div>

      <!-- Step 1: Training System -->
      <div v-show="step === 1" class="form-container">
        <div class="form-section">
          <div class="form-group">
            <label class="form-label">{{ t.step2.hasSystem }}</label>
            <div class="select-container">
              <input 
                v-model="formData.hasSystem"
                type="text"
                class="form-input"
                :placeholder="t.step2.systemPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step2.years }}</label>
            <input 
              v-model="formData.years"
              type="text"
              class="form-input"
              :placeholder="t.step2.yearsPlaceholder"
            />
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step2.expire }}</label>
            <input 
              v-model="formData.expire"
              type="date"
              class="form-input"
              :placeholder="t.step2.expirePlaceholder"
            />
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step2.satisfied }}</label>
            <div class="select-container">
              <input 
                v-model="formData.satisfied"
                type="text"
                class="form-input"
                :placeholder="t.step2.satisfiedPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>
        </div>

        <button @click="handleNext" class="next-btn">{{ t.buttons.next }}</button>
      </div>

      <!-- Step 2: Function Needs -->
      <div v-show="step === 2" class="form-container">
        <div class="section-header">
          <h3 class="section-title">{{ t.step3.title }}</h3>
        </div>

        <div class="form-section">
          <p class="section-subtitle">{{ t.step3.subtitle }}</p>
          
          <div class="checkbox-list">
            <label v-for="(option, idx) in t.step3.options" :key="idx" class="checkbox-label">
              <input 
                type="checkbox" 
                :value="option"
                v-model="formData.aiFeatures"
                class="checkbox-input"
              />
              <span class="checkbox-text">{{ option }}</span>
            </label>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle2 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.recommend"
                type="text"
                class="form-input"
                :placeholder="t.step3.recommend"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle3 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.match"
                type="text"
                class="form-input"
                :placeholder="t.step3.match"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle4 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.job"
                type="text"
                class="form-input"
                :placeholder="t.step3.job"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle5 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.lifecycle"
                type="text"
                class="form-input"
                :placeholder="t.step3.lifecycle"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle6 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.nightShift"
                type="text"
                class="form-input"
                :placeholder="t.step3.nightShift"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle7 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.storage"
                type="text"
                class="form-input"
                :placeholder="t.step3.storage"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle8 }}</p>
          <div class="form-group">
            <div class="select-container">
              <input 
                v-model="formData.map"
                type="text"
                class="form-input"
                :placeholder="t.step3.map"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <p class="section-subtitle">{{ t.step3.subtitle9 }}</p>
          <div class="form-group">
            <input 
              v-model="formData.share"
              type="text"
              class="form-input"
              :placeholder="t.step3.share"
            />
          </div>
        </div>

        <button @click="handleNext" class="next-btn">{{ t.buttons.next }}</button>
      </div>

      <!-- Step 3: Scene Planning -->
      <div v-show="step === 3" class="form-container">
        <div class="section-header">
          <h3 class="section-title">{{ t.step4.title }}</h3>
        </div>

        <div class="form-section">
          <div class="form-group">
            <label class="form-label">{{ t.step4.budget }}</label>
            <div class="select-container">
              <input 
                v-model="formData.budget"
                type="text"
                class="form-input"
                :placeholder="t.step4.budgetPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step4.users }}</label>
            <div class="select-container">
              <input 
                v-model="formData.users"
                type="text"
                class="form-input"
                :placeholder="t.step4.usersPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step4.deployment }}</label>
            <div class="select-container">
              <input 
                v-model="formData.deployment"
                type="text"
                class="form-input"
                :placeholder="t.step4.deploymentPlaceholder"
              />
              <svg class="select-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">{{ t.step4.delivery }}</label>
            <input 
              v-model="formData.delivery"
              type="date"
              class="form-input"
              :placeholder="t.step4.deliveryPlaceholder"
            />
          </div>
        </div>

        <button @click="handleComplete" class="complete-btn">{{ t.buttons.complete }}</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const translations = {
  zh: {
    title: 'CV调查问卷',
    tabs: ['基本信息', '培育系统', '功能需求', '场景规划'],
    welcome: {
      desc: '您好！为协助理念设研的并评估AI驱动型虚拟履标系统的用户开拓与产品市场，我们设计了以下问卷，通常能完善需要填写问卷，以便我们细致地准备的用者体验数，信息等发温排还感问及当的选，进一步优化方案，感谢您的支持！'
    },
    step1: {
      phone: '设备电话',
      phonePlaceholder: '11天即2312321321-',
      industry: '所属行业',
      industryPlaceholder: '制造业',
      salary: '月收入薪酬区',
      salaryPlaceholder: '1-5万+',
      experience: '拥有履历工程经',
      experiencePlaceholder: '100-500'
    },
    step2: {
      hasSystem: '现有是否有体的名（如无则填写）',
      systemPlaceholder: '是',
      years: '该有多年',
      yearsPlaceholder: '12',
      expire: '即将到期',
      expirePlaceholder: '2025-12-25',
      satisfied: '是否满意当前版型理面板',
      satisfiedPlaceholder: '是'
    },
    step3: {
      title: 'AI部落虚拟功能所求，可多选',
      subtitle: '属较以回复"人精选功能（思路）',
      options: [
        '根據入选检测（重复/敏感词入）',
        '简单理成反检测筛（农页/精读/完度）',
        '拖邮/物料自动计数'
      ],
      subtitle2: '业平民以响观"人精资功能（思选）',
      recommend: '温递/群众求职者档察',
      subtitle3: '拍反及组观"人精资功能（思选）',
      match: '女',
      subtitle4: '岗位概述及其察',
      job: '61-100道',
      subtitle5: '客察居点（思选）',
      lifecycle: '生成年间',
      subtitle6: '块适应团档察',
      nightShift: '夜班功能（红外/星光级）',
      subtitle7: '是无需套利存储体发功能',
      storage: '夜班功能（红外/星光级）',
      subtitle8: '是否需要附加地面信问',
      map: '是',
      subtitle9: '支持分享数',
      share: '58同城'
    },
    step4: {
      title: '项目整体规划规划',
      budget: '预算规模（万元）',
      budgetPlaceholder: '80-100',
      users: '预期使用使用排辑',
      usersPlaceholder: '一个月内',
      deployment: '最查需要技送项目',
      deploymentPlaceholder: '否',
      delivery: '具体执术选择时',
      deliveryPlaceholder: '2025-12-25'
    },
    buttons: {
      next: '下一步',
      complete: '完成'
    }
  },
  en: {
    title: 'CV Survey',
    tabs: ['Basic Info', 'Training System', 'Function Needs', 'Scene Planning'],
    welcome: {
      desc: 'Hello! To assist in the research and evaluation of AI-driven virtual resume system for users, we have designed the following questionnaire. Please complete the questionnaire so that we can better prepare for user experience and continuously optimize our solutions. Thank you for your support!'
    },
    step1: {
      phone: 'Phone Number',
      phonePlaceholder: '11-digit phone number',
      industry: 'Industry',
      industryPlaceholder: 'Manufacturing',
      salary: 'Monthly Salary Range',
      salaryPlaceholder: '1-5万+',
      experience: 'Resume Work Experience',
      experiencePlaceholder: '100-500'
    },
    step2: {
      hasSystem: 'Do you have an existing system',
      systemPlaceholder: 'Yes',
      years: 'Usage Years',
      yearsPlaceholder: '12',
      expire: 'Expiration Date',
      expirePlaceholder: '2025-12-25',
      satisfied: 'Satisfied with current panel',
      satisfiedPlaceholder: 'Yes'
    },
    step3: {
      title: 'AI Virtual Assistant Features',
      subtitle: 'Resume Screening Features',
      options: [
        'Resume Screening (Duplicate/Sensitive)',
        'Resume Quality Check',
        'Attachment Auto Count'
      ],
      subtitle2: 'Candidate Screening Features',
      recommend: 'Recommend Job Seekers',
      subtitle3: 'Position Matching Features',
      match: 'Female',
      subtitle4: 'Job Description Overview',
      job: '61-100 questions',
      subtitle5: 'Candidate Viewpoint',
      lifecycle: 'Generate Annual',
      subtitle6: 'Image Recognition',
      nightShift: 'Night Vision',
      subtitle7: 'Storage Function',
      storage: 'Night Vision',
      subtitle8: 'Additional Floor Info',
      map: 'Yes',
      subtitle9: 'Support Sharing',
      share: '58.com'
    },
    step4: {
      title: 'Project Planning',
      budget: 'Budget Scale (10k yuan)',
      budgetPlaceholder: '80-100',
      users: 'Expected Usage Schedule',
      usersPlaceholder: 'Within one month',
      deployment: 'Technical Support Project',
      deploymentPlaceholder: 'No',
      delivery: 'Technical Selection Time',
      deliveryPlaceholder: '2025-12-25'
    },
    buttons: {
      next: 'Next',
      complete: 'Complete'
    }
  }
};

const lang = ref('zh');
const step = ref(0);
const formData = ref({
  phone: '',
  industry: '',
  salary: '',
  experience: '',
  hasSystem: '',
  years: '',
  expire: '',
  satisfied: '',
  aiFeatures: [],
  recommend: '',
  match: '',
  job: '',
  lifecycle: '',
  nightShift: '',
  storage: '',
  map: '',
  share: '',
  budget: '',
  users: '',
  deployment: '',
  delivery: ''
});

const t = computed(() => translations[lang.value]);

const toggleLang = () => {
  lang.value = lang.value === 'zh' ? 'en' : 'zh';
};

const handleNext = () => {
  if (step.value < 3) {
    step.value++;
  }
};

const handleComplete = () => {
  const dataToSave = {
    ...formData.value,
    timestamp: new Date().toISOString(),
    language: lang.value
  };

  const existingData = localStorage.getItem('questionnaire_data');
  const dataArray = existingData ? JSON.parse(existingData) : [];
  dataArray.push(dataToSave);
  localStorage.setItem('questionnaire_data', JSON.stringify(dataArray));
  
  const jsonContent = JSON.stringify(dataArray, null, 2);
  const blob = new Blob([jsonContent], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'questions.json';
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
  
  alert(lang.value === 'zh' ? '问卷已提交并下载！' : 'Questionnaire submitted and downloaded!');
  
  formData.value = {
    phone: '', industry: '', salary: '', experience: '',
    hasSystem: '', years: '', expire: '', satisfied: '',
    aiFeatures: [], recommend: '', match: '', job: '',
    lifecycle: '', nightShift: '', storage: '', map: '',
    share: '', budget: '', users: '', deployment: '', delivery: ''
  };
  step.value = 0;
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

  background: #f5f5f5;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
  background: linear-gradient(226.22deg, #2855AE 0%, #7292CF 100%);
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
  background: rgba(255, 255, 255, 0.2);
  border: none;
  border-radius: 6px;
  padding: 6px;
  cursor: pointer;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
}

.lang-toggle:hover {
  background: rgba(255, 255, 255, 0.3);
}

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
  padding: 0 0 80px 0;
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

.form-section {
  background: white;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group:last-child {
  margin-bottom: 0;
}

.form-label {
  display: block;
  color: #666;
  font-size: 13px;
  margin-bottom: 8px;
}

.form-input {
  width: 100%;
  padding: 12px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  font-size: 14px;
  color: #333;
  background: white;
  outline: none;
  transition: border-color 0.2s;
}

.form-input:focus {
  border-color: #5b7fc7;
}

.form-input::placeholder {
  color: #999;
}

.select-container {
  position: relative;
}

.select-icon {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #999;
  pointer-events: none;
}

.section-subtitle {
  color: #999;
  font-size: 12px;
  margin-bottom: 12px;
}

.checkbox-list {
  margin-bottom: 20px;
}

.checkbox-label {
  display: flex;
  align-items: flex-start;
  padding: 10px 0;
  cursor: pointer;
}

.checkbox-input {
  margin-right: 10px;
  margin-top: 2px;
  cursor: pointer;
  flex-shrink: 0;
}

.checkbox-text {
  font-size: 13px;
  color: #333;
  line-height: 1.5;
}

/* Buttons */
.next-btn,
.complete-btn {
  width: calc(100% - 32px);
  margin: 0 16px;
  padding: 14px;
  background: #5b7fc7;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s;
}

.next-btn:hover,
.complete-btn:hover {
  background: #4a6bb5;
}

.complete-btn {
  background: #5b7fc7;
}

.complete-btn:hover {
  background: #4a6bb5;
}
</style>