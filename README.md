<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>VocabMaster 英文聽讀大冒險</title>
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600&family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #4F46E5;
      --primary-light: #818CF8;
      --success: #10B981;
      --danger: #EF4444;
      --warning: #F59E0B;
      --bg: #F3F4F6;
      --card-bg: #FFFFFF;
      --text: #1F2937;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Fredoka', 'Noto Sans TC', sans-serif;
    }

    body {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      color: var(--text);
    }

    .container {
      background: var(--card-bg);
      width: 100%;
      max-width: 600px;
      border-radius: 24px;
      padding: 28px;
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
      text-align: center;
      position: relative;
    }

    header h1 {
      font-size: 1.8rem;
      color: var(--primary);
      margin-bottom: 6px;
    }

    /* 頂部數據看板 */
    .dashboard {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      margin: 20px 0;
      background: #EEF2FF;
      padding: 12px;
      border-radius: 16px;
    }

    .stat-item {
      display: flex;
      flex-direction: column;
    }

    .stat-label {
      font-size: 0.75rem;
      color: #6B7280;
    }

    .stat-val {
      font-size: 1.2rem;
      font-weight: 700;
      color: var(--primary);
    }

    /* 模式切換 */
    .mode-switch {
      display: flex;
      background: #E5E7EB;
      border-radius: 12px;
      padding: 4px;
      margin-bottom: 24px;
    }

    .mode-btn {
      flex: 1;
      border: none;
      padding: 10px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      background: transparent;
      color: #4B5563;
      transition: all 0.3s ease;
    }

    .mode-btn.active {
      background: white;
      color: var(--primary);
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    /* 卡片主體 */
    .card-display {
      min-height: 150px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      margin-bottom: 20px;
      padding: 20px;
      border: 2px dashed #E0E7FF;
      border-radius: 16px;
      background: #FAFAFA;
    }

    .word-text {
      font-size: 2.3rem;
      font-weight: 700;
      color: #111827;
      margin-bottom: 8px;
    }

    .word-translation {
      font-size: 1.1rem;
      color: #6B7280;
    }

    /* 互動按鈕群 */
    .action-controls {
      display: flex;
      justify-content: center;
      gap: 12px;
      margin-bottom: 20px;
    }

    .btn {
      border: none;
      border-radius: 12px;
      padding: 12px 20px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: transform 0.1s ease, filter 0.2s;
    }

    .btn:active {
      transform: scale(0.96);
    }

    .btn-audio {
      background: #EEF2FF;
      color: var(--primary);
    }

    .btn-mic {
      background: var(--primary);
      color: white;
    }

    .btn-mic.listening {
      background: var(--danger);
      animation: pulse 1.2s infinite;
    }

    .btn-next {
      background: #10B981;
      color: white;
    }

    /* 聽力模式選擇題區域 */
    .options-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin-bottom: 20px;
    }

    .option-btn {
      background: white;
      border: 2px solid #E5E7EB;
      border-radius: 12px;
      padding: 14px;
      font-size: 1rem;
      cursor: pointer;
      font-weight: 600;
      transition: all 0.2s;
    }

    .option-btn:hover {
      border-color: var(--primary);
      background: #F5F3FF;
    }

    .option-btn.correct {
      background: var(--success);
      color: white;
      border-color: var(--success);
    }

    .option-btn.wrong {
      background: var(--danger);
      color: white;
      border-color: var(--danger);
    }

    /* 語音辨識即時回饋 */
    .feedback-box {
      min-height: 44px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.95rem;
      font-weight: 600;
      border-radius: 8px;
      background: #F3F4F6;
      margin-bottom: 16px;
      padding: 8px 16px;
    }

    .streak-badge {
      position: absolute;
      top: 24px;
      right: 28px;
      background: #FEF3C7;
      color: #D97706;
      padding: 6px 12px;
      border-radius: 20px;
      font-size: 0.85rem;
      font-weight: 700;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    @keyframes pulse {
      0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.6); }
      70% { box-shadow: 0 0 0 12px rgba(239, 68, 68, 0); }
      100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
    }
  </style>
</head>
<body>

  <div class="container">
    <div class="streak-badge">🔥 <span id="streakCount">0</span> 連勝</div>
    <header>
      <h1>VocabMaster 🎧</h1>
      <p style="color:#6B7280; font-size:0.9rem;">高頻單字片語 100 挑戰</p>
    </header>

    <!-- 儀表板 -->
    <div class="dashboard">
      <div class="stat-item">
        <span class="stat-label">已練習</span>
        <span class="stat-val" id="totalAttempts">0</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">正確率</span>
        <span class="stat-val" id="accuracy">0%</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">EXP</span>
        <span class="stat-val" id="expPoints">0</span>
      </div>
      <div class="stat-item">
        <span class="stat-label">等級</span>
        <span class="stat-val" id="level">Lv.1</span>
      </div>
    </div>

    <!-- 模式切換 -->
    <div class="mode-switch">
      <button class="mode-btn active" id="btnModeSpeak" onclick="setMode('speak')">🗣 口說挑戰</button>
      <button class="mode-btn" id="btnModeListen" onclick="setMode('listen')">👂 聽力辨識</button>
    </div>

    <!-- 單字展示卡 -->
    <div class="card-display">
      <div class="word-text" id="displayWord">apple</div>
      <div class="word-translation" id="displayTrans">蘋果</div>
    </div>

    <!-- 聽力模式專用：4 選 1 選項 -->
    <div class="options-grid" id="optionsContainer" style="display: none;"></div>

    <!-- 即時辨識或提示回饋 -->
    <div class="feedback-box" id="feedbackText">點擊下方按鈕開始練習！</div>

    <!-- 控制區 -->
    <div class="action-controls">
      <button class="btn btn-audio" onclick="playAudio()">🔊 發音</button>
      <button class="btn btn-mic" id="recordBtn" onclick="toggleRecognition()">🎤 說出這個詞</button>
      <button class="btn btn-next" onclick="nextWord()">下一題 ➔</button>
    </div>
  </div>

  <script>
    // 100 個高頻詞彙與片語清單
    const VOCAB_LIST = [
      { word: "opportunity", trans: "機會" },
      { word: "resilient", trans: "有韌性的" },
      { word: "inspire", trans: "激勵" },
      { word: "curious", trans: "好奇的" },
      { word: "efficient", trans: "高效率的" },
      { word: "perspective", trans: "觀點；角度" },
      { word: "collaborate", trans: "合作" },
      { word: "essential", trans: "必要的；不可或缺的" },
      { word: "innovative", trans: "創新的" },
      { word: "strategy", trans: "策略" },
      { word: "determine", trans: "決定；下定決心" },
      { word: "confidence", trans: "自信" },
      { word: "challenge", trans: "挑戰" },
      { word: "solution", trans: "解決方案" },
      { word: "creativity", trans: "創造力" },
      { word: "patience", trans: "耐心" },
      { word: "journey", trans: "旅程" },
      { word: "accomplish", trans: "達成；完成" },
      { word: "potential", trans: "潛力" },
      { word: "attitude", trans: "態度" },
      { word: "break a leg", trans: "祝你好運" },
      { word: "bite the bullet", trans: "咬緊牙關" },
      { word: "call it a day", trans: "今天到此為止" },
      { word: "piece of cake", trans: "輕而易舉的事" },
      { word: "hit the books", trans: "認真讀書" },
      { word: "under the weather", trans: "身體不舒服" },
      { word: "on the fence", trans: "猶豫不決" },
      { word: "spill the beans", trans: "洩漏秘密" },
      { word: "see eye to eye", trans: "意見一致" },
      { word: "once in a blue moon", trans: "千載難逢；極為罕見" },
      { word: "enthusiasm", trans: "熱情" },
      { word: "priority", trans: "優先事項" },
      { word: "adaptable", trans: "適應力強的" },
      { word: "negotiate", trans: "協商" },
      { word: "empathy", trans: "同理心" },
      { word: "generous", trans: "大方的" },
      { word: "optimistic", trans: "樂觀的" },
      { word: "reliable", trans: "可靠的" },
      { word: "sustainable", trans: "永續的" },
      { word: "diversity", trans: "多樣性" },
      { word: "figure out", trans: "理解；弄清楚" },
      { word: "give up", trans: "放棄" },
      { word: "look forward to", trans: "期待" },
      { word: "put off", trans: "拖延" },
      { word: "run out of", trans: "耗盡；用完" },
      { word: "turn down", trans: "拒絕" },
      { word: "bring up", trans: "提出；撫養" },
      { word: "catch up", trans: "趕上進度" },
      { word: "carry on", trans: "繼續" },
      { word: "find out", trans: "查明；發現" },
      { word: "gratitude", trans: "感激之情" },
      { word: "authentic", trans: "真實的；正宗的" },
      { word: "adventure", trans: "冒險" },
      { word: "harmony", trans: "和諧" },
      { word: "wisdom", trans: "智慧" },
      { word: "courage", trans: "勇氣" },
      { word: "humble", trans: "謙虛的" },
      { word: "passion", trans: "熱情" },
      { word: "curiosity", trans: "好奇心" },
      { word: "integrity", trans: "誠實正直" },
      { word: "keep in mind", trans: "記在心上" },
      { word: "make sense", trans: "有道理" },
      { word: "take it easy", trans: "放輕鬆" },
      { word: "up in the air", trans: "懸而未決" },
      { word: "so far so good", trans: "到目前為止都還好" },
      { word: "speak of the devil", trans: "說曹操，曹操到" },
      { word: "cut corners", trans: "偷工減料；走捷徑" },
      { word: "better late than never", trans: "亡羊補牢；遲到總比不到好" },
      { word: "burn the midnight oil", trans: "熬夜" },
      { word: "cost an arm and a leg", trans: "價格極其昂貴" },
      { word: "flexibility", trans: "彈性；靈活性" },
      { word: "motivation", trans: "動力" },
      { word: "feedback", trans: "回饋" },
      { word: "achievement", trans: "成就" },
      { word: "perseverance", trans: "毅力；堅持不懈" },
      { word: "visionary", trans: "有遠見的" },
      { word: "diligent", trans: "勤奮的" },
      { word: "thoughtful", trans: "體貼的" },
      { word: "proactive", trans: "積極主動的" },
      { word: "independence", trans: "獨立" },
      { word: "step by step", trans: "按部就班" },
      { word: "out of the blue", trans: "出乎意料地；突如其來" },
      { word: "in a nutshell", trans: "簡而言之" },
      { word: "hang in there", trans: "堅持下去" },
      { word: "get out of hand", trans: "失去控制" },
      { word: "beat around the bush", trans: "說話拐彎抹角" },
      { word: "back to the drawing board", trans: "從頭再來" },
      { word: "break the ice", trans: "打破冷場" },
      { word: "by the book", trans: "按規則行事" },
      { word: "through thick and thin", trans: "同甘共苦" },
      { word: "clarity", trans: "清晰；明確" },
      { word: "balance", trans: "平衡" },
      { word: "focus", trans: "專注" },
      { word: "curiosity", trans: "求知慾" },
      { word: "mindfulness", trans: "正念" },
      { word: "progress", trans: "進步" },
      { word: "curiosity", trans: "探究精神" },
      { word: "enthusiasm", trans: "幹勁" },
      { word: "reflection", trans: "反思" },
      { word: "dedication", trans: "奉獻；專注" }
    ];

    let currentMode = 'speak'; // 'speak' 或 'listen'
    let currentIndex = 0;
    let correctCount = 0;
    let totalCount = 0;
    let streak = 0;
    let exp = 0;
    let isListening = false;

    // Web Speech API
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    let recognition = null;
    const synth = window.speechSynthesis;

    // 檢查瀏覽器辨識支援
    if (SpeechRecognition) {
      recognition = new SpeechRecognition();
      recognition.lang = 'en-US';
      recognition.continuous = false;
      recognition.interimResults = false;

      recognition.onstart = () => {
        isListening = true;
        const btn = document.getElementById('recordBtn');
        btn.classList.add('listening');
        btn.innerText = '🔴 聆聽中...請朗讀';
        showFeedback("正在聽您說話，請清楚讀出螢幕上的單字...", "#6B7280");
      };

      recognition.onresult = (event) => {
        const spokenWord = event.results[0][0].transcript.toLowerCase().trim();
        validateSpeech(spokenWord);
      };

      recognition.onerror = (e) => {
        showFeedback(`辨識未成功：${e.error}，請再試一次`, "var(--danger)");
        resetMicBtn();
      };

      recognition.onend = () => {
        resetMicBtn();
      };
    }

    function resetMicBtn() {
      isListening = false;
      const btn = document.getElementById('recordBtn');
      btn.classList.remove('listening');
      btn.innerText = '🎤 說出這個詞';
    }

    // 發音功能 (SpeechSynthesis)
    function playAudio(customText = null) {
      if (!synth) return;
      synth.cancel(); // 停止先前的發音
      const textToRead = customText || VOCAB_LIST[currentIndex].word;
      const utterance = new SpeechSynthesisUtterance(textToRead);
      utterance.lang = 'en-US';
      utterance.rate = 0.9; // 稍慢更清楚
      synth.speak(utterance);
    }

    // 模式設定
    function setMode(mode) {
      currentMode = mode;
      document.getElementById('btnModeSpeak').classList.toggle('active', mode === 'speak');
      document.getElementById('btnModeListen').classList.toggle('active', mode === 'listen');
      
      const recordBtn = document.getElementById('recordBtn');
      const optionsBox = document.getElementById('optionsContainer');

      if (mode === 'speak') {
        recordBtn.style.display = 'inline-flex';
        optionsBox.style.display = 'none';
      } else {
        recordBtn.style.display = 'none';
        optionsBox.style.display = 'grid';
      }
      loadCurrentQuestion();
    }

    // 載入題目
    function loadCurrentQuestion() {
      const current = VOCAB_LIST[currentIndex];
      const wordText = document.getElementById('displayWord');
      const transText = document.getElementById('displayTrans');
      const optionsBox = document.getElementById('optionsContainer');

      if (currentMode === 'speak') {
        wordText.innerText = current.word;
        transText.innerText = current.trans;
        showFeedback("點擊麥克風並朗讀單字", "#6B7280");
      } else {
        // 聽力模式：遮蔽文字，僅給發音與選項
        wordText.innerText = "❓ 聽音辨字";
        transText.innerText = "點選正確的單字選項";
        generateOptions(current);
        playAudio(); // 自動播音一次
        showFeedback("請仔細聽發音，選出正確的單字", "#6B7280");
      }
    }

    // 隨機產生 4 個聽力選項
    function generateOptions(correctItem) {
      const optionsBox = document.getElementById('optionsContainer');
      optionsBox.innerHTML = '';
      
      let options = [correctItem];
      while (options.length < 4) {
        let rand = VOCAB_LIST[Math.floor(Math.random() * VOCAB_LIST.length)];
        if (!options.some(opt => opt.word === rand.word)) {
          options.push(rand);
        }
      }
      // 洗牌
      options.sort(() => Math.random() - 0.5);

      options.forEach(item => {
        const btn = document.createElement('button');
        btn.className = 'option-btn';
        btn.innerText = `${item.word}\n(${item.trans})`;
        btn.onclick = () => validateOption(btn, item.word === correctItem.word);
        optionsBox.appendChild(btn);
      });
    }

    // 檢查口說辨識
    function validateSpeech(spoken) {
      const target = VOCAB_LIST[currentIndex].word.toLowerCase();
      // 容錯比對：去標點符號
      const cleanSpoken = spoken.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g, "").trim();
      const cleanTarget = target.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g, "").trim();

      if (cleanSpoken === cleanTarget) {
        handleSuccess(`🎉 太棒了！您讀的是 "${spoken}"，完全正確！`);
      } else {
        handleFailure(`聽到的是 "${spoken}"，請再試一次或聽發音示範！`);
      }
    }

    // 檢查聽力點選
    function validateOption(button, isCorrect) {
      const allBtns = document.querySelectorAll('.option-btn');
      allBtns.forEach(b => b.disabled = true);

      if (isCorrect) {
        button.classList.add('correct');
        handleSuccess("🎉 答對了！耳朵太靈敏了！");
      } else {
        button.classList.add('wrong');
        allBtns.forEach(b => {
          if (b.innerText.includes(VOCAB_LIST[currentIndex].word)) {
            b.classList.add('correct');
          }
        });
        handleFailure("答錯囉，點擊發音多聽幾次複習吧！");
      }
    }

    // 回饋與分數統計
    function handleSuccess(msg) {
      correctCount++;
      totalCount++;
      streak++;
      exp += 15 + streak * 2;
      showFeedback(msg, "var(--success)");
      updateStats();
    }

    function handleFailure(msg) {
      totalCount++;
      streak = 0;
      showFeedback(msg, "var(--danger)");
      updateStats();
    }

    function updateStats() {
      document.getElementById('totalAttempts').innerText = totalCount;
      const rate = totalCount === 0 ? 0 : Math.round((correctCount / totalCount) * 100);
      document.getElementById('accuracy').innerText = `${rate}%`;
      document.getElementById('streakCount').innerText = streak;
      document.getElementById('expPoints').innerText = exp;
      document.getElementById('level').innerText = `Lv.${Math.floor(exp / 100) + 1}`;
    }

    function showFeedback(text, color) {
      const fb = document.getElementById('feedbackText');
      fb.innerText = text;
      fb.style.color = color;
    }

    // 麥克風切換
    function toggleRecognition() {
      if (!SpeechRecognition) {
        alert("很抱歉，您的瀏覽器不支援語音辨識 API，建議使用 Google Chrome 或 Edge。");
        return;
      }
      if (isListening) {
        recognition.stop();
      } else {
        recognition.start();
      }
    }

    // 下一題
    function nextWord() {
      currentIndex = (currentIndex + 1) % VOCAB_LIST.length;
      loadCurrentQuestion();
    }

    // 初始化
    loadCurrentQuestion();
  </script>
</body>
</html>
