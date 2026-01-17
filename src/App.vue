<template>
  <!-- 主内容区域 -->
  <div id="app" class="container">
    <!-- 背景图片容器 - 分为左右两部分 -->
    <div class="background-left"></div>
    <div class="background-right"></div>
    
    <!-- 落叶特效容器 -->
    <div class="falling-leaves">
      <div class="set">
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
      </div>
      <div class="set set-2">
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
      </div>
      <div class="set set-3">
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
      </div>
      <div class="set set-4">
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
        <div><img src="/images/leaves1.png" alt="落叶"></div>
        <div><img src="/images/leaves2.png" alt="落叶"></div>
        <div><img src="/images/leaves3.png" alt="落叶"></div>
        <div><img src="/images/leaves4.png" alt="落叶"></div>
      </div>
    </div>
    
    <!-- 卡片内容区域（透明化处理） -->
    <div class="card">
      <h1 class="title">MBTI 人格测试</h1>

      <!-- 进度条 -->
      <div class="progress-container">
        <div
          class="progress-bar"
          :style="{ width: (answeredCount / questions.length) * 100 + '%' }"
        ></div>
      </div>
      <p class="progress-text">已答 {{ answeredCount }} 题 / 共 {{ questions.length }} 题</p>
      
      <!-- 后门按钮 - 双击显示 -->
      <div 
        v-if="!isFinished && !showBackdoor"
        class="backdoor-hint"
        @dblclick="toggleBackdoor"
        title="双击显示调试选项"
      >
        <span>调试模式</span>
      </div>
      
      <!-- 后门控制面板 -->
      <div v-if="showBackdoor && !isFinished" class="backdoor-controls">
        <h3>调试面板</h3>
        <div class="backdoor-buttons">
          <button @click="showRandomResult" class="btn btn-backdoor random">
            随机结果
          </button>
          <button @click="showSpecificResult('ENFJ')" class="btn btn-backdoor enfj">
            ENFJ
          </button>
          <button @click="showSpecificResult('INTP')" class="btn btn-backdoor intp">
            INTP
          </button>
          <button @click="showSpecificResult('ISTJ')" class="btn btn-backdoor istj">
            ISTJ
          </button>
          <button @click="showSpecificResult('ESFP')" class="btn btn-backdoor esfp">
            ESFP
          </button>
          <button @click="hideBackdoor" class="btn btn-backdoor close">
            关闭面板
          </button>
        </div>
        <div class="backdoor-info">
          <p>当前分数: E{{scores.E}} I{{scores.I}} S{{scores.S}} N{{scores.N}} T{{scores.T}} F{{scores.F}} J{{scores.J}} P{{scores.P}}</p>
        </div>
      </div>

      <!-- 批量题目显示 -->
      <div v-if="!isFinished" class="questions-batch">
        <div 
          v-for="(question, index) in currentQuestions" 
          :key="question.id || currentQuestionIndex + index"
          class="question-item"
          :class="{ 'answered': userAnswers[currentQuestionIndex + index] }"
        >
          <h3 class="question-title">第 {{ currentQuestionIndex + index + 1 }} 题</h3>
          <p class="question-text">{{ question.question }}</p>
          
          <div class="options">
            <button 
              @click="answer(currentQuestionIndex + index, 'A')" 
              class="btn btn-option option-a"
              :class="{ 'selected': userAnswers[currentQuestionIndex + index] === 'A' }"
            >
              A. {{ question.optionA }}
            </button>
            <button 
              @click="answer(currentQuestionIndex + index, 'B')" 
              class="btn btn-option option-b"
              :class="{ 'selected': userAnswers[currentQuestionIndex + index] === 'B' }"
            >
              B. {{ question.optionB }}
            </button>
          </div>
        </div>
      </div>

      <!-- 结果页面 -->
      <div v-else class="result-container">
        <h2 class="result-title">测试完成！</h2>
        <div class="result-type">
          <span class="type-label">你的 MBTI 类型是：</span>
          <span class="type-value">{{ resultType }}</span>
        </div>
        
        <!-- 维度得分展示 -->
        <div class="dimension-scores">
          <h3>维度得分分析</h3>
          <div class="dimension-grid">
            <div class="dimension-item">
              <div class="dimension-label">
                <span class="dimension-name">外向 (E)</span>
                <span class="dimension-percent">{{ ePercentage }}%</span>
                <span class="dimension-name">内向 (I)</span>
                <span class="dimension-percent">{{ iPercentage }}%</span>
              </div>
              <div class="dimension-bar">
                <div class="dimension-fill e-fill" :style="{ width: ePercentage + '%' }"></div>
                <div class="dimension-fill i-fill" :style="{ width: iPercentage + '%' }"></div>
              </div>
            </div>
            
            <div class="dimension-item">
              <div class="dimension-label">
                <span class="dimension-name">实感 (S)</span>
                <span class="dimension-percent">{{ sPercentage }}%</span>
                <span class="dimension-name">直觉 (N)</span>
                <span class="dimension-percent">{{ nPercentage }}%</span>
              </div>
              <div class="dimension-bar">
                <div class="dimension-fill s-fill" :style="{ width: sPercentage + '%' }"></div>
                <div class="dimension-fill n-fill" :style="{ width: nPercentage + '%' }"></div>
              </div>
            </div>
            
            <div class="dimension-item">
              <div class="dimension-label">
                <span class="dimension-name">思考 (T)</span>
                <span class="dimension-percent">{{ tPercentage }}%</span>
                <span class="dimension-name">情感 (F)</span>
                <span class="dimension-percent">{{ fPercentage }}%</span>
              </div>
              <div class="dimension-bar">
                <div class="dimension-fill t-fill" :style="{ width: tPercentage + '%' }"></div>
                <div class="dimension-fill f-fill" :style="{ width: fPercentage + '%' }"></div>
              </div>
            </div>
            
            <div class="dimension-item">
              <div class="dimension-label">
                <span class="dimension-name">判断 (J)</span>
                <span class="dimension-percent">{{ jPercentage }}%</span>
                <span class="dimension-name">知觉 (P)</span>
                <span class="dimension-percent">{{ pPercentage }}%</span>
              </div>
              <div class="dimension-bar">
                <div class="dimension-fill j-fill" :style="{ width: jPercentage + '%' }"></div>
                <div class="dimension-fill p-fill" :style="{ width: pPercentage + '%' }"></div>
              </div>
            </div>
          </div>
        </div>
        
        <div class="result-description">
          <h3>人格描述</h3>
          <p>{{ resultDescription }}</p>
        </div>
        
        <!-- 后门按钮也在结果页面显示 -->
        <div v-if="isFinished" class="backdoor-result-controls">
          <button @click="showRandomResult" class="btn btn-backdoor random">
            随机结果
          </button>
          <button @click="restartTest" class="btn btn-restart">重新测试</button>
        </div>
      </div>

      <!-- 导航按钮 -->
      <div v-if="!isFinished" class="navigation">
        <button 
          v-if="currentQuestionIndex > 0"
          @click="previousBatch" 
          class="btn btn-nav btn-prev"
        >
          上一组
        </button>
        <button 
          @click="nextBatch" 
          class="btn btn-nav btn-next"
          :disabled="!canProceed"
        >
          {{ isLastBatch ? '查看结果' : '下一组' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import questions from '@/assets/mbti-questions.json'

export default {
  data() {
    return {
      started: true,
      currentQuestionIndex: 0, // 当前组的起始索引
      batchSize: 3, // 每组显示的题目数量
      scores: { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 },
      userAnswers: Array(questions.length).fill(null), // 初始化答案数组
      questions: questions,
      resultType: '',
      resultDescription: '',
      showBackdoor: false, // 控制后门面板显示
      descriptions: {
        'ISTJ': '物流师：严肃、负责、注重事实，善于组织与执行。',
        'ISFJ': '守护者：忠诚、体贴、有责任感，默默支持他人。',
        'INFJ': '倡导者：理想主义、富有洞察力，追求深层意义。',
        'INTJ': '战略家：独立、理性、有远见，擅长系统思考。',
        'ISTP': '探索者：冷静、务实、动手能力强，喜欢解决问题。',
        'ISFP': '创作者：温和、敏感、热爱美与自由，忠于内心。',
        'INFP': '调停者：理想主义、共情力强，追求真实与价值。',
        'INTP': '逻辑学家：好奇、分析力强，热爱抽象理论与逻辑。',
        'ESTP': '企业家：精力充沛、机智、行动派，活在当下。',
        'ESFP': '表演者：热情、随和、享受生活，善于带动气氛。',
        'ENFP': '竞选者：创意十足、热情洋溢，激励他人追梦。',
        'ENTP': '辩论家：聪明、好奇、思维敏捷，喜欢挑战观点。',
        'ESTJ': '总经理：务实、果断、有条理，重视效率与规则。',
        'ESFJ': '执政官：友善、负责、乐于助人，注重和谐与传统。',
        'ENFJ': '主人公：真诚、有领导力、鼓舞人心，关心他人成长。',
        'ENTJ': '指挥官：自信、果断、战略思维强，天生的领导者。'
      }
    }
  },
  computed: {
    // 当前显示的题目组
    currentQuestions() {
      const start = this.currentQuestionIndex;
      const end = Math.min(start + this.batchSize, this.questions.length);
      return this.questions.slice(start, end);
    },
    
    // 是否已完成所有题目
    isFinished() {
      return this.currentQuestionIndex >= this.questions.length;
    },
    
    // 是否最后一组题目
    isLastBatch() {
      return this.currentQuestionIndex + this.batchSize >= this.questions.length;
    },
    
    // 当前组所有题目是否都已回答
    currentBatchAnswered() {
      const start = this.currentQuestionIndex;
      const end = Math.min(start + this.batchSize, this.questions.length);
      
      // 检查当前组的所有题目是否都有答案
      for (let i = start; i < end; i++) {
        if (this.userAnswers[i] === null) {
          return false;
        }
      }
      return true;
    },
    
    // 是否可以进入下一组
    canProceed() {
      return this.currentBatchAnswered;
    },
    
    // 已回答的题目数量
    answeredCount() {
      return this.userAnswers.filter(answer => answer !== null).length;
    },
    
    // 计算每个维度的百分比
    ePercentage() {
      const total = this.scores.E + this.scores.I;
      return total > 0 ? Math.round((this.scores.E / total) * 100) : 0;
    },
    
    iPercentage() {
      const total = this.scores.E + this.scores.I;
      return total > 0 ? Math.round((this.scores.I / total) * 100) : 0;
    },
    
    sPercentage() {
      const total = this.scores.S + this.scores.N;
      return total > 0 ? Math.round((this.scores.S / total) * 100) : 0;
    },
    
    nPercentage() {
      const total = this.scores.S + this.scores.N;
      return total > 0 ? Math.round((this.scores.N / total) * 100) : 0;
    },
    
    tPercentage() {
      const total = this.scores.T + this.scores.F;
      return total > 0 ? Math.round((this.scores.T / total) * 100) : 0;
    },
    
    fPercentage() {
      const total = this.scores.T + this.scores.F;
      return total > 0 ? Math.round((this.scores.F / total) * 100) : 0;
    },
    
    jPercentage() {
      const total = this.scores.J + this.scores.P;
      return total > 0 ? Math.round((this.scores.J / total) * 100) : 0;
    },
    
    pPercentage() {
      const total = this.scores.J + this.scores.P;
      return total > 0 ? Math.round((this.scores.P / total) * 100) : 0;
    }
  },
  methods: {
    // 回答问题
    answer(questionIndex, choice) {
      // 先获取之前的答案（如果有的话）
      const previousAnswer = this.userAnswers[questionIndex];
      
      // 更新用户答案
      this.userAnswers[questionIndex] = choice;
      
      // 更新分数
      const q = this.questions[questionIndex];
      if (!q || !q.dimension || q.dimension.length !== 2) {
        console.warn('Invalid question or dimension:', q);
        return;
      }

      const dim = q.dimension;
      const dimA = dim[0]; // 例如 'E'
      const dimB = dim[1]; // 例如 'I'
      
      // 如果之前有答案，先撤销之前的分数
      if (previousAnswer !== null) {
        if (previousAnswer === 'A') {
          this.scores[dimA]--;
        } else if (previousAnswer === 'B') {
          this.scores[dimB]--;
        }
      }
      
      // 添加新选择的分数
      if (choice === 'A') {
        this.scores[dimA]++;
      } else if (choice === 'B') {
        this.scores[dimB]++;
      }
    },
    
    // 下一组题目
    nextBatch() {
      if (!this.canProceed) {
        alert('请先完成本组所有题目！');
        return;
      }
      
      if (this.isLastBatch) {
        // 如果是最后一组，计算并显示结果
        this.showResult();
      } else {
        // 跳转到下一组
        this.currentQuestionIndex += this.batchSize;
        // 滚动到顶部
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }
    },
    
    // 上一组题目
    previousBatch() {
      if (this.currentQuestionIndex > 0) {
        // 计算上一组的起始索引
        let prevIndex = this.currentQuestionIndex - this.batchSize;
        if (prevIndex < 0) {
          prevIndex = 0;
        }
        
        this.currentQuestionIndex = prevIndex;
        // 滚动到顶部
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }
    },
    
    // 显示结果
    showResult() {
      // 计算 MBTI 类型
      let type = '';
      type += this.scores.E >= this.scores.I ? 'E' : 'I';
      type += this.scores.S >= this.scores.N ? 'S' : 'N';
      type += this.scores.T >= this.scores.F ? 'T' : 'F';
      type += this.scores.J >= this.scores.P ? 'J' : 'P';
      
      this.resultType = type;
      this.resultDescription = this.descriptions[type] || '暂无描述';
      
      // 标记为已完成
      this.currentQuestionIndex = this.questions.length;
    },
    
    // 重新测试
    restartTest() {
      this.currentQuestionIndex = 0;
      this.scores = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };
      this.userAnswers = Array(this.questions.length).fill(null);
      this.resultType = '';
      this.resultDescription = '';
      this.showBackdoor = false;
    },
    
    // 显示/隐藏后门面板
    toggleBackdoor() {
      this.showBackdoor = !this.showBackdoor;
    },
    
    // 隐藏后门面板
    hideBackdoor() {
      this.showBackdoor = false;
    },
    
    // 显示随机结果
    showRandomResult() {
      // 随机选择一个MBTI类型
      const types = Object.keys(this.descriptions);
      const randomType = types[Math.floor(Math.random() * types.length)];
      
      // 为随机类型生成合理的分数
      const generateScoresForType = (type) => {
        const scores = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };
        const letters = type.split('');
        
        // 为每个维度设置分数
        letters.forEach((letter, index) => {
          const dimension = this.getDimensionByIndex(index);
          const opposite = this.getOppositeLetter(letter);
          
          // 当前倾向得分为6-10之间的随机数
          scores[letter] = Math.floor(Math.random() * 5) + 6;
          // 对立倾向得分为0-4之间的随机数
          scores[opposite] = Math.floor(Math.random() * 5);
        });
        
        return scores;
      };
      
      this.scores = generateScoresForType(randomType);
      this.resultType = randomType;
      this.resultDescription = this.descriptions[randomType];
      this.currentQuestionIndex = this.questions.length;
      this.showBackdoor = false;
      
      // 滚动到顶部
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },
    
    // 显示特定类型的结果
    showSpecificResult(type) {
      // 为特定类型生成合理的分数
      const generateScoresForType = (type) => {
        const scores = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };
        const letters = type.split('');
        
        // 为每个维度设置分数
        letters.forEach((letter, index) => {
          const dimension = this.getDimensionByIndex(index);
          const opposite = this.getOppositeLetter(letter);
          
          // 当前倾向得分为7-10之间的随机数
          scores[letter] = Math.floor(Math.random() * 4) + 7;
          // 对立倾向得分为0-3之间的随机数
          scores[opposite] = Math.floor(Math.random() * 4);
        });
        
        return scores;
      };
      
      this.scores = generateScoresForType(type);
      this.resultType = type;
      this.resultDescription = this.descriptions[type];
      this.currentQuestionIndex = this.questions.length;
      this.showBackdoor = false;
      
      // 滚动到顶部
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },
    
    // 根据索引获取维度
    getDimensionByIndex(index) {
      const dimensions = [
        ['E', 'I'],
        ['S', 'N'],
        ['T', 'F'],
        ['J', 'P']
      ];
      return dimensions[index] || ['E', 'I'];
    },
    
    // 获取对立字母
    getOppositeLetter(letter) {
      const opposites = {
        'E': 'I',
        'I': 'E',
        'S': 'N',
        'N': 'S',
        'T': 'F',
        'F': 'T',
        'J': 'P',
        'P': 'J'
      };
      return opposites[letter] || 'I';
    }
  },
  // 键盘快捷键
  mounted() {
    // 添加键盘事件监听
    window.addEventListener('keydown', this.handleKeyDown);
  },
  
  beforeDestroy() {
    // 移除键盘事件监听
    window.removeEventListener('keydown', this.handleKeyDown);
  }
}
</script>

<style>
/* 全局重置 */
*,
*::before,
*::after {
  box-sizing: border-box;
}

html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
  background-color: rgba(245, 247, 250, 0.8);
  overflow-y: auto;
  position: relative;
}

/* 背景图片分为左右两部分 */
.background-left {
  position: fixed;
  top: 0;
  left: 0;
  width: 40%; /* 左侧宽度 */
  height: 100%;
  background-image: url('/images/bk.png');
  background-size: cover;
  background-position: left center;
  background-repeat: no-repeat;
  z-index: 1;
  pointer-events: none;
  filter: brightness(0.9);
}

.background-right {
  position: fixed;
  top: 0;
  right: 0;
  width: 40%; /* 右侧宽度 */
  height: 100%;
  background-image: url('/images/bk.png');
  background-size: cover;
  background-position: right center;
  background-repeat: no-repeat;
  z-index: 1;
  pointer-events: none;
  filter: brightness(0.9);
}

/* 落叶特效样式 - 增加更多落叶并调整位置 */
.falling-leaves {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 2; /* 落叶在背景之上，但在内容之下 */
  overflow: hidden;
}

.set {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  pointer-events: none;
}

.set div {
  position: absolute;
  display: block;
  transform: scale(0.6);
}

.set div img {
  width: 70px; /* 减小落叶尺寸，让更多落叶可见 */
  height: auto;
  opacity: 0.8;
}

/* 设置12片落叶的位置和动画 */
.set div:nth-child(1) {
  left: 5%;
  animation: animate1 18s linear infinite;
  animation-delay: -3s;
}

.set div:nth-child(2) {
  left: 15%;
  animation: animate2 20s linear infinite;
  animation-delay: -5s;
}

.set div:nth-child(3) {
  left: 25%;
  animation: animate3 22s linear infinite;
  animation-delay: -7s;
}

.set div:nth-child(4) {
  left: 35%;
  animation: animate4 19s linear infinite;
  animation-delay: -2s;
}

.set div:nth-child(5) {
  left: 50%;
  animation: animate1 21s linear infinite;
  animation-delay: -6s;
}

.set div:nth-child(6) {
  left: 60%;
  animation: animate2 17s linear infinite;
  animation-delay: -4s;
}

.set div:nth-child(7) {
  left: 70%;
  animation: animate3 20s linear infinite;
  animation-delay: -1s;
}

.set div:nth-child(8) {
  left: 80%;
  animation: animate4 23s linear infinite;
  animation-delay: -8s;
}

.set div:nth-child(9) {
  left: 90%;
  animation: animate1 19s linear infinite;
  animation-delay: -5s;
}

.set div:nth-child(10) {
  left: 20%;
  animation: animate2 24s linear infinite;
  animation-delay: -9s;
}

.set div:nth-child(11) {
  left: 40%;
  animation: animate3 18s linear infinite;
  animation-delay: -3s;
}

.set div:nth-child(12) {
  left: 75%;
  animation: animate4 22s linear infinite;
  animation-delay: -6s;
}

/* 多种动画效果，增加落叶飘落的多样性 */
@keyframes animate1 {
  0% {
    opacity: 0;
    top: -10%;
    transform: translateX(15px) rotate(0deg);
  }
  10% {
    opacity: 0.8;
  }
  20% {
    transform: translateX(-10px) rotate(45deg);
  }
  40% {
    transform: translateX(-5px) rotate(90deg);
  }
  60% {
    transform: translateX(10px) rotate(135deg);
  }
  80% {
    transform: translateX(-8px) rotate(180deg);
  }
  100% {
    top: 110%;
    transform: translateX(-12px) rotate(225deg);
    opacity: 0;
  }
}

@keyframes animate2 {
  0% {
    opacity: 0;
    top: -15%;
    transform: translateX(-10px) rotate(30deg);
  }
  10% {
    opacity: 0.7;
  }
  30% {
    transform: translateX(12px) rotate(75deg);
  }
  50% {
    transform: translateX(-8px) rotate(120deg);
  }
  70% {
    transform: translateX(5px) rotate(165deg);
  }
  90% {
    transform: translateX(-15px) rotate(210deg);
  }
  100% {
    top: 115%;
    transform: translateX(8px) rotate(255deg);
    opacity: 0;
  }
}

@keyframes animate3 {
  0% {
    opacity: 0;
    top: -12%;
    transform: translateX(8px) rotate(-30deg);
  }
  15% {
    opacity: 0.6;
  }
  35% {
    transform: translateX(-12px) rotate(15deg);
  }
  55% {
    transform: translateX(7px) rotate(60deg);
  }
  75% {
    transform: translateX(-9px) rotate(105deg);
  }
  95% {
    transform: translateX(13px) rotate(150deg);
  }
  100% {
    top: 108%;
    transform: translateX(-5px) rotate(195deg);
    opacity: 0;
  }
}

@keyframes animate4 {
  0% {
    opacity: 0;
    top: -8%;
    transform: translateX(-5px) rotate(15deg);
  }
  12% {
    opacity: 0.9;
  }
  25% {
    transform: translateX(9px) rotate(60deg);
  }
  45% {
    transform: translateX(-11px) rotate(105deg);
  }
  65% {
    transform: translateX(6px) rotate(150deg);
  }
  85% {
    transform: translateX(-13px) rotate(195deg);
  }
  100% {
    top: 112%;
    transform: translateX(10px) rotate(240deg);
    opacity: 0;
  }
}

.set-2 {
  transform: scale(1.5) rotateY(180deg);
  filter: blur(1px);
  opacity: 0.7;
}

.set-3 {
  transform: scale(0.8) rotateY(180deg);
  filter: blur(2px);
  opacity: 0.6;
}

.set-4 {
  transform: scale(1.2) rotateY(180deg);
  filter: blur(1.5px);
  opacity: 0.5;
}

/* 主容器和卡片样式 - 透明化处理 */
#app {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  z-index: 3; /* 内容在最上层 */
  padding: 20px 0;
}

.container {
  width: 100%;
  max-width: 800px;
  padding: 0 20px;
  margin: 0 auto;
  position: relative;
}

/* 卡片透明化处理 */
.card {
  width: 100%;
  background: rgba(255, 255, 255, 0.85); /* 稍微提高透明度，让背景更清晰 */
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1); /* 轻微阴影 */
  padding: 32px;
  text-align: left;
  border: 1px solid rgba(233, 236, 239, 0.5); /* 半透明边框 */
  position: relative;
  z-index: 3;
  backdrop-filter: blur(3px); /* 减少模糊效果，让背景更清晰 */
}

/* 增强文字可读性 */
.title {
  font-size: 28px;
  font-weight: 700;
  color: #2c3e50; /* 深色文字 */
  margin-bottom: 24px;
  text-align: center;
  text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.8); /* 白色文字阴影增强可读性 */
}

.progress-container {
  height: 8px;
  background-color: rgba(224, 224, 224, 0.6);
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 8px;
}
.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, rgba(79, 172, 254, 0.8), rgba(0, 198, 251, 0.8));
  transition: width 0.3s ease;
}

.progress-text {
  text-align: center;
  color: #34495e; /* 深灰色文字 */
  font-size: 14px;
  margin-bottom: 32px;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

/* 后门提示样式 */
.backdoor-hint {
  text-align: center;
  margin-bottom: 20px;
  padding: 8px;
  background-color: rgba(79, 172, 254, 0.1);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 12px;
  color: #666;
  border: 1px dashed rgba(79, 172, 254, 0.3);
}

.backdoor-hint:hover {
  background-color: rgba(79, 172, 254, 0.2);
  border-color: rgba(79, 172, 254, 0.5);
}

.backdoor-hint span {
  opacity: 0.6;
}

/* 后门控制面板样式 */
.backdoor-controls {
  background-color: rgba(248, 249, 250, 0.9);
  border-radius: 12px;
  padding: 20px;
  margin-bottom: 24px;
  border: 2px solid rgba(79, 172, 254, 0.5);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.backdoor-controls h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 16px;
  font-size: 18px;
}

.backdoor-buttons {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-bottom: 16px;
}

.btn-backdoor {
  padding: 10px 12px;
  font-size: 13px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
  font-weight: 600;
  text-align: center;
}

.btn-backdoor:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.btn-backdoor.random {
  background: linear-gradient(90deg, rgba(79, 172, 254, 0.8), rgba(0, 198, 251, 0.8));
  color: white;
}

.btn-backdoor.enfj {
  background: linear-gradient(90deg, rgba(46, 204, 113, 0.8), rgba(39, 174, 96, 0.8));
  color: white;
}

.btn-backdoor.intp {
  background: linear-gradient(90deg, rgba(241, 196, 15, 0.8), rgba(243, 156, 18, 0.8));
  color: white;
}

.btn-backdoor.istj {
  background: linear-gradient(90deg, rgba(231, 76, 60, 0.8), rgba(192, 57, 43, 0.8));
  color: white;
}

.btn-backdoor.esfp {
  background: linear-gradient(90deg, rgba(155, 89, 182, 0.8), rgba(142, 68, 173, 0.8));
  color: white;
}

.btn-backdoor.close {
  background: linear-gradient(90deg, rgba(149, 165, 166, 0.8), rgba(127, 140, 141, 0.8));
  color: white;
  grid-column: span 3;
}

.backdoor-info {
  text-align: center;
  font-size: 12px;
  color: #666;
  padding-top: 10px;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
}

.backdoor-info p {
  margin: 0;
}

/* 批量题目样式 */
.questions-batch {
  display: flex;
  flex-direction: column;
  gap: 32px;
  margin-bottom: 32px;
}

.question-item {
  padding: 24px;
  border-radius: 12px;
  border: 2px solid rgba(233, 236, 239, 0.6);
  transition: all 0.3s ease;
  background: rgba(255, 255, 255, 0.7); /* 提高透明度，让背景更清晰 */
}

.question-item.answered {
  border-color: rgba(79, 172, 254, 0.7);
  background-color: rgba(79, 172, 254, 0.15);
}

.question-title {
  font-size: 16px;
  font-weight: 600;
  color: #2c3e50;
  margin-bottom: 12px;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

.question-text {
  font-size: 16px;
  color: #2c3e50;
  line-height: 1.6;
  margin-bottom: 20px;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

/* 选项样式 */
.options {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.btn {
  padding: 14px 16px;
  font-size: 14px;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
  font-weight: 500;
  outline: none;
  text-align: left;
  color: inherit;
}

.btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.option-a {
  background-color: rgba(230, 243, 255, 0.9);
  color: #1a365d;
  border: 2px solid rgba(184, 215, 255, 0.7);
}

.option-b {
  background-color: rgba(248, 242, 255, 0.9);
  color: #6a36d7;
  border: 2px solid rgba(226, 213, 255, 0.7);
}

.option-a.selected {
  background-color: rgba(79, 172, 254, 0.9);
  color: white;
  border-color: rgba(79, 172, 254, 0.9);
}

.option-b.selected {
  background-color: rgba(155, 135, 245, 0.9);
  color: white;
  border-color: rgba(155, 135, 245, 0.9);
}

/* 导航按钮样式 */
.navigation {
  display: flex;
  justify-content: space-between;
  margin-top: 32px;
}

.btn-nav {
  padding: 12px 28px;
  border-radius: 8px;
  font-weight: 600;
  text-align: center;
  min-width: 120px;
}

.btn-prev {
  background-color: rgba(248, 249, 250, 0.9);
  color: #495057;
  border: 2px solid rgba(222, 226, 230, 0.7);
}

.btn-prev:hover:not(:disabled) {
  background-color: rgba(233, 236, 239, 0.9);
}

.btn-next {
  background: linear-gradient(90deg, rgba(79, 172, 254, 0.8), rgba(0, 198, 251, 0.8));
  color: white;
  border: none;
}

.btn-next:disabled {
  background: rgba(204, 204, 204, 0.8);
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.btn-next:hover:not(:disabled) {
  background: linear-gradient(90deg, rgba(58, 156, 252, 0.9), rgba(0, 180, 240, 0.9));
}

/* 结果页面样式 */
.result-container {
  text-align: center;
  padding: 20px 0;
}

.result-title {
  font-size: 28px;
  color: #2c3e50;
  margin-bottom: 32px;
  text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.8);
}

.result-type {
  margin-bottom: 40px;
}

.type-label {
  font-size: 18px;
  color: #2c3e50;
  margin-right: 12px;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

.type-value {
  font-size: 36px;
  font-weight: 700;
  color: #4facfe;
  letter-spacing: 2px;
  text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.8);
}

/* 维度得分展示样式 */
.dimension-scores {
  background-color: rgba(248, 249, 250, 0.7);
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 32px;
  text-align: left;
}

.dimension-scores h3 {
  font-size: 18px;
  color: #2c3e50;
  margin-bottom: 20px;
  text-align: center;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

.dimension-grid {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.dimension-item {
  margin-bottom: 15px;
}

.dimension-label {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  font-size: 14px;
}

.dimension-name {
  color: #2c3e50;
  font-weight: 500;
}

.dimension-percent {
  color: #4facfe;
  font-weight: 600;
}

.dimension-bar {
  height: 16px;
  background-color: rgba(224, 224, 224, 0.6);
  border-radius: 8px;
  overflow: hidden;
  display: flex;
}

.dimension-fill {
  height: 100%;
  transition: width 0.5s ease;
}

/* 每个维度的不同颜色 */
.e-fill {
  background: linear-gradient(90deg, rgba(79, 172, 254, 0.8), rgba(0, 198, 251, 0.8));
}

.i-fill {
  background: linear-gradient(90deg, rgba(155, 135, 245, 0.8), rgba(186, 104, 200, 0.8));
}

.s-fill {
  background: linear-gradient(90deg, rgba(46, 204, 113, 0.8), rgba(39, 174, 96, 0.8));
}

.n-fill {
  background: linear-gradient(90deg, rgba(241, 196, 15, 0.8), rgba(243, 156, 18, 0.8));
}

.t-fill {
  background: linear-gradient(90deg, rgba(231, 76, 60, 0.8), rgba(192, 57, 43, 0.8));
}

.f-fill {
  background: linear-gradient(90deg, rgba(155, 89, 182, 0.8), rgba(142, 68, 173, 0.8));
}

.j-fill {
  background: linear-gradient(90deg, rgba(52, 152, 219, 0.8), rgba(41, 128, 185, 0.8));
}

.p-fill {
  background: linear-gradient(90deg, rgba(230, 126, 34, 0.8), rgba(211, 84, 0, 0.8));
}

.result-description {
  background-color: rgba(248, 249, 250, 0.7);
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 32px;
  text-align: left;
}

.result-description h3 {
  font-size: 18px;
  color: #2c3e50;
  margin-bottom: 16px;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

.result-description p {
  font-size: 16px;
  color: #34495e;
  line-height: 1.8;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

/* 结果页面后门按钮 */
.backdoor-result-controls {
  display: flex;
  justify-content: center;
  gap: 16px;
  margin-top: 24px;
}

.btn-restart {
  background: linear-gradient(90deg, rgba(79, 172, 254, 0.8), rgba(0, 198, 251, 0.8));
  color: white;
  padding: 14px 40px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
  margin-top: 20px;
}

.btn-restart:hover {
  background: linear-gradient(90deg, rgba(58, 156, 252, 0.9), rgba(0, 180, 240, 0.9));
  transform: translateY(-2px);
}

</style>