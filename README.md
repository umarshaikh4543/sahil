<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Online Exam | 25 Objective + Practical (75 Marks)</title>
    <!-- JSZip library for password-protected ZIP export -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <!-- FileSaver.js to trigger download -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', 'Poppins', 'Roboto', sans-serif;
        }
        body {
            background: linear-gradient(145deg, #eef2f7 0%, #d9e0e8 100%);
            padding: 30px 20px;
        }
        .exam-container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 42px;
            box-shadow: 0 25px 45px -12px rgba(0,0,0,0.3);
            overflow: hidden;
            backdrop-filter: blur(0px);
        }
        /* header */
        .exam-header {
            background: #0a2b3e;
            padding: 1.5rem 2rem;
            color: white;
            border-bottom: 5px solid #ffb347;
        }
        .exam-header h1 {
            font-size: 1.9rem;
            font-weight: 700;
            letter-spacing: -0.3px;
        }
        .exam-header p {
            font-size: 1rem;
            opacity: 0.9;
            margin-top: 8px;
        }
        .marks-badge {
            display: inline-block;
            background: #ffb347;
            color: #0a2b3e;
            padding: 5px 14px;
            border-radius: 50px;
            font-weight: bold;
            margin-top: 12px;
            font-size: 0.9rem;
        }
        /* two column layout */
        .exam-layout {
            display: flex;
            flex-wrap: wrap;
        }
        .objective-section {
            flex: 2;
            min-width: 280px;
            background: #fefefe;
            border-right: 1px solid #e2e8f0;
            padding: 25px 20px;
            max-height: 85vh;
            overflow-y: auto;
        }
        .practical-section {
            flex: 1.3;
            min-width: 320px;
            background: #f9fbfd;
            padding: 25px 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        @media (max-width: 900px) {
            .exam-layout {
                flex-direction: column;
            }
            .objective-section {
                border-right: none;
                border-bottom: 2px solid #e2e8f0;
                max-height: 60vh;
            }
        }
        /* objective questions styling */
        .question-card {
            background: white;
            border-radius: 24px;
            padding: 18px 20px;
            margin-bottom: 20px;
            box-shadow: 0 5px 12px rgba(0,0,0,0.03);
            border: 1px solid #e9edf2;
            transition: all 0.2s;
        }
        .question-card:hover {
            border-color: #cbd5e1;
            box-shadow: 0 8px 18px rgba(0,0,0,0.05);
        }
        .q-text {
            font-weight: 700;
            font-size: 1rem;
            color: #0f2c3d;
            margin-bottom: 12px;
            display: flex;
            gap: 8px;
        }
        .q-num {
            background: #0a2b3e;
            color: white;
            width: 28px;
            height: 28px;
            border-radius: 30px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-size: 0.85rem;
            font-weight: bold;
            flex-shrink: 0;
        }
        .options {
            display: flex;
            flex-wrap: wrap;
            gap: 16px;
            margin-left: 8px;
            padding-left: 8px;
        }
        .option label {
            margin-left: 6px;
            font-weight: 500;
            color: #2c3e44;
            cursor: pointer;
        }
        .option input {
            cursor: pointer;
            transform: scale(1.05);
            accent-color: #0a2b3e;
        }
        hr {
            margin: 12px 0;
            border-color: #eef2f8;
        }
        /* practical area */
        .practical-card {
            background: white;
            border-radius: 28px;
            padding: 1.5rem;
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
            border: 1px solid #e2edf2;
        }
        .prac-title {
            font-size: 1.6rem;
            font-weight: 800;
            background: linear-gradient(135deg, #1e4b6e, #0f2c3d);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 8px;
        }
        .prac-marks {
            background: #f0f4f9;
            padding: 6px 12px;
            border-radius: 40px;
            font-weight: 600;
            font-size: 0.9rem;
            display: inline-block;
            margin-bottom: 16px;
            color: #0a2b3e;
        }
        .question-desc {
            background: #eef3fc;
            padding: 14px;
            border-radius: 20px;
            margin: 15px 0;
            font-weight: 500;
            border-left: 5px solid #ffb347;
        }
        textarea {
            width: 100%;
            padding: 16px;
            font-family: 'Courier New', 'Fira Code', monospace;
            font-size: 14px;
            border-radius: 20px;
            border: 1.5px solid #cbdde9;
            background: #fefefe;
            resize: vertical;
            transition: 0.2s;
        }
        textarea:focus {
            border-color: #0a2b3e;
            outline: none;
            box-shadow: 0 0 0 3px rgba(10,43,62,0.2);
        }
        button {
            background: #0a2b3e;
            color: white;
            border: none;
            padding: 14px 24px;
            border-radius: 60px;
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.2s;
            width: 100%;
            margin-top: 12px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        button:hover {
            background: #ff8c1a;
            color: #0a2b3e;
            transform: translateY(-2px);
        }
        .secondary-btn {
            background: #e2e8f0;
            color: #1e2f3a;
            margin-top: 8px;
        }
        .secondary-btn:hover {
            background: #cbd5e1;
            transform: none;
        }
        .export-btn {
            background: #2c5f2d;
            margin-top: 10px;
        }
        .export-btn:hover {
            background: #1f4a20;
            color: white;
        }
        .result-area {
            background: #eef2fa;
            border-radius: 28px;
            padding: 18px;
            margin-top: 20px;
            border: 1px solid #cde0ed;
        }
        .result-head {
            font-weight: 800;
            font-size: 1.3rem;
            margin-bottom: 12px;
            color: #0a2b3e;
            border-left: 5px solid #ffb347;
            padding-left: 12px;
        }
        .result-stats {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 12px;
            font-weight: 500;
        }
        .stat-card {
            background: white;
            border-radius: 20px;
            padding: 12px 16px;
            flex: 1;
            text-align: center;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
        }
        .prac-breakdown {
            font-size: 0.85rem;
            background: #ffffffcc;
            padding: 12px;
            border-radius: 20px;
            margin-top: 8px;
        }
        footer {
            text-align: center;
            padding: 16px;
            font-size: 0.8rem;
            background: #f1f5f9;
            color: #2c5368;
            border-top: 1px solid #dce5ec;
        }
        .warning {
            color: #c2410c;
            font-size: 0.8rem;
            margin-top: 8px;
        }
        .obj-header-actions {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 16px;
        }
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #e2e8f0;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #8ba0ae;
            border-radius: 10px;
        }
    </style>
</head>
<body>

<div class="exam-container">
    <div class="exam-header">
        <h1>📋 वेब डेव्हलपमेंट परीक्षा</h1>
        <p>25 वस्तुनिष्ठ प्रश्न (25 गुण) + 1 व्यावहारिक प्रश्न (75 गुण) | एकूण 100 गुण</p>
        <span class="marks-badge">✨ उत्तरे Auto-Save होतात | वस्तुनिष्ठ निकाल password-protected ZIP मध्ये export करा (skill123)</span>
    </div>

    <div class="exam-layout">
        <!-- 25 Objective Questions -->
        <div class="objective-section" id="objectiveSection">
            <div class="obj-header-actions">
                <h3>✅ वस्तुनिष्ठ प्रश्न (प्रत्येकी 1 गुण)</h3>
                <div style="display: flex; gap: 8px;">
                    <button type="button" id="clearObjBtn" class="secondary-btn" style="width: auto; padding: 6px 18px; margin:0;">सर्व रीसेट करा</button>
                    <button type="button" id="exportObjBtn" class="export-btn" style="width: auto; padding: 6px 18px; margin:0; background:#1e5631;">📥 Export (Password: skill123)</button>
                </div>
            </div>
            <div id="questionsContainer"></div>
        </div>

        <!-- Practical + Result -->
        <div class="practical-section">
            <div class="practical-card">
                <div class="prac-title">💻 व्यावहारिक प्रश्न (75 Marks)</div>
                <span class="prac-marks">🎯 75 गुणांसाठी कोड लिहा</span>
                <div class="question-desc">
                    <strong>📌 प्रश्न:</strong> "पर्सनल पोर्टफोलिओ कार्ड" तयार करा. HTML व CSS वापरून प्रोफाइल इमेज, नाव, बायो आणि सोशल लिंक्स दाखवा. रिस्पॉन्सिव्ह असावे. (इनलाइन किंवा इंटरनल CSS)
                </div>
                <label style="font-weight: 600; margin-top: 5px;">✍️ तुमचा HTML/CSS कोड इथे टाइप करा:</label>
                <textarea id="practicalCodeArea" rows="14" placeholder="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;My Portfolio&lt;/title&gt;
    &lt;style&gt;
        /* CSS here */
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class='card'&gt;
        &lt;img src='profile.jpg'&gt;
        &lt;h1&gt;Your Name&lt;/h1&gt;
        &lt;p&gt;Bio...&lt;/p&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;"></textarea>
                <div class="warning">💡 टीप: प्रॅक्टिकल मूल्यांकन 5 क्रायटेरियावर आधारित (प्रत्येकी 15 गुण) : HTML स्ट्रक्चर, img टॅग, हेडिंग, पॅराग्राफ, CSS स्टाइलिंग.</div>
            </div>

            <button id="submitExamBtn">📥 परीक्षा सबमिट करा & निकाल पहा</button>
            
            <!-- Result Section -->
            <div class="result-area" id="resultArea">
                <div class="result-head">📊 परीक्षा निकाल</div>
                <div id="resultContent">
                    <p style="color: #3b5c74;">सबमिट केल्यानंतर गुण दिसतील...</p>
                </div>
            </div>
        </div>
    </div>
    <footer>© ऑनलाइन परीक्षा प्रणाली | प्रत्येक उत्तर स्वयं सुरक्षित (auto-save) | वस्तुनिष्ठ निकाल export → password: skill123</footer>
</div>

<script>
    // ---------- 25 OBJECTIVE QUESTIONS (MCQ) ----------
    const questionsData = [
        { text: "HTML चे पूर्ण रूप काय आहे?", options: ["Hyper Text Markup Language", "High Tech Modern Language", "Hyper Transfer Markup Language", "Hyperlink Text Markup Language"], correct: 0 },
        { text: "CSS मध्ये background color बदलण्यासाठी कोणता property वापरतात?", options: ["background-color", "color", "bgcolor", "background"], correct: 0 },
        { text: "अक्रमित यादी (unordered list) साठी कोणता टॅग वापरतात?", options: ["<ul>", "<ol>", "<li>", "<list>"], correct: 0 },
        { text: "CSS चे फुल फॉर्म काय आहे?", options: ["Creative Style Sheets", "Cascading Style Sheets", "Computer Style Sheets", "Colorful Style Sheets"], correct: 1 },
        { text: "JavaScript मध्ये व्हेरिएबल घोषित करण्यासाठी योग्य पर्याय कोणता?", options: ["var", "let", "const", "हे सर्व"], correct: 3 },
        { text: "HTML मध्ये सर्वात मोठे heading कोणते?", options: ["<h1>", "<h6>", "<heading>", "<head>"], correct: 0 },
        { text: "इमेज दाखवण्यासाठी कोणता टॅग वापरतात?", options: ["<img>", " MentionsView", "<pic>", "<src>"], correct: 0 },
        { text: "CSS मध्ये बॉर्डर गोलाकार (rounded) करण्यासाठी कोणता property वापरतात?", options: ["border-radius", "border-round", "corner-radius", "border-circle"], correct: 0 },
        { text: "JavaScript मध्ये 'console.log()' चा उपयोग काय?", options: ["डीबगिंगसाठी", "वापरकर्त्यास संदेश", "फाइल सेव्ह करणे", "गणितीय क्रिया"], correct: 0 },
        { text: "Flexbox मध्ये आयटम्स horizontal केंद्रित करण्यासाठी कोणता गुणधर्म?", options: ["justify-content: center", "align-items: center", "text-align: center", "flex-center"], correct: 0 },
        { text: "HTML मध्ये लिंक तयार करण्यासाठी कोणता टॅग?", options: ["<a>", "<link>", "<href>", "<url>"], correct: 0 },
        { text: "CSS Grid लेआउटसाठी कंटेनरवर कोणता display वापरतात?", options: ["display: grid", "display: flex", "display: block", "display: inline"], correct: 0 },
        { text: "JavaScript function परिभाषित करण्यासाठी कोणता कीवर्ड?", options: ["function", "def", "func", "define"], correct: 0 },
        { text: "HTML मध्ये फॉर्म submit बटणासाठी कोणता type वापरतात?", options: ["submit", "button", "reset", "input"], correct: 0 },
        { text: "CSS मध्ये padding आणि margin मध्ये मुख्य फरक काय?", options: ["padding inner space, margin outer space", "दोन्ही समान", "padding फक्त डावीकडे", "margin फक्त वर"], correct: 0 },
        { text: "Bootstrap हे काय आहे?", options: ["CSS Framework", "JavaScript Library", "Database", "Backend Language"], correct: 0 },
        { text: "HTML डॉक्युमेंट टाइप घोषित करण्यासाठी कोणती लाईन?", options: ["<!DOCTYPE html>", "<html>", "<!DOCTYPE>", "<head>"], correct: 0 },
        { text: "JavaScript मध्ये array ची लांबी कोणत्या property ने मिळते?", options: ["length", "size", "count", "len"], correct: 0 },
        { text: "CSS चा 'position: absolute' कसा वापरतात?", options: ["जवळच्या relative पॅरेंटशी संबंधित", "document body शी संबंधित", "स्क्रीनवर स्थिर", "स्क्रोल सह हलते"], correct: 0 },
        { text: "HTML5 मध्ये व्हिडिओ एम्बेड करण्यासाठी कोणता टॅग?", options: ["<video>", "<media>", "<movie>", "<source>"], correct: 0 },
        { text: "CSS मध्ये रंग लाल देण्यासाठी योग्य कोड?", options: ["color: red;", "font-color: red;", "text: red;", "bg: red;"], correct: 0 },
        { text: "JavaScript मध्ये '==' आणि '===' मध्ये काय फरक?", options: ["== फक्त value, === value आणि type दोन्ही", "दोन्ही सारखे", "=== फक्त value", "कोणताही फरक नाही"], correct: 0 },
        { text: "HTML मध्ये टेबल बनवण्यासाठी पंक्ती (row) कोणत्या टॅगने दर्शवतात?", options: ["<tr>", "<td>", "<th>", "<table>"], correct: 0 },
        { text: "CSS मध्ये hover effect साठी कोणता pseudo-class?", options: [":hover", ":active", ":focus", ":click"], correct: 0 },
        { text: "API मध्ये GET आणि POST मधील मुख्य फरक?", options: ["GET data URL मध्ये, POST body मध्ये", "दोन्ही सुरक्षित", "GET फक्त डेटा पाठवतो", "POST फक्त मिळवतो"], correct: 0 }
    ];
    
    const TOTAL_OBJ = questionsData.length;
    let selectedAnswers = new Array(TOTAL_OBJ).fill(null);
    
    // DOM elements
    const questionsContainer = document.getElementById('questionsContainer');
    const practicalTextarea = document.getElementById('practicalCodeArea');
    const submitBtn = document.getElementById('submitExamBtn');
    const clearObjBtn = document.getElementById('clearObjBtn');
    const exportObjBtn = document.getElementById('exportObjBtn');
    const resultContentDiv = document.getElementById('resultContent');
    
    // Storage keys
    const STORAGE_OBJ_KEY = "exam_obj_answers";
    const STORAGE_PRACTICAL_KEY = "exam_practical_code";
    
    // ---------- Render objective questions ----------
    function renderQuestions() {
        questionsContainer.innerHTML = '';
        questionsData.forEach((q, idx) => {
            const card = document.createElement('div');
            card.className = 'question-card';
            const qNumber = idx + 1;
            card.innerHTML = `
                <div class="q-text">
                    <span class="q-num">${qNumber}</span>
                    <span>${escapeHtml(q.text)}</span>
                </div>
                <div class="options" id="q-${idx}-options">
                    ${q.options.map((opt, optIdx) => `
                        <div class="option">
                            <input type="radio" name="q${idx}" value="${optIdx}" id="q${idx}_opt${optIdx}">
                            <label for="q${idx}_opt${optIdx}">${escapeHtml(opt)}</label>
                        </div>
                    `).join('')}
                </div>
            `;
            questionsContainer.appendChild(card);
            
            const radios = card.querySelectorAll(`input[name="q${idx}"]`);
            radios.forEach(radio => {
                radio.addEventListener('change', (e) => {
                    const selectedVal = parseInt(e.target.value);
                    selectedAnswers[idx] = selectedVal;
                    saveObjectiveAnswersToLocal();
                });
            });
        });
        loadSavedObjectiveAnswers();
    }
    
    function escapeHtml(str) {
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }
    
    function saveObjectiveAnswersToLocal() {
        localStorage.setItem(STORAGE_OBJ_KEY, JSON.stringify(selectedAnswers));
    }
    
    function loadSavedObjectiveAnswers() {
        const saved = localStorage.getItem(STORAGE_OBJ_KEY);
        if (saved) {
            try {
                const parsed = JSON.parse(saved);
                if (parsed && parsed.length === TOTAL_OBJ) {
                    for (let i = 0; i < TOTAL_OBJ; i++) {
                        if (parsed[i] !== null && parsed[i] !== undefined) {
                            selectedAnswers[i] = parsed[i];
                            const radioBtn = document.querySelector(`input[name="q${i}"][value="${parsed[i]}"]`);
                            if (radioBtn) radioBtn.checked = true;
                        }
                    }
                }
            } catch(e) { console.warn(e); }
        }
    }
    
    function clearAllObjectives() {
        for (let i = 0; i < TOTAL_OBJ; i++) {
            selectedAnswers[i] = null;
            const radios = document.querySelectorAll(`input[name="q${i}"]`);
            radios.forEach(radio => radio.checked = false);
        }
        saveObjectiveAnswersToLocal();
        const tempMsg = document.createElement('div');
        tempMsg.textContent = "✅ सर्व वस्तुनिष्ठ उत्तरे रीसेट झाली.";
        tempMsg.style.backgroundColor = "#e6f7e6";
        tempMsg.style.padding = "8px";
        tempMsg.style.borderRadius = "20px";
        tempMsg.style.marginTop = "12px";
        tempMsg.style.textAlign = "center";
        const objSection = document.querySelector('.objective-section');
        const existingMsg = document.getElementById('resetMsgToast');
        if(existingMsg) existingMsg.remove();
        tempMsg.id = "resetMsgToast";
        objSection.insertBefore(tempMsg, objSection.firstChild);
        setTimeout(() => { if(tempMsg) tempMsg.remove(); }, 1800);
    }
    
    function loadPracticalFromStorage() {
        const savedCode = localStorage.getItem(STORAGE_PRACTICAL_KEY);
        if (savedCode !== null) {
            practicalTextarea.value = savedCode;
        }
    }
    
    function savePracticalToStorage() {
        localStorage.setItem(STORAGE_PRACTICAL_KEY, practicalTextarea.value);
    }
    
    practicalTextarea.addEventListener('input', () => savePracticalToStorage());
    
    // ---------- Objective Evaluation & Export (Password protected ZIP) ----------
    function getObjectiveScoreAndDetails() {
        let score = 0;
        const details = [];
        for (let i = 0; i < TOTAL_OBJ; i++) {
            const selectedIdx = selectedAnswers[i];
            const isCorrect = (selectedIdx !== null && selectedIdx === questionsData[i].correct);
            if (isCorrect) score++;
            const selectedText = (selectedIdx !== null) ? questionsData[i].options[selectedIdx] : "(उत्तर निवडले नाही)";
            const correctText = questionsData[i].options[questionsData[i].correct];
            details.push({
                num: i+1,
                question: questionsData[i].text,
                selected: selectedText,
                correct: correctText,
                status: isCorrect ? "✔️ बरोबर" : "❌ चूक / निवडले नाही"
            });
        }
        return { score, total: TOTAL_OBJ, details };
    }
    
    function generateObjectiveReportText() {
        const { score, total, details } = getObjectiveScoreAndDetails();
        const now = new Date().toLocaleString();
        let report = `===========================================\n`;
        report += `📋 वस्तुनिष्ठ प्रश्नांचा निकाल (परीक्षा निकाल)\n`;
        report += `===========================================\n`;
        report += `दिनांक / वेळ: ${now}\n`;
        report += `एकूण प्रश्न: ${total}  |  मिळालेले गुण: ${score} / ${total}\n`;
        report += `===========================================\n\n`;
        details.forEach(d => {
            report += `प्रश्न ${d.num}: ${d.question}\n`;
            report += `   ➤ तुमचे उत्तर: ${d.selected}\n`;
            report += `   ➤ योग्य उत्तर: ${d.correct}\n`;
            report += `   ➤ स्थिती: ${d.status}\n\n`;
        });
        report += `===========================================\n`;
        report += `🔐 ही फाईल पासवर्डने संरक्षित आहे. पासवर्ड: skill123\n`;
        report += `===========================================\n`;
        return report;
    }
    
    async function exportObjectiveResultsToPasswordProtectedZip() {
        // Make sure selectedAnswers is in sync with radio buttons
        for (let i = 0; i < TOTAL_OBJ; i++) {
            const selectedRadio = document.querySelector(`input[name="q${i}"]:checked`);
            if (selectedRadio) {
                selectedAnswers[i] = parseInt(selectedRadio.value);
            } else {
                if (selectedAnswers[i] !== null) selectedAnswers[i] = null;
            }
        }
        saveObjectiveAnswersToLocal();
        
        const reportContent = generateObjectiveReportText();
        const zip = new JSZip();
        // Add text file inside zip
        zip.file("objective_results.txt", reportContent);
        
        try {
            // Generate password-protected ZIP with AES-256 encryption
            const blob = await zip.generateAsync({ 
                type: "blob", 
                encryption: "aes-256", 
                password: "skill123" 
            });
            // Trigger download using FileSaver
            saveAs(blob, "objective_results_secured.zip");
            // Show temporary success message
            const msgDiv = document.createElement('div');
            msgDiv.textContent = "✅ निकाल यशस्वीरित्या Export झाला! फाईल password 'skill123' ने सुरक्षित आहे.";
            msgDiv.style.backgroundColor = "#d4edda";
            msgDiv.style.padding = "8px";
            msgDiv.style.borderRadius = "20px";
            msgDiv.style.marginTop = "12px";
            msgDiv.style.textAlign = "center";
            msgDiv.style.color = "#155724";
            const targetSection = document.querySelector('.objective-section');
            const existingMsg = document.getElementById('exportMsgToast');
            if(existingMsg) existingMsg.remove();
            msgDiv.id = "exportMsgToast";
            targetSection.insertBefore(msgDiv, targetSection.firstChild);
            setTimeout(() => { if(msgDiv) msgDiv.remove(); }, 3000);
        } catch (err) {
            console.error(err);
            alert("Export करताना त्रुटी आली: " + err.message);
        }
    }
    
    // ---------- Practical Evaluation ----------
    function evaluatePracticalCode(code) {
        if (!code || code.trim() === "") return { marks: 0, breakdown: [] };
        let criteria = [
            { name: "✅ HTML मूलभूत रचना (DOCTYPE/html/body)", weight: 15, check: () => /<!DOCTYPE\s+html>/i.test(code) || (/<html[\s>]/i.test(code) && /<body[\s>]/i.test(code)) },
            { name: "🖼️ इमेज टॅग (<img>) उपस्थिती", weight: 15, check: () => /<img\s+[^>]*src\s*=/i.test(code) },
            { name: "📌 हेडिंग टॅग (h1 ते h6)", weight: 15, check: () => /<h[1-6][\s>]/i.test(code) },
            { name: "📝 पॅराग्राफ टॅग (<p>)", weight: 15, check: () => /<p[\s>]/i.test(code) },
            { name: "🎨 CSS स्टाइलिंग (<style> किंवा style attribute)", weight: 15, check: () => /<style[\s>]/i.test(code) || /style\s*=\s*["']/i.test(code) }
        ];
        let totalMarks = 0;
        let breakdownDetails = [];
        for (let cr of criteria) {
            let passed = cr.check();
            let earned = passed ? cr.weight : 0;
            totalMarks += earned;
            breakdownDetails.push(`${cr.name}: ${earned}/${cr.weight}`);
        }
        return { marks: totalMarks, breakdown: breakdownDetails };
    }
    
    function evaluateObjectiveScore() {
        let score = 0;
        for (let i = 0; i < TOTAL_OBJ; i++) {
            const userAns = selectedAnswers[i];
            if (userAns !== null && userAns === questionsData[i].correct) {
                score++;
            }
        }
        return score;
    }
    
    function computeAndShowResult() {
        saveObjectiveAnswersToLocal();
        savePracticalToStorage();
        const objScore = evaluateObjectiveScore();
        const objMax = TOTAL_OBJ;
        const userCode = practicalTextarea.value;
        const practicalResult = evaluatePracticalCode(userCode);
        const pracMarks = practicalResult.marks;
        const totalMarks = objScore + pracMarks;
        const percentage = ((totalMarks / 100) * 100).toFixed(1);
        let grade = "";
        if (percentage >= 70) grade = "उत्तीर्ण (प्रथम श्रेणी) 🎉";
        else if (percentage >= 40) grade = "उत्तीर्ण ✅";
        else grade = "अनुत्तीर्ण ❌ (पुन्हा प्रयत्न करा)";
        
        let pracBreakHtml = "";
        if (practicalResult.breakdown.length) {
            pracBreakHtml = `<div class="prac-breakdown"><strong>🔍 प्रॅक्टिकल मूल्यांकन तपशील:</strong><br> ${practicalResult.breakdown.join('<br>')}</div>`;
        } else {
            pracBreakHtml = `<div class="prac-breakdown">⚠️ कोड सापडला नाही किंवा रिकामा</div>`;
        }
        
        const resultHtml = `
            <div class="result-stats">
                <div class="stat-card"><strong>📖 वस्तुनिष्ठ गुण</strong><br> ${objScore} / ${objMax}</div>
                <div class="stat-card"><strong>💻 व्यावहारिक गुण</strong><br> ${pracMarks} / 75</div>
                <div class="stat-card"><strong>🏆 एकूण गुण</strong><br> ${totalMarks} / 100</div>
                <div class="stat-card"><strong>📈 टक्केवारी</strong><br> ${percentage}%</div>
            </div>
            <div style="margin-top: 16px; font-weight: bold; text-align: center; background: #fff3e0; border-radius: 40px; padding: 10px;">🎓 निकाल: ${grade}</div>
            ${pracBreakHtml}
            <div style="font-size: 12px; margin-top: 12px; text-align: center;">✔️ वस्तुनिष्ठ उत्तरे व कोड सेव्ह झाले आहेत. पुन्हा सबमिट केल्यास नवीन मूल्यांकन होईल.</div>
        `;
        resultContentDiv.innerHTML = resultHtml;
    }
    
    // Event listeners
    submitBtn.addEventListener('click', () => {
        for (let i = 0; i < TOTAL_OBJ; i++) {
            const selectedRadio = document.querySelector(`input[name="q${i}"]:checked`);
            if (selectedRadio) {
                selectedAnswers[i] = parseInt(selectedRadio.value);
            } else {
                if (selectedAnswers[i] !== null) selectedAnswers[i] = null;
            }
        }
        saveObjectiveAnswersToLocal();
        computeAndShowResult();
    });
    
    clearObjBtn.addEventListener('click', clearAllObjectives);
    exportObjBtn.addEventListener('click', exportObjectiveResultsToPasswordProtectedZip);
    
    // Initial load
    renderQuestions();
    loadPracticalFromStorage();
    practicalTextarea.dispatchEvent(new Event('input'));
</script>
</body>
</html> 
