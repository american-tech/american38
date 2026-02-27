<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>نظام توزيع الخبز</title>
<script src="https://unpkg.com/html5-qrcode"></script>
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body { 
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
    text-align: center; 
    direction: rtl; 
    margin: 0;
    min-height: 100vh;
    background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
    padding: 30px 20px;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    padding: 30px;
    border-radius: 24px;
    box-shadow: 0 20px 40px -10px rgba(0, 0, 0, 0.1), 0 4px 12px rgba(0, 0, 0, 0.05);
    position: relative;
    min-height: 600px;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.3);
}

h2 { 
    color: #0f3b2b;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 20px;
    position: relative;
    display: inline-block;
    padding-bottom: 10px;
}

h2::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 60px;
    height: 3px;
    background: linear-gradient(90deg, #2e7d5e, #4caf7e);
    border-radius: 3px;
}

table { 
    width: 100%; 
    margin-top: 25px; 
    border-collapse: separate;
    border-spacing: 0 8px;
    background: transparent;
}

th { 
    background: linear-gradient(135deg, #1e4d3a, #2e7d5e);
    color: white;
    padding: 16px 12px;
    font-weight: 500;
    font-size: 15px;
    letter-spacing: 0.3px;
    border: none;
}

th:first-child {
    border-radius: 12px 0 0 12px;
}

th:last-child {
    border-radius: 0 12px 12px 0;
}

td { 
    background-color: white;
    padding: 14px 12px; 
    text-align: center;
    border: none;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
    font-size: 14px;
    color: #334155;
}

tr td:first-child {
    border-radius: 10px 0 0 10px;
    font-weight: 600;
    color: #1e4d3a;
}

tr td:last-child {
    border-radius: 0 10px 10px 0;
}

tr:hover td {
    background-color: #f8fafc;
    transition: background-color 0.2s ease;
}

#total { 
    font-size: 24px; 
    background: linear-gradient(135deg, #1e4d3a, #2e7d5e);
    color: white;
    margin: 25px 0 15px;
    font-weight: 600;
    padding: 16px 24px;
    border-radius: 50px;
    display: inline-block;
    box-shadow: 0 10px 20px -5px rgba(30, 77, 58, 0.3);
    letter-spacing: 0.5px;
}

#reader {
    width: 100%;
    max-width: 450px;
    margin: 20px auto;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 15px 30px -8px rgba(0, 0, 0, 0.15);
    border: 3px solid white;
}

.status-success {
    color: #2e7d5e;
    font-weight: 600;
    padding: 12px 24px;
    font-size: 18px;
    background: linear-gradient(135deg, #e8f5e9, #c8e6c9);
    border-radius: 50px;
    display: inline-block;
    margin: 10px 0;
    border: 1px solid #a5d6a7;
}

.status-error {
    color: #b71c1c;
    font-weight: 600;
    padding: 12px 24px;
    font-size: 18px;
    background: linear-gradient(135deg, #ffebee, #ffcdd2);
    border-radius: 50px;
    display: inline-block;
    margin: 10px 0;
    border: 1px solid #ef9a9a;
}

.status-ignored {
    color: #6b6b6b;
    font-weight: 600;
    padding: 12px 24px;
    font-size: 18px;
    background: linear-gradient(135deg, #f5f5f5, #eeeeee);
    border-radius: 50px;
    display: inline-block;
    margin: 10px 0;
    border: 1px solid #bdbdbd;
}

.date-display {
    font-size: 16px;
    color: #64748b;
    margin: 10px 0 20px;
    direction: ltr;
    background: #f8fafc;
    padding: 8px 16px;
    border-radius: 50px;
    display: inline-block;
    border: 1px solid #e2e8f0;
}

/* زر الإعدادات */
.settings-container {
    position: relative;
    display: inline-block;
    margin: 20px 0;
}

.settings-button {
    background: linear-gradient(135deg, #334155, #475569);
    color: white;
    border: none;
    border-radius: 50px;
    padding: 14px 40px;
    font-size: 18px;
    cursor: pointer;
    box-shadow: 0 8px 16px -4px rgba(51, 65, 85, 0.3);
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    font-weight: 500;
    letter-spacing: 0.3px;
    display: flex;
    align-items: center;
    gap: 10px;
    margin: 0 auto;
}

.settings-button:hover {
    background: linear-gradient(135deg, #1e293b, #334155);
    transform: translateY(-2px);
    box-shadow: 0 12px 24px -6px rgba(51, 65, 85, 0.4);
}

.settings-button:active {
    transform: translateY(0);
}

.gear-icon {
    display: inline-block;
    animation: rotate 10s linear infinite;
    font-size: 22px;
}

@keyframes rotate {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}

.settings-menu {
    display: none;
    position: absolute;
    bottom: 70px;
    left: 50%;
    transform: translateX(-50%);
    background: white;
    min-width: 280px;
    box-shadow: 0 20px 40px -12px rgba(0, 0, 0, 0.25);
    border-radius: 20px;
    padding: 16px;
    z-index: 1000;
    border: 1px solid #e2e8f0;
}

.settings-menu.show {
    display: block;
    animation: slideUp 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes slideUp {
    from {
        opacity: 0;
        transform: translateX(-50%) translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateX(-50%) translateY(0);
    }
}

.settings-menu button {
    width: 100%;
    margin: 8px 0;
    padding: 14px 16px;
    border: none;
    border-radius: 14px;
    font-size: 15px;
    cursor: pointer;
    transition: all 0.2s ease;
    text-align: right;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 10px;
}

.thursday-menu-btn {
    background: linear-gradient(135deg, #9333ea, #a855f7);
    color: white;
    box-shadow: 0 4px 12px -2px rgba(147, 51, 234, 0.3);
}

.thursday-menu-btn:hover {
    background: linear-gradient(135deg, #7e22ce, #9333ea);
    transform: translateX(-2px);
}

.thursday-menu-btn.active {
    background: linear-gradient(135deg, #16a34a, #22c55e);
    box-shadow: 0 4px 12px -2px rgba(22, 163, 74, 0.3);
}

.reset-menu-btn {
    background: linear-gradient(135deg, #ea580c, #f97316);
    color: white;
    box-shadow: 0 4px 12px -2px rgba(234, 88, 12, 0.3);
}

.reset-menu-btn:hover {
    background: linear-gradient(135deg, #c2410c, #ea580c);
    transform: translateX(-2px);
}

.download-menu-btn {
    background: linear-gradient(135deg, #2563eb, #3b82f6);
    color: white;
    box-shadow: 0 4px 12px -2px rgba(37, 99, 235, 0.3);
}

.download-menu-btn:hover {
    background: linear-gradient(135deg, #1d4ed8, #2563eb);
    transform: translateX(-2px);
}

.thursday-indicator {
    margin: 15px auto;
    padding: 12px 24px;
    border-radius: 50px;
    font-weight: 600;
    font-size: 15px;
    display: inline-block;
    background: linear-gradient(135deg, #f3e8ff, #e9d5ff);
    color: #9333ea;
    border: 1px solid #d8b4fe;
    box-shadow: 0 4px 10px -2px rgba(147, 51, 234, 0.15);
}

.thursday-indicator.inactive {
    background: linear-gradient(135deg, #f1f5f9, #e2e8f0);
    color: #475569;
    border: 1px solid #cbd5e1;
    box-shadow: none;
}

/* تنسيق الفوتر */
.footer {
    margin-top: 40px;
    padding: 20px 0 0;
}

/* تحسين مظهر الماسح الضوئي */
#reader__scan_region {
    background: #f8fafc;
}

#reader__dashboard_section {
    padding: 16px !important;
}

#reader__dashboard_section button {
    background: linear-gradient(135deg, #1e4d3a, #2e7d5e) !important;
    color: white !important;
    border: none !important;
    border-radius: 12px !important;
    padding: 10px 24px !important;
    font-size: 14px !important;
    font-weight: 500 !important;
    cursor: pointer !important;
    transition: all 0.2s ease !important;
}

#reader__dashboard_section button:hover {
    background: linear-gradient(135deg, #0f3b2b, #1e4d3a) !important;
    transform: translateY(-1px) !important;
}

/* تحسين للجوال */
@media (max-width: 768px) {
    body {
        padding: 15px 10px;
    }
    
    .container {
        padding: 20px;
    }
    
    th {
        padding: 12px 8px;
        font-size: 13px;
    }
    
    td {
        padding: 10px 6px;
        font-size: 12px;
    }
    
    #total {
        font-size: 20px;
        padding: 12px 20px;
    }
    
    h2 {
        font-size: 24px;
    }
}
</style>
</head>
<body>

<div class="container">
    <h2>🍞 نظام توزيع ربطات الخبز</h2>
    <div class="date-display" id="currentDate"></div>
    
    <div id="thursdayIndicator" class="thursday-indicator inactive">
        ⚪ وضع الخميس غير مفعل
    </div>

    <h3 id="status" class="status-success">جاهز للمسح</h3>

    <div id="reader"></div>

    <table id="dataTable">
        <thead>
            <tr>
                <th>التسلسل</th>
                <th>اسم المستفيد</th>
                <th>عدد الربطات</th>
                <th>التاريخ</th>
            </tr>
        </thead>
        <tbody id="tableBody">
        </tbody>
    </table>

    <div id="total">إجمالي الربطات: 0</div>

    <!-- زر الإعدادات في الأسفل -->
    <div class="footer">
        <div class="settings-container">
            <button class="settings-button" onclick="toggleSettingsMenu()">
                <span class="gear-icon">⚙️</span> الإعدادات
            </button>
            <div class="settings-menu" id="settingsMenu">
                <button class="thursday-menu-btn" id="thursdayMenuBtn" onclick="toggleThursdayMode()">
                    <span>🟣</span> تفعيل وضع الخميس
                </button>
                <button class="reset-menu-btn" onclick="resetToday()">
                    <span>🔄</span> تصفير اليوم
                </button>
                <button class="download-menu-btn" onclick="downloadCSV()">
                    <span>📥</span> تحميل التقرير
                </button>
            </div>
        </div>
    </div>
</div>

<script>
let counter = 1;
let totalBread = 0;
let today = new Date().toISOString().slice(0,10);
let scannedToday = JSON.parse(localStorage.getItem(today)) || {};
let scanHistory = JSON.parse(localStorage.getItem('history_' + today)) || [];
let thursdayMode = JSON.parse(localStorage.getItem('thursdayMode_' + today)) || false;

// متغيرات لمنع التكرار
let lastScanTime = 0;
let lastScannedText = '';
const SCAN_DELAY = 3000; // 3 ثواني

// فتح وإغلاق قائمة الإعدادات
function toggleSettingsMenu() {
    const menu = document.getElementById('settingsMenu');
    menu.classList.toggle('show');
}

// إغلاق القائمة عند النقر خارجها
window.onclick = function(event) {
    if (!event.target.matches('.settings-button') && !event.target.matches('.settings-button *')) {
        const menu = document.getElementById('settingsMenu');
        if (menu.classList.contains('show')) {
            menu.classList.remove('show');
        }
    }
}

// تحديث واجهة وضع الخميس
function updateThursdayUI() {
    const indicator = document.getElementById('thursdayIndicator');
    const menuBtn = document.getElementById('thursdayMenuBtn');
    
    if (thursdayMode) {
        indicator.className = 'thursday-indicator';
        indicator.innerHTML = '🟣 وضع الخميس مفعل - مضاعفة ربطات الـ 1';
        menuBtn.innerHTML = '<span>⚪</span> إلغاء وضع الخميس';
        menuBtn.classList.add('active');
    } else {
        indicator.className = 'thursday-indicator inactive';
        indicator.innerHTML = '⚪ وضع الخميس غير مفعل';
        menuBtn.innerHTML = '<span>🟣</span> تفعيل وضع الخميس';
        menuBtn.classList.remove('active');
    }
}

// تبديل وضع الخميس
function toggleThursdayMode() {
    thursdayMode = !thursdayMode;
    localStorage.setItem('thursdayMode_' + today, JSON.stringify(thursdayMode));
    updateThursdayUI();
}

// دالة للحصول على التاريخ بالإنجليزية (day/month)
function getEnglishDate() {
    let date = new Date();
    let day = date.getDate();
    let month = date.getMonth() + 1;
    let year = date.getFullYear();
    
    const monthNames = [
        'January', 'February', 'March', 'April', 'May', 'June',
        'July', 'August', 'September', 'October', 'November', 'December'
    ];
    
    return {
        short: `${day}/${month}/${year}`,
        medium: `${monthNames[month-1]} ${day}, ${year}`,
        display: `${monthNames[month-1]} ${day}`
    };
}

// عرض التاريخ الحالي
function updateDateDisplay() {
    let dateInfo = getEnglishDate();
    document.getElementById('currentDate').innerText = dateInfo.medium;
}

// تحميل البيانات المحفوظة
function loadSavedData() {
    if (scanHistory.length > 0) {
        const tableBody = document.getElementById('tableBody');
        tableBody.innerHTML = '';
        counter = 1;
        totalBread = 0;
        
        scanHistory.forEach(record => {
            addRowToTable(record.name, record.count, record.time);
        });
    }
    updateThursdayUI();
}

// إضافة صف للجدول
function addRowToTable(name, count, time) {
    let tableBody = document.getElementById('tableBody');
    let row = tableBody.insertRow();
    
    row.insertCell(0).innerText = counter++;
    row.insertCell(1).innerText = name;
    row.insertCell(2).innerText = count;
    row.insertCell(3).innerText = time;
    
    totalBread += count;
    document.getElementById("total").innerText = "إجمالي الربطات: " + totalBread;
}

// إنشاء صوت نجاح
function playSuccessSound() {
    let audio = new Audio();
    audio.src = 'data:audio/wav;base64,UklGRlwAAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YVAAAAA8AP8A/gD/AP8A/wD/AP8A';
    audio.play().catch(e => console.log('صوت غير مدعوم'));
}

// إنشاء صوت خطأ
function playErrorSound() {
    let audio = new Audio();
    audio.src = 'data:audio/wav;base64,UklGRlQAAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YVAAAAAQAQABAAEAAQABAAEAAQABAA==';
    audio.play().catch(e => console.log('صوت غير مدعوم'));
}

function onScanSuccess(decodedText) {
    let currentTime = Date.now();
    
    // منع القراءات المتكررة خلال 3 ثواني
    if (currentTime - lastScanTime < SCAN_DELAY && decodedText === lastScannedText) {
        return; // تجاهل صامت
    }

    lastScanTime = currentTime;
    lastScannedText = decodedText;

    console.log("تم المسح: " + decodedText);
    
    // التحقق من الصيغة المحددة
    let exactMatch = decodedText.match(/^NAME:\s*([^,]+?)\s*,\s*COUNT:\s*(\d+)$/i);
    
    if (!exactMatch) {
        document.getElementById("status").className = "status-ignored";
        document.getElementById("status").innerText = "⚠️ كود غير معتمد";
        return;
    }

    let name = exactMatch[1].trim();
    let originalCount = parseInt(exactMatch[2]);

    if (isNaN(originalCount) || originalCount <= 0) {
        document.getElementById("status").className = "status-error";
        document.getElementById("status").innerText = "❌ عدد غير صالح";
        playErrorSound();
        return;
    }

    // منع التكرار اليومي
    if (scannedToday[name]) {
        document.getElementById("status").className = "status-error";
        document.getElementById("status").innerText = "❌ " + name + " مستلم مسبقاً";
        playErrorSound();
        return;
    }

    // تحديد العدد النهائي (مع مراعاة وضع الخميس)
    let finalCount = originalCount;
    
    // إذا كان وضع الخميس مفعل والعدد الأصلي هو 1 فقط
    if (thursdayMode && originalCount === 1) {
        finalCount = 2; // مضاعفة ربطه واحدة فقط
    }

    // تسجيل الاستلام
    scannedToday[name] = true;
    localStorage.setItem(today, JSON.stringify(scannedToday));
    
    let dateInfo = getEnglishDate();
    let recordDate = dateInfo.display;
    
    let record = { 
        name, 
        count: finalCount, 
        time: recordDate
    };
    
    scanHistory.push(record);
    localStorage.setItem('history_' + today, JSON.stringify(scanHistory));
    
    addRowToTable(name, finalCount, recordDate);

    // رسالة بسيطة
    document.getElementById("status").className = "status-success";
    document.getElementById("status").innerText = "✅ تم تسجيل " + name + " - " + finalCount + " ربطات";
    
    playSuccessSound();
}

function onScanError(error) {
    if (!error.includes("NotFoundException")) {
        console.warn("خطأ في المسح: " + error);
    }
}

// إعدادات الماسح الضوئي
let html5QrcodeScanner = new Html5QrcodeScanner(
    "reader", 
    { 
        fps: 5,
        qrbox: 250,
        rememberLastUsedCamera: true,
        showTorchButtonIfSupported: true
    }
);

html5QrcodeScanner.render(onScanSuccess, onScanError);

// تحميل التقرير
function downloadCSV() {
    let csv = [];
    
    csv.push("التسلسل,اسم المستفيد,عدد الربطات,التاريخ");
    
    scanHistory.forEach((record, index) => {
        csv.push(`${index + 1},${record.name},${record.count},${record.time}`);
    });
    
    csv.push("");
    csv.push("إجمالي الربطات," + totalBread);
    csv.push("تاريخ التقرير," + getEnglishDate().medium);

    let file = new Blob(["\uFEFF" + csv.join("\n")], { type: "text/csv;charset=utf-8" });
    let link = document.createElement("a");
    link.download = "توزيع_الخبز_" + today + ".csv";
    link.href = URL.createObjectURL(file);
    link.click();
    
    // إغلاق القائمة بعد التحميل
    document.getElementById('settingsMenu').classList.remove('show');
}

// تصفير بيانات اليوم
function resetToday() {
    if (confirm('هل أنت متأكد من تصفير جميع بيانات اليوم؟')) {
        scannedToday = {};
        scanHistory = [];
        totalBread = 0;
        counter = 1;
        thursdayMode = false;
        
        localStorage.removeItem(today);
        localStorage.removeItem('history_' + today);
        localStorage.removeItem('thursdayMode_' + today);
        
        document.getElementById('tableBody').innerHTML = '';
        document.getElementById("total").innerText = "إجمالي الربطات: 0";
        document.getElementById("status").className = "status-success";
        document.getElementById("status").innerText = "تم تصفير البيانات";
        
        updateThursdayUI();
        
        // إغلاق القائمة بعد التصفير
        document.getElementById('settingsMenu').classList.remove('show');
    }
}

// تحديث التاريخ وعرضه
updateDateDisplay();

// تحميل البيانات المحفوظة عند بدء التشغيل
loadSavedData();
</script>

</body>
</html>
