[educational_game.html](https://github.com/user-attachments/files/32160641/educational_game.html)
# -
مهم
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>تحدي علماء موهبة | العلوم للصف الخامس</title>
<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>
<!-- FontAwesome & Google Fonts -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800;900&family=Amiri:wght@400;700;800&display=swap" rel="stylesheet">
<!-- Canvas Confetti -->
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

<script>
tailwind.config = {
  theme: {
    extend: {
      fontFamily: { sans: ['Tajawal', 'sans-serif'], amiri: ['Amiri', 'serif'] },
      colors: {
        mawhiba: { 50: '#ecfdf5', 100: '#d1fae5', 500: '#10b981', 600: '#059669', 700: '#047857', 900: '#064e3b' }
      }
    }
  }
}
</script>

<style>
body {
  font-family: 'Tajawal', sans-serif;
  background: radial-gradient(circle at 50% 10%, #1e1b4b 0%, #0f172a 60%, #022c22 100%);
  min-height: 100vh;
  color: #f8fafc;
  user-select: none;
}

.glass-card {
  background: rgba(15, 23, 42, 0.85);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.15);
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
}

.opt-btn {
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  border: 2px solid rgba(255, 255, 255, 0.12);
}

.opt-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  border-color: #38bdf8;
  background: rgba(30, 41, 59, 0.9);
  box-shadow: 0 10px 20px -5px rgba(56, 189, 248, 0.25);
}

.correct-anim {
  background: linear-gradient(135deg, #059669 0%, #10b981 100%) !important;
  border-color: #34d399 !important;
  animation: popIn 0.4s ease;
}

.wrong-anim {
  background: linear-gradient(135deg, #be123c 0%, #f43f5e 100%) !important;
  border-color: #fb7185 !important;
  animation: shake 0.4s ease;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-8px); }
  75% { transform: translateX(8px); }
}

@keyframes popIn {
  0% { transform: scale(0.95); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

.powerup-btn {
  transition: all 0.2s ease;
}
.powerup-btn:hover:not(:disabled) {
  transform: scale(1.08);
  box-shadow: 0 0 15px rgba(251, 191, 36, 0.4);
}
.powerup-btn:disabled {
  opacity: 0.35;
  cursor: not-allowed;
  filter: grayscale(1);
}

.scroll-area { max-height: 55vh; overflow-y: auto; }
.scroll-area::-webkit-scrollbar { width: 6px; }
.scroll-area::-webkit-scrollbar-thumb { background: #334155; border-radius: 6px; }

/* Certificate Styles */
@media print {
  body * { visibility: hidden !important; }
  #cert-modal, #cert-modal * { visibility: visible !important; }
  #cert-modal { position: absolute; inset: 0; background: #fff; padding: 0; overflow: visible; }
  .no-print { display: none !important; }
}

.cert-frame {
  width: 210mm;
  min-height: 297mm;
  background: #ffffff;
  color: #0f172a;
  padding: 12mm;
  margin: auto;
  position: relative;
  box-shadow: 0 0 30px rgba(0,0,0,0.3);
}

.cert-border {
  border: 4px double #059669;
  border-radius: 8mm;
  padding: 10mm;
  height: 100%;
  position: relative;
  background: radial-gradient(circle at 10% 10%, rgba(236,253,245,0.8) 0%, rgba(255,255,255,1) 80%);
}
</style>
</head>
<body class="flex flex-col justify-between p-3 sm:p-6 min-h-screen">

<header class="max-w-5xl w-full mx-auto flex items-center justify-between glass-card p-3 sm:p-4 rounded-2xl mb-4">
  <div class="flex items-center gap-3">
    <div class="bg-gradient-to-tr from-emerald-500 to-teal-400 w-11 h-11 rounded-xl flex items-center justify-center text-slate-950 font-black text-xl shadow-lg">
      <i class="fa-solid fa-flask-vial"></i>
    </div>
    <div>
      <h1 class="text-white font-extrabold text-base sm:text-lg leading-tight">مختبر علماء موهبة 🚀</h1>
      <p class="text-emerald-300 text-xs font-medium">العلوم للصف الخامس • العلاقات والنسب في حياتنا</p>
    </div>
  </div>

  <div class="flex items-center gap-2 sm:gap-4">
    <!-- Sound Toggle -->
    <button id="sound-btn" onclick="toggleSound()" class="w-9 h-9 rounded-xl bg-white/10 hover:bg-white/20 border border-white/20 text-white flex items-center justify-center transition">
      <i id="sound-icon" class="fa-solid fa-volume-high"></i>
    </button>
    <!-- Streak -->
    <div class="flex items-center gap-1.5 bg-amber-500/20 px-3 py-1.5 rounded-xl border border-amber-400/30 text-amber-300 font-bold text-xs">
      <i class="fa-solid fa-fire text-amber-400 animate-bounce"></i>
      <span id="streak-counter">0</span>
    </div>
    <!-- XP Score -->
    <div class="bg-emerald-500/20 px-3.5 py-1.5 rounded-xl border border-emerald-400/30 text-emerald-300 font-extrabold text-sm flex items-center gap-1.5">
      <i class="fa-solid fa-trophy text-amber-400"></i>
      <span id="score-counter">0</span> <span class="text-[10px] text-emerald-400/80">XP</span>
    </div>
  </div>
</header>

<main class="max-w-5xl w-full mx-auto flex-1 flex flex-col justify-center">

  <!-- ================= 1. شاشة البداية ================= -->
  <div id="start-screen" class="glass-card rounded-3xl p-6 sm:p-10 text-center shadow-2xl transition-all">
    <div class="w-20 h-20 sm:w-24 sm:h-24 bg-emerald-500/20 text-emerald-400 rounded-3xl flex items-center justify-center mx-auto mb-4 border-2 border-emerald-500/40 text-4xl shadow-inner">
      <i class="fa-solid fa-brain"></i>
    </div>
    <h2 class="text-2xl sm:text-4xl font-extrabold text-white mb-2">تحدي علماء موهبة في العلوم 🧬</h2>
    <p class="text-slate-300 max-w-lg mx-auto mb-6 text-xs sm:text-base leading-relaxed">
      مرحباً بك يا عبقري المستقبل! اختبر معلوماتك في وحدة <b class="text-emerald-400">العلاقات والنسب في حياتنا</b>، اكسب النقاط، واستخدم المساعدات الذكية لتصل لشهادة التميز!
    </p>

    <!-- اسم الطالب -->
    <div class="max-w-md mx-auto mb-6 text-right">
      <label class="block text-xs font-bold text-emerald-300 mb-1.5">
        <i class="fa-solid fa-user-graduate ml-1"></i> اسم الطالب (سيظهر في شهادة الإنجاز):
      </label>
      <input id="student-name-input" type="text" placeholder="اكتب اسمك الثلاثي هنا..."
        class="w-full px-4 py-3 rounded-xl bg-slate-900/90 border-2 border-emerald-500/40 focus:border-emerald-400 focus:outline-none text-white text-sm font-medium transition">
    </div>

    <!-- اختيار الرحلة -->
    <div class="mb-8">
      <h3 class="font-bold text-slate-300 text-xs sm:text-sm mb-3 flex items-center justify-center gap-2">
        <i class="fa-solid fa-compass text-amber-400"></i> اختر مستوى الرحلة التعليمية:
      </h3>
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 max-w-3xl mx-auto">
        <div onclick="selectLevel('easy')" id="level-easy" class="diff-card cursor-pointer p-4 rounded-2xl border-2 border-emerald-500/40 bg-emerald-950/30 text-center hover:border-emerald-400 transition">
          <div class="text-2xl mb-1">🚀</div>
          <div class="font-extrabold text-emerald-400 text-sm">أبطال العلوم</div>
          <div class="text-[11px] text-slate-400 mt-0.5">8 أسئلة • 30 ث/سؤال</div>
        </div>
        <div onclick="selectLevel('medium')" id="level-medium" class="diff-card cursor-pointer p-4 rounded-2xl border-2 border-amber-500/40 bg-amber-950/30 text-center hover:border-amber-400 transition bg-amber-500/10 border-amber-400">
          <div class="text-2xl mb-1">💡</div>
          <div class="font-extrabold text-amber-400 text-sm">المبدعون</div>
          <div class="text-[11px] text-slate-400 mt-0.5">9 أسئلة • 25 ث/سؤال</div>
        </div>
        <div onclick="selectLevel('hard')" id="level-hard" class="diff-card cursor-pointer p-4 rounded-2xl border-2 border-rose-500/40 bg-rose-950/30 text-center hover:border-rose-400 transition">
          <div class="text-2xl mb-1">⭐</div>
          <div class="font-extrabold text-rose-400 text-sm">المتألقون</div>
          <div class="text-[11px] text-slate-400 mt-0.5">6 أسئلة • 20 ث/سؤال</div>
        </div>
        <div onclick="selectLevel('all')" id="level-all" class="diff-card cursor-pointer p-4 rounded-2xl border-2 border-purple-500/40 bg-purple-950/30 text-center hover:border-purple-400 transition">
          <div class="text-2xl mb-1">🏆</div>
          <div class="font-extrabold text-purple-400 text-sm">أبطال موهبة</div>
          <div class="text-[11px] text-slate-400 mt-0.5">23 سؤالاً شاملاً</div>
        </div>
      </div>
    </div>

    <button onclick="startGame()" class="bg-gradient-to-r from-emerald-500 to-teal-600 hover:from-emerald-400 hover:to-teal-500 text-slate-950 font-black text-base sm:text-lg px-10 py-4 rounded-2xl shadow-xl transform hover:scale-105 transition-all">
      <i class="fa-solid fa-play ml-2"></i> ابدأ التحدي الآن
    </button>
  </div>

  <!-- ================= 2. شاشة اللعب والأسئلة ================= -->
  <div id="game-screen" class="hidden glass-card rounded-3xl p-5 sm:p-8 shadow-2xl">
    
    <!-- الشريط العلوي للعبة -->
    <div class="flex flex-wrap items-center justify-between gap-3 mb-4 pb-3 border-b border-white/10">
      <div class="flex items-center gap-2">
        <span id="question-progress-badge" class="bg-emerald-500/20 text-emerald-300 font-extrabold px-3 py-1 rounded-xl text-xs border border-emerald-500/30">
          السؤال 1 من 10
        </span>
        <span id="category-badge" class="bg-sky-500/20 text-sky-300 font-bold px-3 py-1 rounded-xl text-xs border border-sky-500/30">
          المفاهيم الأساسية
        </span>
      </div>

      <!-- وسائل المساعدة -->
      <div class="flex items-center gap-2">
        <button id="powerup-5050" onclick="usePowerup5050()" class="powerup-btn bg-slate-800 border border-amber-400/50 text-amber-300 px-3 py-1.5 rounded-xl text-xs font-bold flex items-center gap-1.5 shadow" title="حذف إجابتين خاطئتين">
          <i class="fa-solid fa-scale-unbalanced text-amber-400"></i> 50:50
        </button>
        <button id="powerup-freeze" onclick="usePowerupFreeze()" class="powerup-btn bg-slate-800 border border-cyan-400/50 text-cyan-300 px-3 py-1.5 rounded-xl text-xs font-bold flex items-center gap-1.5 shadow" title="تجميد المؤقت لمخية 10 ثوانٍ">
          <i class="fa-solid fa-snowflake text-cyan-400"></i> تجميد
        </button>
      </div>
    </div>

    <!-- شريط الوقت -->
    <div class="mb-5">
      <div class="flex justify-between items-center text-xs font-bold text-slate-400 mb-1">
        <span><i class="fa-solid fa-stopwatch text-emerald-400 ml-1"></i> الوقت المتبقي:</span>
        <span id="timer-text" class="text-amber-300 font-black text-sm">30</span>
      </div>
      <div class="w-full bg-slate-800 h-2.5 rounded-full overflow-hidden p-0.5 border border-white/10">
        <div id="timer-bar" class="bg-gradient-to-r from-emerald-400 via-amber-400 to-rose-500 h-full rounded-full transition-all duration-200"></div>
      </div>
    </div>

    <!-- نص السؤال -->
    <div class="bg-slate-900/80 p-5 sm:p-6 rounded-2xl border border-white/10 mb-5">
      <h2 id="question-text" class="text-base sm:text-xl font-extrabold text-white leading-relaxed text-right">
        جاري تحميل السؤال...
      </h2>
    </div>

    <!-- خيارات الإجابة -->
    <div id="options-container" class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-5"></div>

    <!-- صندوق التغذية الراجعة والتفسير -->
    <div id="feedback-box" class="hidden p-4 sm:p-5 rounded-2xl mb-5 border text-right">
      <div class="flex items-start gap-3">
        <div id="feedback-icon" class="text-2xl shrink-0 mt-0.5"></div>
        <div>
          <h4 id="feedback-title" class="font-extrabold text-base mb-1"></h4>
          <p id="feedback-detail" class="text-xs sm:text-sm leading-relaxed text-slate-200"></p>
        </div>
      </div>
    </div>

    <!-- زر التالي -->
    <div class="flex justify-end">
      <button id="next-btn" onclick="nextQuestion()" class="hidden bg-gradient-to-r from-sky-400 to-blue-600 hover:from-sky-500 hover:to-blue-700 text-slate-950 font-black text-sm sm:text-base px-8 py-3 rounded-xl shadow-lg transition-all transform hover:scale-105 flex items-center gap-2">
        <span>السؤال التالي</span>
        <i class="fa-solid fa-arrow-left"></i>
      </button>
    </div>
  </div>

  <!-- ================= 3. شاشة النتائج والمراجعة ================= -->
  <div id="end-screen" class="hidden glass-card rounded-3xl p-6 sm:p-8 shadow-2xl text-center">
    <div class="w-20 h-20 sm:w-24 sm:h-24 bg-amber-400/20 text-amber-400 rounded-full flex items-center justify-center mx-auto mb-3 text-4xl border-2 border-amber-400/40 shadow-xl">
      <i class="fa-solid fa-award"></i>
    </div>
    <h2 id="result-title" class="text-2xl sm:text-3xl font-black text-white mb-1">إنجاز رائع يا عالم العلوم! 🌟</h2>
    <p id="result-subtitle" class="text-slate-300 text-xs sm:text-sm mb-6">أكملت جميع أسئلة التحدي بنجاح.</p>

    <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 max-w-3xl mx-auto mb-6">
      <div class="bg-slate-900/80 p-3.5 rounded-2xl border border-white/10">
        <div class="text-xs text-slate-400 font-bold mb-1">نقاط التميز</div>
        <div id="final-score" class="text-xl sm:text-2xl font-black text-emerald-400">0</div>
      </div>
      <div class="bg-slate-900/80 p-3.5 rounded-2xl border border-white/10">
        <div class="text-xs text-slate-400 font-bold mb-1">الإجابات الصحيحة</div>
        <div id="final-correct" class="text-xl sm:text-2xl font-black text-sky-400">0</div>
      </div>
      <div class="bg-slate-900/80 p-3.5 rounded-2xl border border-white/10">
        <div class="text-xs text-slate-400 font-bold mb-1">نسبة الإتقان</div>
        <div id="final-percentage" class="text-xl sm:text-2xl font-black text-amber-400">0%</div>
      </div>
      <div class="bg-slate-900/80 p-3.5 rounded-2xl border border-white/10">
        <div class="text-xs text-slate-400 font-bold mb-1">أعلى الحماس (Streak)</div>
        <div id="final-streak" class="text-xl sm:text-2xl font-black text-purple-400">0</div>
      </div>
    </div>

    <!-- مراجعة الأسئلة -->
    <div id="review-container" class="hidden mb-6 text-right">
      <h3 class="font-bold text-slate-200 text-sm mb-3 flex items-center gap-2">
        <i class="fa-solid fa-clipboard-check text-emerald-400"></i> مراجعة الإجابات والتفسيرات العلمية:
      </h3>
      <div id="review-list" class="scroll-area space-y-2"></div>
    </div>

    <!-- أزرار التحكم -->
    <div class="flex flex-wrap gap-3 justify-center">
      <button onclick="openCertificateModal()" class="bg-gradient-to-r from-amber-400 to-yellow-500 hover:from-amber-500 hover:to-yellow-600 text-slate-950 font-black text-sm px-6 py-3 rounded-xl shadow-lg transition-all inline-flex items-center gap-2">
        <i class="fa-solid fa-certificate"></i> عرض و استخراج الشهادة
      </button>
      <button onclick="startGame()" class="bg-emerald-600 hover:bg-emerald-500 text-white font-bold text-sm px-6 py-3 rounded-xl shadow-lg transition-all inline-flex items-center gap-2">
        <i class="fa-solid fa-rotate-right"></i> إعادة الرحلة
      </button>
      <button onclick="backToStart()" class="bg-slate-800 hover:bg-slate-700 text-slate-200 font-bold text-sm px-6 py-3 rounded-xl shadow transition-all inline-flex items-center gap-2">
        <i class="fa-solid fa-house"></i> القائمة الرئيسية
      </button>
    </div>
  </div>

</main>

<footer class="max-w-5xl w-full mx-auto text-center mt-6 text-emerald-200/60 text-[11px]">
  برنامج موهبة المتقدم في العلوم والرياضيات • المراجعة النهائية للصف الخامس الابتدائي
</footer>

<!-- ================= 4. نافذة الشهادة المطبوعة ================= -->
<div id="cert-modal" class="hidden fixed inset-0 z-50 bg-slate-950/85 backdrop-blur-md items-start justify-center overflow-y-auto p-3 sm:p-6">
  <div class="w-full max-w-4xl flex flex-col items-center gap-4 my-auto">
    
    <!-- أزرار التحكم بالشهادة -->
    <div class="no-print flex flex-wrap gap-3 justify-center bg-slate-900 border border-white/10 rounded-2xl px-5 py-3 shadow-2xl">
      <button onclick="window.print()" class="bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-extrabold text-xs px-5 py-2.5 rounded-xl transition inline-flex items-center gap-2">
        <i class="fa-solid fa-print"></i> طباعة / حفظ PDF
      </button>
      <button onclick="closeCertificateModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-200 font-bold text-xs px-5 py-2.5 rounded-xl transition inline-flex items-center gap-2">
        <i class="fa-solid fa-xmark"></i> إغلاق
      </button>
    </div>

    <!-- الشهادة الرسمية -->
    <div id="certificate" class="cert-frame rounded-2xl">
      <div class="cert-border flex flex-col justify-between">
        
        <!-- ترويسة الشهادة -->
        <div class="flex items-center justify-between mb-6">
          <div class="flex items-center gap-3 text-right">
            <div class="w-14 h-14 rounded-full bg-emerald-600 text-white flex items-center justify-center text-2xl font-bold shadow-md">
              <i class="fa-solid fa-flask"></i>
            </div>
            <div>
              <div class="font-extrabold text-slate-900 text-base">برنامج موهبة المتقدم</div>
              <div class="text-xs text-slate-600">في العلوم والرياضيات</div>
            </div>
          </div>
          <div class="text-left text-xs text-slate-500">
            <div>رقم الاعتماد التسلسلي:</div>
            <div id="cert-serial" class="font-bold text-slate-900 text-sm dir-ltr">MAW-2026-9821</div>
          </div>
        </div>

        <!-- العنوان -->
        <div class="text-center my-4">
          <div class="text-[11px] font-bold tracking-widest text-emerald-800 uppercase">Certificate of Achievement</div>
          <h1 class="font-amiri text-4xl sm:text-5xl font-extrabold text-emerald-900 my-2">شهـــادة إنجـــاز وتفــوق</h1>
          <p class="text-xs text-slate-600">تُمنح هذه الشهادة بكل فخر واعتزاز إلى الطالب/ـة:</p>
        </div>

        <!-- اسم الطالب -->
        <div class="text-center my-2">
          <div class="inline-block px-10 py-2 border-b-2 border-emerald-600 min-w-[280px]">
            <span id="cert-name" class="font-amiri text-3xl sm:text-4xl font-extrabold text-slate-900">عالم موهبة</span>
          </div>
        </div>

        <!-- نص الشهادة -->
        <p class="text-center text-xs sm:text-sm text-slate-700 leading-relaxed max-w-xl mx-auto my-4">
          نظير اجتيازه بنجاح <b>الاختبار الإلكتروني التفاعلي للعلوم - الصف الخامس الابتدائي</b><br>
          الوحدة الأولى: <b>العلاقات والنسب في حياتنا</b>، برحلة <span id="cert-level" class="font-bold text-emerald-800">أبطال موهبة</span>، محققاً النتيجة الآتية:
        </p>

        <!-- كروت إحصائيات الشهادة -->
        <div class="grid grid-cols-4 gap-2 my-4 max-w-lg mx-auto w-full text-center">
          <div class="bg-emerald-50 border border-emerald-200 p-2 rounded-xl">
            <div class="text-[10px] text-slate-500 font-bold">النقاط الكلية</div>
            <div id="cert-score" class="text-base font-extrabold text-emerald-800">0</div>
          </div>
          <div class="bg-emerald-50 border border-emerald-200 p-2 rounded-xl">
            <div class="text-[10px] text-slate-500 font-bold">الإجابات الصحيحة</div>
            <div id="cert-correct" class="text-base font-extrabold text-emerald-800">0</div>
          </div>
          <div class="bg-emerald-50 border border-emerald-200 p-2 rounded-xl">
            <div class="text-[10px] text-slate-500 font-bold">نسبة الإتقان</div>
            <div id="cert-percentage" class="text-base font-extrabold text-emerald-800">0%</div>
          </div>
          <div class="bg-emerald-50 border border-emerald-200 p-2 rounded-xl">
            <div class="text-[10px] text-slate-500 font-bold">التقدير العام</div>
            <div id="cert-grade" class="text-xs font-extrabold text-emerald-800 mt-1">ممتاز</div>
          </div>
        </div>

        <!-- التوقيع والتاريخ -->
        <div class="flex justify-between items-end mt-6 pt-4 border-t border-slate-200 text-xs">
          <div class="text-right text-slate-600">
            <div><b>تاريخ الإصدار:</b> <span id="cert-date">13 سبتمبر 2026</span></div>
            <div class="text-[10px] text-slate-400">معتمد إلكترونياً من لجنة العلوم</div>
          </div>
          <div class="text-center">
            <div class="w-16 h-16 border-2 border-emerald-600 rounded-full flex items-center justify-center text-emerald-800 font-black text-xs rotate-12 mb-1 mx-auto">
              موهبة MAWHIBA
            </div>
            <div class="font-bold text-slate-800">المشرف الأكاديمي</div>
          </div>
        </div>

      </div>
    </div>

  </div>
</div>

<!-- ================= 5. جافا سكريبت الكامل ================= -->
<script>
class SoundEngine {
  constructor() {
    this.ctx = null;
    this.muted = false;
  }
  init() {
    if(!this.ctx) {
      this.ctx = new (window.AudioContext || window.webkitAudioContext)();
    }
  }
  playTone(freq, type, duration, gainVal=0.15) {
    if(this.muted) return;
    this.init();
    if(this.ctx.state === 'suspended') this.ctx.resume();
    const osc = this.ctx.createOscillator();
    const gain = this.ctx.createGain();
    osc.type = type;
    osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
    gain.gain.setValueAtTime(gainVal, this.ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.0001, this.ctx.currentTime + duration);
    osc.connect(gain);
    gain.connect(this.ctx.destination);
    osc.start();
    osc.stop(this.ctx.currentTime + duration);
  }
  playCorrect() {
    this.playTone(523.25, 'sine', 0.12);
    setTimeout(() => this.playTone(659.25, 'sine', 0.12), 80);
    setTimeout(() => this.playTone(783.99, 'sine', 0.25), 160);
  }
  playWrong() {
    this.playTone(220, 'sawtooth', 0.15);
    setTimeout(() => this.playTone(164.81, 'sawtooth', 0.25), 120);
  }
  playPowerup() {
    this.playTone(880, 'triangle', 0.2);
  }
  playVictory() {
    [523.25, 659.25, 783.99, 1046.50].forEach((f, i) => {
      setTimeout(() => this.playTone(f, 'sine', 0.3), i * 120);
    });
  }
}

const sfx = new SoundEngine();

function toggleSound() {
  sfx.muted = !sfx.muted;
  document.getElementById('sound-icon').className = sfx.muted ? 'fa-solid fa-volume-xmark' : 'fa-solid fa-volume-high';
}

const questionBank = [
  // Easy
  { level:'easy', cat:'المفاهيم الأساسية', q:'ما المقصود بـ "النسبة" في الرياضيات والعلوم؟', opts:['المقارنة بين كميتين من نفس النوع توضح كم تحتوي إحداهما على الأخرى','مجموع قيمتين مختلفين في وحدة القياس','الفرق الناتج من طرح الكمية الأولى من الثانية','حاصل ضرب كميتين في التجارب العلمية'], ans:0, exp:'ممتاز! النسبة مقارنة رياضية بين كميتين من نفس النوع والوحدة.' },
  { level:'easy', cat:'تطبيقات النسب', q:'إذا كان طول ذراع طالب 40 سم وطول جسمه الكلي 160 سم، فما نسبة طول الذراع إلى الطول الكلي؟', opts:['1 : 2','1 : 4','1 : 3','1 : 5'], ans:1, exp:'صحيح! 40 ÷ 160 = 1/4 بعد التبسيط.' },
  { level:'easy', cat:'وحدات القياس', q:'أي مما يلي يُعد مثالاً على "قياس غير معياري"؟', opts:['استخدام المتر لقياس طول الغرفة','استخدام الكيلوجرام لقياس الكتلة','استخدام الذراع أو الشبر كوحدة قياس','استخدام اللتر لقياس الحجم'], ans:2, exp:'رائع! القياس غير المعياري يختلف باختلاف جسم الأشخاص كالذراع والشبر.' },
  { level:'easy', cat:'النسبة الذهبية', q:'ما القيمة التقريبية للنسبة الذهبية (Golden Ratio) التي تظهر في الطبيعة والهندسة المعمارية؟', opts:['3.14','1.618','2.718','0.618'], ans:1, exp:'أحسنت! النسبة الذهبية تساوي تقريباً 1.618.' },
  { level:'easy', cat:'القياس المعياري', q:'أي مما يلي يُعد معياراً موحداً لا يتغير بين الأشخاص والدول؟', opts:['طول قدم الشخص','عرض كف اليد','وحدة المتر الدولي','طول ذراع الملك'], ans:2, exp:'صحيح! المتر الدولي وحدة قياس معيارية ثابتة.' },
  { level:'easy', cat:'مقارنة الحجوم', q:'وعاء به 500 مل من الماء وآخر به 1 لتر. ما نسبة حجم الماء في الوعاء الأول إلى الثاني؟', opts:['1 : 2','1 : 1','2 : 1','1 : 4'], ans:0, exp:'ممتاز! 1 لتر = 1000 مل، إذن 500 : 1000 تبسط إلى 1 : 2.' },
  { level:'easy', cat:'العلاقات البصرية', q:'شجرة طول ظلها في منتصف النهار 2 متر وطول الشجرة 6 أمتار. ما نسبة طول الظل إلى طول الشجرة؟', opts:['1 : 3','1 : 2','3 : 1','1 : 6'], ans:0, exp:'صحيح! 2 ÷ 6 = 1/3.' },
  { level:'easy', cat:'أدوات القياس', q:'ما أداة القياس الأكثر دقة لقياس حجم كمية صغيرة من السائل في المختبر؟', opts:['المخبار المدرج','الكأس الزجاجي الكبير','المسطرة الخشبية','الميزان ذو الكفتين'], ans:0, exp:'أحسنت! المخبار المدرج مصمم لقياس حجوم السوائل بدقة.' },

  // Medium
  { level:'medium', cat:'النسب في أجزاء الجسم', q:'إذا كانت نسبة طول رأس الإنسان إلى طول جسمه الإجمالي في مرحلة البلوغ هي 1 : 8، وطول الشخص 176 سم، فما طول رأسه؟', opts:['22 سم','20 سم','24 سم','18 سم'], ans:0, exp:'رائع! 176 ÷ 8 = 22 سم.' },
  { level:'medium', cat:'العلاقات والنسب', q:'مخلوط من الماء والملح نسبته 4 : 1 (ماء : ملح). إذا استخدمنا 200 جرام من الماء، فكم جراماً من الملح نحتاج؟', opts:['50 جرام','40 جرام','100 جرام','25 جرام'], ans:0, exp:'ممتاز! 200 ÷ 4 = 50 جرام ملح.' },
  { level:'medium', cat:'الأشكال الهندسيّة', q:'مستطيل طوله 15 سم وعرضه 10 سم. ما نسبة عرض المستطيل إلى محيطه الكلي؟', opts:['1 : 5','1 : 3','2 : 5','1 : 4'], ans:0, exp:'أحسنت! المحيط = 2*(15+10) = 50 سم. النسبة 10 : 50 = 1 : 5.' },
  { level:'medium', cat:'النسبة الذهبية في الطبيعة', q:'أين تتجلى النسبة الذهبية بوضوح في الكائنات الحية؟', opts:['في نمو حلزون القوقعة وتناسق أوراق النباتات','في عدد أرجُل الحشرات فقط','في لون صبغة الكلوفيل','في قياس سرعة الصوت في الماء'], ans:0, exp:'صحيح! تظهر النسبة الذهبية في اللولب الحلزوني وترتيب بتلات الأزهار.' },
  { level:'medium', cat:'التحويلات والنسب', q:'كتلة مكعب خشبي 300 جرام وكتلة مكعب حديدي من نفس الحجم 2400 جرام. ما نسبة كتلة الخشب إلى كتلة الحديد؟', opts:['1 : 8','1 : 4','1 : 6','1 : 2'], ans:0, exp:'ممتاز! 300 ÷ 2400 = 1/8.' },
  { level:'medium', cat:'تجارب النمو', q:'نبتة نمت بمقدار 3 سم في الأسبوع الأول و 9 سم في الأسبوع الثاني. ما نسبة نمو الأسبوع الأول إلى إجمالي النمو؟', opts:['1 : 4','1 : 3','1 : 2','3 : 4'], ans:0, exp:'إجمالي النمو = 3 + 9 = 12 سم. النسبة = 3 : 12 = 1 : 4.' },
  { level:'medium', cat:'الكثافة والنسبة', q:'إذا كانت نسبة كثافة الزيت إلى كثافة الماء هي 0.8 : 1، فماذا يحدث عند خلطهما؟', opts:['يطفو الزيت فوق الماء لأن كثافته أقل','ينغمر الزيت تحت الماء','يدوب الزيت تماماً في الماء','يتجمد المزيج مباشرة'], ans:0, exp:'رائع! السائل ذو الكثافة الأقل (الزيت) يطفو فوق الأعلى.' },
  { level:'medium', cat:'النسب في الضوء', q:'مرآة تعكس 90% من الضوء الساقط عليها. ما نسبة الضوء الممتص إلى الضوء المنعكس؟', opts:['1 : 9','1 : 10','9 : 10','1 : 8'], ans:0, exp:'المنعكس = 90% والممتص = 10%. النسبة 10 : 90 = 1 : 9.' },
  { level:'medium', cat:'الرسم البياني', q:'إذا كان مقياس الرسم في الخريطة هو 1 : 1000، وكل 1 سم على الخريطة يمثل في الواقع:', opts:['10 أمتار','1 متر','100 متر','1000 متر'], ans:0, exp:'صحيح! 1000 سم = 10 أمتار.' },

  // Hard
  { level:'hard', cat:'التحليل المتقدم', q:'عينة من سبيكة تحتوي على النحاس والزنك بنسبة 3 : 2. إذا كان وزن السبيكة الكلي 500 جرام، فما وزن النحاس فيها؟', opts:['300 جرام','200 جرام','250 جرام','350 جرام'], ans:0, exp:'مجموع الأجزاء = 3 + 2 = 5. قيمة الجزء = 500 ÷ 5 = 100. النحاس = 3 * 100 = 300 جرام.' },
  { level:'hard', cat:'التكبير والتصغير', q:'تم فحص خلية بكتيرية تحت مجهر يكبّر بمقدار 400 مرة، فظهر طولها 2 مم. ما طول الخلية الحقيقي بالمايكرومتر؟ (علماً أن 1 مم = 1000 مايكرومتر)', opts:['5 مايكرومتر','20 مايكرومتر','50 مايكرومتر','2 مايكرومتر'], ans:0, exp:'الطول الحقيقي بالـ مم = 2 ÷ 400 = 0.005 مم. بالمايكرومتر = 0.005 * 1000 = 5 مايكرومتر.' },
  { level:'hard', cat:'قانون التركيز', q:'محلول سكري وزنه الكلي 250 جرام ويحتوي على 50 جرام سكر. ما نسبة السكر إلى الماء في هذا المحلول؟', opts:['1 : 4','1 : 5','1 : 3','2 : 5'], ans:0, exp:'وزن الماء = 250 - 50 = 200 جرام. النسبة سكر : ماء = 50 : 200 = 1 : 4.' },
  { level:'hard', cat:'السرعة والنسبة', q:'سيارة أ قطعت 120 كم في ساعتين، وسيارة ب قطعت 180 كم في 3 ساعات. ما نسبة سرعة السيارة أ إلى سرعة السيارة ب؟', opts:['1 : 1','2 : 3','3 : 4','4 : 3'], ans:0, exp:'سرعة أ = 120/2 = 60 كم/س. سرعة ب = 180/3 = 60 كم/س. النسبة = 60 : 60 = 1 : 1.' },
  { level:'hard', cat:'التغذية والرقم الهيدروجيني', q:'نسبة حمضية المحلول أ هي pH=4 والمحلول ب pH=6. كم مرة يزيد تركيز أيونات الهيدروجين في أ مقارنة بـ ب؟ (كل درجة بمقدار 10 أضعاف)', opts:['100 مرة','10 مرات','20 مرة','2 مرة'], ans:0, exp:'الفرق درجتان (6 - 4 = 2). إذن التركيز يزيد 10 * 10 = 100 مرة!' },
  { level:'hard', cat:'التوازن البيئي', q:'في نظام بيئي مغلق، نسبة المنتجات (النباتات) إلى المستهلكات هي 10 : 1. إذا اختفت نصف النباتات، فما النسبة الجديدة للاتزان؟', opts:['5 : 1','2 : 1','10 : 2','1 : 5'], ans:0, exp:'صحيح! 10 تُصبح 5، فتصبح النسبة الجديدة 5 : 1.' }
];

let selectedLevel = 'medium';
let activeQuestions = [];
let currentQIndex = 0;
let score = 0;
let streak = 0;
let maxStreak = 0;
let correctCount = 0;
let timerInterval = null;
let timeLeft = 25;
let questionTime = 25;
let userAnswers = [];

function selectLevel(level) {
  selectedLevel = level;
  document.querySelectorAll('.diff-card').forEach(card => {
    card.classList.remove('bg-amber-500/10', 'border-amber-400', 'bg-emerald-500/10', 'border-emerald-400', 'bg-rose-500/10', 'border-rose-400', 'bg-purple-500/10', 'border-purple-400');
  });
  
  const selectedCard = document.getElementById(`level-${level}`);
  if(level === 'easy') selectedCard.classList.add('bg-emerald-500/10', 'border-emerald-400');
  else if(level === 'medium') selectedCard.classList.add('bg-amber-500/10', 'border-amber-400');
  else if(level === 'hard') selectedCard.classList.add('bg-rose-500/10', 'border-rose-400');
  else selectedCard.classList.add('bg-purple-500/10', 'border-purple-400');
}

function startGame() {
  sfx.init();
  const nameInput = document.getElementById('student-name-input').value.trim();
  
  // Filter questions based on level
  if(selectedLevel === 'all') {
    activeQuestions = [...questionBank].sort(() => Math.random() - 0.5);
    questionTime = 25;
  } else {
    activeQuestions = questionBank.filter(q => q.level === selectedLevel).sort(() => Math.random() - 0.5);
    questionTime = selectedLevel === 'easy' ? 30 : (selectedLevel === 'medium' ? 25 : 20);
  }

  currentQIndex = 0;
  score = 0;
  streak = 0;
  maxStreak = 0;
  correctCount = 0;
  userAnswers = [];

  document.getElementById('start-screen').classList.add('hidden');
  document.getElementById('end-screen').classList.add('hidden');
  document.getElementById('game-screen').classList.remove('hidden');

  // Reset powerups
  document.getElementById('powerup-5050').disabled = false;
  document.getElementById('powerup-freeze').disabled = false;

  updateHeaderUI();
  displayQuestion();
}

function updateHeaderUI() {
  document.getElementById('score-counter').innerText = score;
  document.getElementById('streak-counter').innerText = streak;
}

function displayQuestion() {
  if(currentQIndex >= activeQuestions.length) {
    finishGame();
    return;
  }

  const q = activeQuestions[currentQIndex];
  document.getElementById('question-progress-badge').innerText = `السؤال ${currentQIndex + 1} من ${activeQuestions.length}`;
  document.getElementById('category-badge').innerText = q.cat;
  document.getElementById('question-text').innerText = q.q;

  document.getElementById('feedback-box').classList.add('hidden');
  document.getElementById('next-btn').classList.add('hidden');

  const container = document.getElementById('options-container');
  container.innerHTML = '';

  q.opts.forEach((opt, idx) => {
    const btn = document.createElement('button');
    btn.className = "opt-btn bg-slate-900/90 hover:bg-slate-800 text-white font-bold p-4 rounded-2xl text-right text-sm sm:text-base flex items-center justify-between";
    btn.onclick = () => selectAnswer(idx);
    btn.innerHTML = `
      <span class="leading-snug">${opt}</span>
      <span class="w-7 h-7 rounded-lg bg-white/10 flex items-center justify-center text-xs text-slate-300 font-sans shrink-0 ml-2">${['أ','ب','ج','د'][idx]}</span>
    `;
    container.appendChild(btn);
  });

  resetTimer();
}

function resetTimer() {
  clearInterval(timerInterval);
  timeLeft = questionTime;
  updateTimerBar();

  timerInterval = setInterval(() => {
    timeLeft--;
    updateTimerBar();
    if(timeLeft <= 0) {
      clearInterval(timerInterval);
      sfx.playWrong();
      selectAnswer(-1); // Timeout
    }
  }, 1000);
}

function updateTimerBar() {
  document.getElementById('timer-text').innerText = timeLeft;
  const pct = (timeLeft / questionTime) * 100;
  document.getElementById('timer-bar').style.width = `${pct}%`;
}

function selectAnswer(selectedIdx) {
  clearInterval(timerInterval);
  const q = activeQuestions[currentQIndex];
  const buttons = document.querySelectorAll('#options-container button');
  
  buttons.forEach(b => b.disabled = true);

  const isCorrect = selectedIdx === q.ans;
  userAnswers.push({ question: q, selected: selectedIdx, isCorrect: isCorrect });

  const fbBox = document.getElementById('feedback-box');
  const fbTitle = document.getElementById('feedback-title');
  const fbDetail = document.getElementById('feedback-detail');
  const fbIcon = document.getElementById('feedback-icon');

  if(isCorrect) {
    sfx.playCorrect();
    if(selectedIdx >= 0) buttons[selectedIdx].classList.add('correct-anim');
    
    streak++;
    if(streak > maxStreak) maxStreak = streak;
    correctCount++;

    const bonus = (streak * 15) + (timeLeft * 5);
    score += 100 + bonus;

    fbBox.className = "p-4 sm:p-5 rounded-2xl mb-5 border text-right bg-emerald-950/80 border-emerald-500/50 text-emerald-200";
    fbIcon.innerHTML = '<i class="fa-solid fa-circle-check text-emerald-400"></i>';
    fbTitle.innerText = "إجابة صحيحة وعبقرية! 🎉";
  } else {
    sfx.playWrong();
    if(selectedIdx >= 0) buttons[selectedIdx].classList.add('wrong-anim');
    buttons[q.ans].classList.add('correct-anim');

    streak = 0;

    fbBox.className = "p-4 sm:p-5 rounded-2xl mb-5 border text-right bg-rose-950/80 border-rose-500/50 text-rose-200";
    fbIcon.innerHTML = '<i class="fa-solid fa-circle-xmark text-rose-400"></i>';
    fbTitle.innerText = selectedIdx === -1 ? "انتهى الوقت المحدد! ⏰" : "إجابة خاطئة 💡";
  }

  fbDetail.innerText = q.exp;
  fbBox.classList.remove('hidden');

  updateHeaderUI();

  // Next button label
  const nextBtnText = document.querySelector('#next-btn span');
  if(currentQIndex === activeQuestions.length - 1) {
    nextBtnText.innerText = "عرض النتائج النهائية";
  } else {
    nextBtnText.innerText = "السؤال التالي";
  }
  document.getElementById('next-btn').classList.remove('hidden');
}

function nextQuestion() {
  currentQIndex++;
  displayQuestion();
}

function usePowerup5050() {
  sfx.playPowerup();
  const q = activeQuestions[currentQIndex];
  const buttons = document.querySelectorAll('#options-container button');
  let removed = 0;

  buttons.forEach((btn, idx) => {
    if(idx !== q.ans && removed < 2) {
      btn.style.opacity = '0.2';
      btn.disabled = true;
      removed++;
    }
  });

  document.getElementById('powerup-5050').disabled = true;
}

function usePowerupFreeze() {
  sfx.playPowerup();
  timeLeft += 10;
  updateTimerBar();
  document.getElementById('powerup-freeze').disabled = true;
}

function finishGame() {
  sfx.playVictory();
  document.getElementById('game-screen').classList.add('hidden');
  document.getElementById('end-screen').classList.remove('hidden');

  const percentage = Math.round((correctCount / activeQuestions.length) * 100);

  document.getElementById('final-score').innerText = score;
  document.getElementById('final-correct').innerText = `${correctCount} / ${activeQuestions.length}`;
  document.getElementById('final-percentage').innerText = `${percentage}%`;
  document.getElementById('final-streak').innerText = maxStreak;

  // Render review list
  const reviewList = document.getElementById('review-list');
  reviewList.innerHTML = '';

  userAnswers.forEach((ansObj, idx) => {
    const item = document.createElement('div');
    item.className = `p-3 rounded-xl border ${ansObj.isCorrect ? 'bg-emerald-950/40 border-emerald-500/30' : 'bg-rose-950/40 border-rose-500/30'}`;
    item.innerHTML = `
      <div class="flex items-center justify-between text-xs font-bold mb-1">
        <span class="${ansObj.isCorrect ? 'text-emerald-400' : 'text-rose-400'}">السؤال ${idx+1}: ${ansObj.isCorrect ? 'إجابة صحيحة' : 'تحتاج مراجعة'}</span>
        <span class="text-slate-400">${ansObj.question.cat}</span>
      </div>
      <p class="text-xs text-slate-200 font-medium mb-1">${ansObj.question.q}</p>
      <p class="text-[11px] text-slate-400"><b class="text-emerald-400">التفسير:</b> ${ansObj.question.exp}</p>
    `;
    reviewList.appendChild(item);
  });

  document.getElementById('review-container').classList.remove('hidden');

  if(percentage >= 70) {
    confetti({ particleCount: 100, spread: 70, origin: { y: 0.6 } });
  }
}

function backToStart() {
  document.getElementById('end-screen').classList.add('hidden');
  document.getElementById('start-screen').classList.remove('hidden');
}

function openCertificateModal() {
  const name = document.getElementById('student-name-input').value.trim() || "عالم موهبة المبدع";
  const percentage = Math.round((correctCount / activeQuestions.length) * 100);

  document.getElementById('cert-name').innerText = name;
  document.getElementById('cert-score').innerText = score;
  document.getElementById('cert-correct').innerText = `${correctCount} / ${activeQuestions.length}`;
  document.getElementById('cert-percentage').innerText = `${percentage}%`;

  // Grade evaluation
  let gradeText = "ممتاز مرتفع";
  if(percentage < 60) gradeText = "مقبول";
  else if(percentage < 75) gradeText = "جيد جداً";
  else if(percentage < 90) gradeText = "ممتاز";
  document.getElementById('cert-grade').innerText = gradeText;

  // Level label
  const levelLabels = { easy: 'أبطال العلوم', medium: 'المبدعون', hard: 'المتألقون', all: 'أبطال موهبة' };
  document.getElementById('cert-level').innerText = levelLabels[selectedLevel] || 'أبطال موهبة';

  // Date
  const today = new Date();
  document.getElementById('cert-date').innerText = today.toLocaleDateString('ar-SA', { year: 'numeric', month: 'long', day: 'numeric' });

  // Random serial
  document.getElementById('cert-serial').innerText = `MAW-${Math.floor(1000 + Math.random() * 9000)}-2026`;

  document.getElementById('cert-modal').classList.remove('hidden');
  document.getElementById('cert-modal').classList.add('flex');
}

function closeCertificateModal() {
  document.getElementById('cert-modal').classList.add('hidden');
  document.getElementById('cert-modal').classList.remove('flex');
}
</script>
</body>
</html>
