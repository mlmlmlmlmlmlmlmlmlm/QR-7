<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Сабақ Қатысу Жүйесі</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            background: white;
            padding: 30px;
            border-radius: 20px;
            text-align: center;
            margin-bottom: 30px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            border: 5px solid #1a2980;
        }

        .header h1 {
            color: #1a2980;
            font-size: 2.5rem;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }

        .header p {
            color: #666;
            font-size: 1.2rem;
            line-height: 1.6;
        }

        /* Mode Selection */
        .mode-selector {
            display: flex;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .mode-btn {
            flex: 1;
            min-width: 250px;
            padding: 25px;
            border: none;
            border-radius: 15px;
            font-size: 1.3rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
        }

        .mode-btn i {
            font-size: 2rem;
        }

        .mode-btn.active {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
        }

        .mode-btn.teacher {
            background: linear-gradient(135deg, #ff7e5f, #feb47b);
            color: white;
        }

        .mode-btn.student {
            background: linear-gradient(135deg, #4CAF50, #2E7D32);
            color: white;
        }

        .mode-btn.reset {
            background: linear-gradient(135deg, #ff416c, #ff4b2b);
            color: white;
        }

        /* Panel Styling */
        .panel {
            background: white;
            padding: 40px;
            border-radius: 20px;
            margin-bottom: 30px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            display: none;
        }

        .panel.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .panel h2 {
            color: #1a2980;
            font-size: 2rem;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 3px solid #26d0ce;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        /* QR Code Section */
        .qr-container {
            display: flex;
            gap: 40px;
            margin: 40px 0;
            padding: 30px;
            background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
            border-radius: 20px;
            flex-wrap: wrap;
            align-items: center;
        }

        #qrcode {
            background: white;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border: 5px solid #1a2980;
        }

        .token-info {
            flex: 1;
            min-width: 300px;
        }

        .token-display {
            font-size: 2.5rem;
            font-weight: bold;
            background: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            margin: 20px 0;
            letter-spacing: 5px;
            color: #ff7e5f;
            border: 3px dashed #1a2980;
            font-family: 'Courier New', monospace;
        }

        .controls {
            display: flex;
            gap: 20px;
            margin-top: 30px;
            flex-wrap: wrap;
        }

        .control-group {
            flex: 1;
            min-width: 250px;
        }

        .control-group label {
            display: block;
            margin-bottom: 10px;
            font-weight: bold;
            color: #1a2980;
            font-size: 1.1rem;
        }

        .control-group input {
            width: 100%;
            padding: 15px;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 1.1rem;
            transition: all 0.3s;
        }

        .control-group input:focus {
            border-color: #1a2980;
            outline: none;
            box-shadow: 0 0 0 3px rgba(26, 41, 128, 0.1);
        }

        /* Buttons */
        .btn {
            padding: 18px 35px;
            border: none;
            border-radius: 12px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 12px;
            margin: 10px;
            min-width: 200px;
            justify-content: center;
        }

        .btn-primary {
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            color: white;
        }

        .btn-success {
            background: linear-gradient(135deg, #4CAF50, #2E7D32);
            color: white;
        }

        .btn-warning {
            background: linear-gradient(135deg, #ff9800, #ff5722);
            color: white;
        }

        .btn-danger {
            background: linear-gradient(135deg, #f44336, #c62828);
            color: white;
        }

        .btn:hover:not(:disabled) {
            transform: translateY(-5px);
            box-shadow: 0 15px 25px rgba(0, 0, 0, 0.2);
        }

        .btn:disabled {
            background: #cccccc;
            cursor: not-allowed;
            transform: none !important;
            box-shadow: none !important;
        }

        /* Attendance Table */
        .table-container {
            overflow-x: auto;
            margin: 30px 0;
            border-radius: 15px;
            border: 2px solid #e0e0e0;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        thead {
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            color: white;
        }

        th {
            padding: 20px 15px;
            text-align: left;
            font-weight: bold;
            font-size: 1.1rem;
        }

        tbody tr {
            border-bottom: 1px solid #eee;
            transition: background 0.3s;
        }

        tbody tr:hover {
            background: #f0f9ff;
        }

        td {
            padding: 18px 15px;
            font-size: 1.1rem;
        }

        /* Student Registration */
        .registration-form {
            background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
            padding: 40px;
            border-radius: 20px;
            margin: 30px 0;
        }

        .form-group {
            margin-bottom: 30px;
        }

        .form-group label {
            display: block;
            margin-bottom: 15px;
            font-weight: bold;
            color: #1a2980;
            font-size: 1.3rem;
        }

        .form-group input {
            width: 100%;
            max-width: 500px;
            padding: 20px;
            border: 2px solid #ddd;
            border-radius: 12px;
            font-size: 1.2rem;
            transition: all 0.3s;
        }

        .form-group input:focus {
            border-color: #4CAF50;
            outline: none;
            box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.1);
        }

        /* Messages */
        .message {
            padding: 25px;
            border-radius: 15px;
            margin: 25px 0;
            display: none;
            align-items: center;
            gap: 20px;
            font-size: 1.2rem;
            animation: fadeIn 0.5s ease;
        }

        .message.show {
            display: flex;
        }

        .message i {
            font-size: 2rem;
        }

        .message-success {
            background: #e8f5e9;
            color: #2e7d32;
            border-left: 8px solid #4CAF50;
        }

        .message-error {
            background: #ffebee;
            color: #c62828;
            border-left: 8px solid #f44336;
        }

        .message-info {
            background: #e3f2fd;
            color: #1565c0;
            border-left: 8px solid #2196f3;
        }

        .message-warning {
            background: #fff8e1;
            color: #ff8f00;
            border-left: 8px solid #ffc107;
        }

        /* Materials */
        .materials-section {
            margin-top: 40px;
        }

        .materials-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 25px;
            margin-top: 25px;
        }

        .material-card {
            background: white;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
            border: 2px solid #e0e0e0;
            transition: all 0.3s;
        }

        .material-card:hover {
            transform: translateY(-10px);
            border-color: #4CAF50;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
        }

        .material-card h4 {
            color: #1a2980;
            margin-bottom: 15px;
            font-size: 1.4rem;
        }

        .material-meta {
            color: #666;
            margin-bottom: 20px;
            font-size: 0.95rem;
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .download-btn {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            color: white;
            padding: 15px 25px;
            border-radius: 10px;
            text-decoration: none;
            font-weight: bold;
            transition: all 0.3s;
        }

        .download-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(26, 41, 128, 0.2);
        }

        /* Status Badge */
        .status-badge {
            display: inline-block;
            padding: 10px 20px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 1.1rem;
            margin-left: 15px;
        }

        .status-active {
            background: #4CAF50;
            color: white;
        }

        .status-inactive {
            background: #f44336;
            color: white;
        }

        /* Stats */
        .stats-container {
            display: flex;
            gap: 20px;
            margin: 30px 0;
            flex-wrap: wrap;
        }

        .stat-card {
            flex: 1;
            min-width: 200px;
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            color: white;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
        }

        .stat-number {
            font-size: 3rem;
            font-weight: bold;
            margin: 15px 0;
        }

        .stat-label {
            font-size: 1.2rem;
            opacity: 0.9;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .mode-btn, .btn {
                min-width: 100%;
            }
            
            .qr-container {
                flex-direction: column;
                text-align: center;
            }
            
            .controls {
                flex-direction: column;
            }
            
            .control-group {
                min-width: 100%;
            }
            
            .materials-grid {
                grid-template-columns: 1fr;
            }
            
            .header h1 {
                flex-direction: column;
                font-size: 2rem;
            }
        }

        /* Animations */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .pulse {
            animation: pulse 2s infinite;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <h1>
                <i class="fas fa-qrcode"></i>
                QR САБАҚ ҚАТЫСУ ЖҮЙЕСІ
            </h1>
            <p>Мұғалім QR код жасайды → Оқушылар телефондарымен сканерлеп тіркеледі → Жүйе автоматты түрде тіркеу жасайды</p>
            <p style="margin-top: 15px; color: #ff7e5f; font-weight: bold;">
                <i class="fas fa-mobile-alt"></i> Оқушылар телефондарымен QR кодты сканерлеуі керек!
            </p>
        </div>

        <!-- Mode Selector -->
        <div class="mode-selector">
            <button class="mode-btn teacher active" id="teacherModeBtn">
                <i class="fas fa-chalkboard-teacher"></i>
                МҰҒАЛІМ РЕЖИМІ
            </button>
            <button class="mode-btn student" id="studentModeBtn">
                <i class="fas fa-user-graduate"></i>
                ОҚУШЫ РЕЖИМІ
            </button>
            <button class="mode-btn reset" id="resetBtn">
                <i class="fas fa-trash-alt"></i>
                ЖҮЙЕНІ ТАЗАРТУ
            </button>
        </div>

        <!-- Teacher Panel -->
        <div id="teacherPanel" class="panel active">
            <h2><i class="fas fa-chalkboard-teacher"></i> МҰҒАЛІМ ПАНЕЛІ</h2>
            
            <!-- Stats -->
            <div class="stats-container">
                <div class="stat-card">
                    <div class="stat-number" id="totalStudents">0</div>
                    <div class="stat-label">ҚАТЫСУШЫ САНЫ</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number" id="currentSession">1</div>
                    <div class="stat-label">СЕССИЯ НӨМІРІ</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number" id="tokenInterval">10</div>
                    <div class="stat-label">ТОКЕН ИНТЕРВАЛЫ</div>
                </div>
            </div>

            <!-- QR Code Section -->
            <div class="qr-container">
                <div id="qrcode"></div>
                
                <div class="token-info">
                    <h3 style="color: #1a2980; margin-bottom: 20px;">
                        <i class="fas fa-key"></i> АҒЫМДАҒЫ QR ТОКЕН
                    </h3>
                    
                    <div class="token-display" id="currentToken">
                        <span id="tokenText">БАСТАУҒА ДАЙЫН</span>
                    </div>
                    
                    <p style="color: #666; margin: 15px 0; font-size: 1.1rem;">
                        <i class="fas fa-info-circle"></i> Оқушылар осы токенді телефондарымен сканерлеп тіркеледі
                    </p>
                    
                    <div class="controls">
                        <div class="control-group">
                            <label><i class="fas fa-clock"></i> Токен интервалы (секунд):</label>
                            <input type="number" id="intervalInput" value="10" min="5" max="60">
                        </div>
                        
                        <div class="control-group">
                            <label><i class="fas fa-cogs"></i> Басқару:</label>
                            <div style="display: flex; gap: 15px; flex-wrap: wrap;">
                                <button class="btn btn-primary" id="startRotateBtn">
                                    <i class="fas fa-play"></i> QR КОДТЫ БАСТАУ
                                </button>
                                <button class="btn btn-danger" id="stopRotateBtn" disabled>
                                    <i class="fas fa-stop"></i> ТОҚТАТУ
                                </button>
                                <button class="btn btn-warning" id="generateTokenBtn">
                                    <i class="fas fa-redo"></i> БІР ТОКЕН ЖАСАУ
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Attendance Table -->
            <h3 style="margin-top: 40px;">
                <i class="fas fa-users"></i> ҚАТЫСҚАН ОҚУШЫЛАР
                <span class="status-badge status-active" id="attendanceCount">0 адам</span>
            </h3>
            
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th><i class="fas fa-hashtag"></i> №</th>
                            <th><i class="fas fa-user"></i> ОҚУШЫ АТЫ</th>
                            <th><i class="fas fa-calendar-alt"></i> КҮНІ</th>
                            <th><i class="fas fa-clock"></i> УАҚЫТЫ</th>
                            <th><i class="fas fa-qrcode"></i> ТОКЕН</th>
                        </tr>
                    </thead>
                    <tbody id="attendanceTable">
                        <!-- Data will be inserted here -->
                    </tbody>
                </table>
            </div>

            <!-- File Upload -->
            <h3 style="margin-top: 40px;">
                <i class="fas fa-file-upload"></i> САБАҚ МАТЕРИАЛДАРЫН ЖҮКТЕУ
            </h3>
            
            <div style="background: #f5f7fa; padding: 25px; border-radius: 15px; margin-top: 20px;">
                <input type="file" id="fileInput" style="width: 100%; padding: 15px; border: 2px dashed #1a2980; border-radius: 10px; margin-bottom: 20px;">
                
                <button class="btn btn-success" id="uploadBtn">
                    <i class="fas fa-cloud-upload-alt"></i> МАТЕРИАЛДЫ ЖАРИЯЛАУ
                </button>
            </div>
            
            <div id="uploadMsg" class="message info">
                <i class="fas fa-info-circle"></i>
                <span>Жүктеу хабарламасы осында пайда болады</span>
            </div>
        </div>

        <!-- Student Panel -->
        <div id="studentPanel" class="panel">
            <h2><i class="fas fa-user-graduate"></i> ОҚУШЫ ПАНЕЛІ</h2>
            
            <!-- Registration Form -->
            <div class="registration-form">
                <h3 style="color: #1a2980; margin-bottom: 30px;">
                    <i class="fas fa-user-plus"></i> САБАҚҚА ҚАТЫСУ
                </h3>
                
                <div class="form-group">
                    <label for="studentName">
                        <i class="fas fa-user"></i> АТЫҢЫЗДЫ ЕНГІЗІҢІЗ:
                    </label>
                    <input type="text" id="studentName" placeholder="Мысалы: Әлия Нұрғалиева" autocomplete="off">
                </div>
                
                <div class="form-group">
                    <label for="tokenInput">
                        <i class="fas fa-qrcode"></i> МҰҒАЛІМ БЕРГЕН QR ТОКЕНДІ ЕНГІЗІҢІЗ:
                    </label>
                    <input type="text" id="tokenInput" placeholder="QR кодты сканерлеп немесе токенді енгізіңіз" autocomplete="off">
                </div>
                
                <div style="display: flex; gap: 20px; flex-wrap: wrap;">
                    <button class="btn btn-success" id="submitAttendanceBtn">
                        <i class="fas fa-check-circle"></i> ҚАТЫСУДЫ ТІРКЕУ
                    </button>
                    
                    <button class="btn btn-primary" id="scanQRBtn">
                        <i class="fas fa-camera"></i> QR КОДТЫ СКАНЕРЛЕУ
                    </button>
                </div>
                
                <p style="margin-top: 20px; color: #666; font-size: 1.1rem;">
                    <i class="fas fa-lightbulb"></i> КЕҢЕС: QR кодты сканерлеу үшін жоғарыдағы түймені басыңыз
                </p>
            </div>
            
            <div id="studentMsg" class="message info">
                <i class="fas fa-info-circle"></i>
                <span>Хабарлама осында пайда болады</span>
            </div>

            <!-- Materials Section -->
            <div class="materials-section">
                <h3>
                    <i class="fas fa-book-open"></i> САБАҚ МАТЕРИАЛДАРЫ
                    <span class="status-badge status-active" id="materialsCount">0 материал</span>
                </h3>
                
                <div id="materialsList" class="materials-grid">
                    <!-- Materials will be loaded here -->
                </div>
            </div>
        </div>

        <!-- Instructions -->
        <div class="panel" style="background: linear-gradient(135deg, #1a2980, #26d0ce); color: white;">
            <h2 style="color: white; border-bottom-color: white;">
                <i class="fas fa-graduation-cap"></i> ЖҮЙЕНІҢ ЖҮМЫС БАРЫСЫ
            </h2>
            
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 30px; margin-top: 30px;">
                <div style="background: rgba(255, 255, 255, 0.1); padding: 25px; border-radius: 15px;">
                    <h3 style="color: white; margin-bottom: 15px;">1. МҰҒАЛІМ РЕЖИМІ</h3>
                    <p>Мұғалім "QR КОДТЫ БАСТАУ" түймесін басады → Жүйе автоматты түрде токен жасайды → Оқушылар осы токенді пайдаланады</p>
                </div>
                
                <div style="background: rgba(255, 255, 255, 0.1); padding: 25px; border-radius: 15px;">
                    <h3 style="color: white; margin-bottom: 15px;">2. ОҚУШЫ РЕЖИМІ</h3>
                    <p>Оқушы атын енгізеді → QR кодты сканерлейді → "ҚАТЫСУДЫ ТІРКЕУ" түймесін басады → Жүйе тіркеуді растайды</p>
                </div>
                
                <div style="background: rgba(255, 255, 255, 0.1); padding: 25px; border-radius: 15px;">
                    <h3 style="color: white; margin-bottom: 15px;">3. ТЕХНИКАЛЫҚ МҮМКІНДІКТЕР</h3>
                    <p>✓ QR код авто жаңарту<br>
                       ✓ Материалдарды жүктеу<br>
                       ✓ Деректерді сақтау<br>
                       ✓ Барлық құрылғылармен үйлесімді</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // ============================================
        // НЕГІЗГІ АЙНЫМАЛЫЛАР
        // ============================================
        let qrcode = null;
        let tokenInterval = null;
        let currentToken = "";
        let sessionId = 1;
        let isTeacherMode = true;

        // ============================================
        // DOM ЭЛЕМЕНТТЕРІ
        // ============================================
        const teacherPanel = document.getElementById('teacherPanel');
        const studentPanel = document.getElementById('studentPanel');
        const teacherModeBtn = document.getElementById('teacherModeBtn');
        const studentModeBtn = document.getElementById('studentModeBtn');
        const resetBtn = document.getElementById('resetBtn');
        const qrcodeContainer = document.getElementById('qrcode');
        const currentTokenSpan = document.getElementById('tokenText');
        const intervalInput = document.getElementById('intervalInput');
        const startRotateBtn = document.getElementById('startRotateBtn');
        const stopRotateBtn = document.getElementById('stopRotateBtn');
        const generateTokenBtn = document.getElementById('generateTokenBtn');
        const attendanceTable = document.getElementById('attendanceTable');
        const attendanceCount = document.getElementById('attendanceCount');
        const totalStudents = document.getElementById('totalStudents');
        const tokenIntervalSpan = document.getElementById('tokenInterval');
        const fileInput = document.getElementById('fileInput');
        const uploadBtn = document.getElementById('uploadBtn');
        const uploadMsg = document.getElementById('uploadMsg');
        const studentNameInput = document.getElementById('studentName');
        const tokenInput = document.getElementById('tokenInput');
        const submitAttendanceBtn = document.getElementById('submitAttendanceBtn');
        const scanQRBtn = document.getElementById('scanQRBtn');
        const studentMsg = document.getElementById('studentMsg');
        const materialsList = document.getElementById('materialsList');
        const materialsCount = document.getElementById('materialsCount');

        // ============================================
        // БАСТАПҚЫ БЕЛСЕНДІРУ
        // ============================================
        document.addEventListener('DOMContentLoaded', function() {
            // Өткен деректерді жүктеу
            loadFromStorage();
            
            // Мұғалім режимін көрсету
            switchToTeacherMode();
            
            // Интервал мәнін жаңарту
            intervalInput.addEventListener('input', function() {
                tokenIntervalSpan.textContent = this.value;
            });
            
            // Автоматты түрде бір токен жасау
            generateSingleToken();
            
            // Әр 5 секунд сайын статистиканы жаңарту
            setInterval(updateStats, 5000);
        });

        // ============================================
        // ДЕРЕКТЕРДІ ЖҮКТЕУ ЖӘНЕ САҚТАУ
        // ============================================
        function loadFromStorage() {
            // Сессия ID
            const savedSession = localStorage.getItem('sessionId');
            if (savedSession) {
                sessionId = parseInt(savedSession);
                document.getElementById('currentSession').textContent = sessionId;
            }
            
            // Өткен токен
            const savedToken = localStorage.getItem('currentToken');
            if (savedToken) {
                currentToken = savedToken;
                currentTokenSpan.textContent = currentToken;
                renderQRCode(currentToken);
            }
            
            // Статистиканы жаңарту
            updateStats();
        }

        function saveToStorage() {
            localStorage.setItem('sessionId', sessionId);
            localStorage.setItem('currentToken', currentToken);
        }

        // ============================================
        // QR КОД ЖАСАУ
        // ============================================
        function renderQRCode(text) {
            if (qrcode) {
                qrcodeContainer.innerHTML = '';
            }
            
            qrcode = new QRCode(qrcodeContainer, {
                text: text,
                width: 250,
                height: 250,
                colorDark: "#1a2980",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });
        }

        // ============================================
        // ТОКЕН ЖАСАУ ФУНКЦИЯЛАРЫ
        // ============================================
        function generateToken() {
            // 6 таңбалы әріп-сан аралас токен
            const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
            let token = '';
            for (let i = 0; i < 6; i++) {
                token += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return token;
        }

        function updateToken() {
            currentToken = generateToken();
            currentTokenSpan.textContent = currentToken;
            localStorage.setItem('currentToken', currentToken);
            renderQRCode(currentToken);
            
            // Анимация эффектісі
            currentTokenSpan.parentElement.classList.add('pulse');
            setTimeout(() => {
                currentTokenSpan.parentElement.classList.remove('pulse');
            }, 1000);
            
            console.log("Жаңа токен жасалды:", currentToken);
        }

        function generateSingleToken() {
            updateToken();
            showMessage(uploadMsg, "Бір токен сәтті жасалды! Оқушылар осы токенді пайдалана алады.", "success");
        }

        function startTokenRotation() {
            const seconds = parseInt(intervalInput.value) || 10;
            
            // Бірден токен жасау
            updateToken();
            
            // Интервалды орнату
            if (tokenInterval) clearInterval(tokenInterval);
            tokenInterval = setInterval(updateToken, seconds * 1000);
            
            // Түймелерді белсендіру/өшіру
            startRotateBtn.disabled = true;
            stopRotateBtn.disabled = false;
            generateTokenBtn.disabled = true;
            
            // Хабарлама
            showMessage(uploadMsg, `QR код айналдыру басталды! Токен әр ${seconds} секунд сайын жаңарады.`, "success");
        }

        function stopTokenRotation() {
            if (tokenInterval) {
                clearInterval(tokenInterval);
                tokenInterval = null;
                
                // Түймелерді белсендіру/өшіру
                startRotateBtn.disabled = false;
                stopRotateBtn.disabled = true;
                generateTokenBtn.disabled = false;
                
                // Хабарлама
                showMessage(uploadMsg, "QR код айналдыру тоқтатылды. Енді токен автоматты түрде жаңармайды.", "info");
            }
        }

        // ============================================
        // ҚАТЫСУДЫ БАСҚАРУ
        // ============================================
        function loadAttendance() {
            const key = `attendance_session_${sessionId}`;
            const data = localStorage.getItem(key);
            return data ? JSON.parse(data) : [];
        }

        function saveAttendance(list) {
            const key = `attendance_session_${sessionId}`;
            localStorage.setItem(key, JSON.stringify(list));
            updateStats();
        }

        function refreshAttendanceTable() {
            const attendanceList = loadAttendance();
            const tableBody = document.getElementById('attendanceTable');
            
            if (attendanceList.length === 0) {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="5" style="text-align: center; padding: 50px; color: #666;">
                            <i class="fas fa-users-slash" style="font-size: 3rem; margin-bottom: 20px; display: block; color: #ccc;"></i>
                            ӘЛІ ЕШКІМ ҚАТЫСПАҒАН<br>
                            <small style="font-size: 0.9rem;">Оқушылар тіркелгеннен кейін осында пайда болады</small>
                        </td>
                    </tr>
                `;
                return;
            }
            
            // Кестені толтыру
            let html = '';
            attendanceList.forEach((student, index) => {
                html += `
                    <tr>
                        <td><strong>${index + 1}</strong></td>
                        <td><strong>${escapeHtml(student.name)}</strong></td>
                        <td>${student.date}</td>
                        <td>${student.time}</td>
                        <td><code style="background: #f0f0f0; padding: 5px 10px; border-radius: 5px;">${student.token}</code></td>
                    </tr>
                `;
            });
            
            tableBody.innerHTML = html;
        }

        // ============================================
        // ОҚУШЫНЫ ТІРКЕУ
        // ============================================
        function registerStudent() {
            const studentName = studentNameInput.value.trim();
            const enteredToken = tokenInput.value.trim().toUpperCase();
            
            // Тексерулер
            if (!studentName) {
                showMessage(studentMsg, "Атыңызды енгізіңіз!", "error");
                studentNameInput.focus();
                return;
            }
            
            if (!enteredToken) {
                showMessage(studentMsg, "Токенді енгізіңіз немесе QR кодты сканерлеңіз!", "error");
                tokenInput.focus();
                return;
            }
            
            // Токенді тексеру
            const savedToken = localStorage.getItem('currentToken') || "";
            
            if (enteredToken !== savedToken) {
                showMessage(studentMsg, "Токен қате! Мұғалім берген дұрыс токенді енгізіңіз.", "error");
                tokenInput.value = "";
                tokenInput.focus();
                return;
            }
            
            // Оқушы бұрын тіркелген бе?
            let attendanceList = loadAttendance();
            const existingStudent = attendanceList.find(s => s.name === studentName);
            
            if (existingStudent) {
                showMessage(studentMsg, `${studentName}, сіз бұрын қатысқансыз!`, "warning");
                return;
            }
            
            // Жаңа оқушыны тіркеу
            const now = new Date();
            const newStudent = {
                name: studentName,
                date: now.toLocaleDateString('kk-KZ'),
                time: now.toLocaleTimeString('kk-KZ', { hour: '2-digit', minute: '2-digit' }),
                token: enteredToken,
                timestamp: now.getTime()
            };
            
            attendanceList.push(newStudent);
            saveAttendance(attendanceList);
            
            // Сәтті хабарлама
            showMessage(studentMsg, `Құттықтаймыз, ${studentName}! Сіз сабаққа сәтті қатыстыңыз.`, "success");
            
            // Өрістерді тазарту
            tokenInput.value = "";
            
            // Кестені жаңарту (егер мұғалім режимінде болсақ)
            if (isTeacherMode) {
                refreshAttendanceTable();
            }
            
            // Материалдарды жаңарту
            refreshMaterialsList();
            
            // Дыбыстық сигнал (егер қолда болса)
            playSuccessSound();
            
            // 5 секундтан кейін хабарламаны жасыру
            setTimeout(() => {
                studentMsg.classList.remove("show");
            }, 5000);
        }

        // ============================================
        // МАТЕРИАЛДАРДЫ БАСҚАРУ
        // ============================================
        function loadMaterials() {
            const key = `materials_session_${sessionId}`;
            const data = localStorage.getItem(key);
            return data ? JSON.parse(data) : [];
        }

        function saveMaterials(list) {
            const key = `materials_session_${sessionId}`;
            localStorage.setItem(key, JSON.stringify(list));
            updateStats();
        }

        function refreshMaterialsList() {
            const materials = loadMaterials();
            const studentName = studentNameInput.value.trim();
            
            // Материалдар санын жаңарту
            materialsCount.textContent = `${materials.length} материал`;
            
            if (materials.length === 0) {
                materialsList.innerHTML = `
                    <div style="grid-column: 1 / -1; text-align: center; padding: 40px; background: #f8f9fa; border-radius: 15px;">
                        <i class="fas fa-file-alt" style="font-size: 3rem; color: #ccc; margin-bottom: 20px; display: block;"></i>
                        <h4 style="color: #666;">ӘЛІ МАТЕРИАЛ ЖОҚ</h4>
                        <p style="color: #888; margin-top: 10px;">Мұғалім материалдарды жариялағаннан кейін осында пайда болады</p>
                    </div>
                `;
                return;
            }
            
            let html = '';
            materials.forEach(material => {
                html += `
                    <div class="material-card">
                        <h4>${escapeHtml(material.name)}</h4>
                        <div class="material-meta">
                            <span><i class="fas fa-calendar"></i> ${material.date}</span>
                            <span><i class="fas fa-file"></i> ${material.size}</span>
                        </div>
                        <p style="color: #666; margin: 15px 0;">${material.description || 'Сабақ материалы'}</p>
                        <a href="${material.url}" download="${material.name}" class="download-btn">
                            <i class="fas fa-download"></i> Жүктеп алу
                        </a>
                    </div>
                `;
            });
            
            materialsList.innerHTML = html;
        }

        function uploadMaterial() {
            const file = fileInput.files[0];
            
            if (!file) {
                showMessage(uploadMsg, "Алдымен файл таңдаңыз!", "error");
                return;
            }
            
            // Файл өлшемін тексеру
            if (file.size > 10 * 1024 * 1024) {
                showMessage(uploadMsg, "Файл өлшемі 10MB-тан аспауы керек!", "error");
                return;
            }
            
            // Файлды оқу
            const reader = new FileReader();
            
            reader.onload = function(e) {
                // Материалды сақтау
                const materials = loadMaterials();
                
                const newMaterial = {
                    id: Date.now(),
                    name: file.name,
                    url: e.target.result,
                    date: new Date().toLocaleDateString('kk-KZ'),
                    size: formatFileSize(file.size),
                    description: `Сабақ материалы - ${file.name.split('.').pop().toUpperCase()} формат`
                };
                
                materials.push(newMaterial);
                saveMaterials(materials);
                
                // Хабарлама
                showMessage(uploadMsg, `"${file.name}" файлы сәтті жүктелді! Оқушылар жүктеп ала алады.`, "success");
                
                // Файл өрісін тазарту
                fileInput.value = "";
                
                // Материалдар тізімін жаңарту
                refreshMaterialsList();
                
                // Егер оқушы режимінде болсақ
                if (!isTeacherMode) {
                    refreshMaterialsList();
                }
            };
            
            reader.readAsDataURL(file);
        }

        // ============================================
        // КӨМЕКШІ ФУНКЦИЯЛАР
        // ============================================
        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        function showMessage(element, text, type) {
            const icon = {
                success: "fa-check-circle",
                error: "fa-exclamation-circle",
                info: "fa-info-circle",
                warning: "fa-exclamation-triangle"
            }[type] || "fa-info-circle";
            
            element.innerHTML = `<i class="fas ${icon}"></i><span>${escapeHtml(text)}</span>`;
            element.className = `message message-${type} show`;
        }

        function formatFileSize(bytes) {
            if (bytes < 1024) return bytes + ' Байт';
            else if (bytes < 1048576) return (bytes / 1024).toFixed(1) + ' КБ';
            else return (bytes / 1048576).toFixed(1) + ' МБ';
        }

        function updateStats() {
            const attendanceList = loadAttendance();
            const materials = loadMaterials();
            
            // Сандарды жаңарту
            totalStudents.textContent = attendanceList.length;
            attendanceCount.textContent = `${attendanceList.length} адам`;
            materialsCount.textContent = `${materials.length} материал`;
            
            // Кестені жаңарту
            refreshAttendanceTable();
        }

        function playSuccessSound() {
            // Қарапайым дыбыстық сигнал
            try {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.value = 800;
                oscillator.type = 'sine';
                
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.5);
                
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + 0.5);
            } catch (e) {
                console.log("Аудио қолдауы жоқ");
            }
        }

        function switchToTeacherMode() {
            teacherPanel.classList.add("active");
            studentPanel.classList.remove("active");
            teacherModeBtn.classList.add("active");
            studentModeBtn.classList.remove("active");
            isTeacherMode = true;
            
            // Статистиканы жаңарту
            updateStats();
        }

        function switchToStudentMode() {
            teacherPanel.classList.remove("active");
            studentPanel.classList.add("active");
            teacherModeBtn.classList.remove("active");
            studentModeBtn.classList.add("active");
            isTeacherMode = false;
            
            // Материалдарды жаңарту
            refreshMaterialsList();
        }

        function resetSystem() {
            if (confirm("БАРЛЫҚ ДЕРЕКТЕР ӨШІРІЛЕДІ! Бұл әрекетті қайтарып алу мүмкін емес. Жалғастыру керек пе?")) {
                // Токен айналдыруды тоқтату
                if (tokenInterval) {
                    clearInterval(tokenInterval);
                    tokenInterval = null;
                }
                
                // Жаңа сессия бастау
                sessionId++;
                localStorage.setItem('sessionId', sessionId);
                
                // Ағымдағы сессия деректерін тазарту
                localStorage.removeItem(`attendance_session_${sessionId-1}`);
                localStorage.removeItem(`materials_session_${sessionId-1}`);
                localStorage.removeItem('currentToken');
                
                // Өрістерді тазарту
                currentToken = "";
                currentTokenSpan.textContent = "ЖАҢА СЕССИЯ";
                qrcodeContainer.innerHTML = "<div style='padding: 50px; text-align: center; color: #666;'><i class='fas fa-qrcode' style='font-size: 100px;'></i><br>QR код осында пайда болады</div>";
                studentNameInput.value = "";
                tokenInput.value = "";
                
                // Түймелерді қалпына келтіру
                startRotateBtn.disabled = false;
                stopRotateBtn.disabled = true;
                generateTokenBtn.disabled = false;
                
                // Статистиканы жаңарту
                updateStats();
                
                showMessage(uploadMsg, `Жүйе тазартылды! Жаңа сессия №${sessionId} басталды.`, "success");
            }
        }

        // ============================================
        // QR СКАНЕРЛЕУ ЭМУЛЯЦИЯСЫ
        // ============================================
        function emulateQRScan() {
            const savedToken = localStorage.getItem('currentToken');
            if (savedToken) {
                tokenInput.value = savedToken;
                showMessage(studentMsg, "QR код сәтті сканерленді! Енді 'ҚАТЫСУДЫ ТІРКЕУ' түймесін басыңыз.", "success");
            } else {
                showMessage(studentMsg, "Мұғалім токен жасамаған! Күтіңіз...", "warning");
            }
        }

        // ============================================
        // ОҚИҒАЛАРДЫ ТІРКЕУ
        // ============================================
        teacherModeBtn.addEventListener('click', switchToTeacherMode);
        studentModeBtn.addEventListener('click', switchToStudentMode);
        resetBtn.addEventListener('click', resetSystem);
        startRotateBtn.addEventListener('click', startTokenRotation);
        stopRotateBtn.addEventListener('click', stopTokenRotation);
        generateTokenBtn.addEventListener('click', generateSingleToken);
        submitAttendanceBtn.addEventListener('click', registerStudent);
        scanQRBtn.addEventListener('click', emulateQRScan);
        uploadBtn.addEventListener('click', uploadMaterial);

        // Enter пернесімен тіркеу
        studentNameInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') registerStudent();
        });
        
        tokenInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') registerStudent();
        });

        // Оқушы аты өзгергенде материалдарды жаңарту
        studentNameInput.addEventListener('input', refreshMaterialsList);
    </script>
</body>
</html>
