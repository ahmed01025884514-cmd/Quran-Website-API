<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>إذاعة القرآن الكريم | بث مباشر</title>
    <link href="https://fonts.googleapis.com/css2?family=Amiri:wght@400;700&family=Tajawal:wght@300;500;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    
    <style>
        :root {
            --gold: #d4af37;
            --deep-green: #0a2e2a;
            --white: #ffffff;
            --glass: rgba(255, 255, 255, 0.1);
        }

        body {
            font-family: 'Tajawal', sans-serif;
            margin: 0;
            padding: 0;
            background: #051917; /* لون خلفية غامق */
            color: var(--white);
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* خلفية متحركة (Animated Particles) */
        .bg-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle at center, #0e3d38 0%, #051917 100%);
            z-index: -1;
        }

        /* زخرفة إسلامية متحركة في الخلفية */
        .islamic-pattern {
            position: fixed;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            width: 800px; height: 800px;
            background: url('https://www.transparenttextures.com/patterns/arabesque.png');
            opacity: 0.1;
            animation: rotatePattern 100s linear infinite;
            z-index: -1;
        }

        @keyframes rotatePattern { from { transform: translate(-50%, -50%) rotate(0deg); } to { transform: translate(-50%, -50%) rotate(360deg); } }

        /* Header */
        header {
            text-align: center;
            padding: 60px 20px;
            background: linear-gradient(to bottom, rgba(10, 46, 42, 0.8), transparent);
        }

        header h1 {
            font-family: 'Amiri', serif;
            font-size: 3.5rem;
            color: var(--gold);
            text-shadow: 0 0 20px rgba(212, 175, 55, 0.5);
            margin-bottom: 10px;
        }

        /* Search Box */
        .search-container {
            max-width: 600px;
            margin: -30px auto 40px;
            padding: 0 20px;
        }

        .search-container input {
            width: 100%;
            padding: 15px 25px;
            border-radius: 50px;
            border: 1px solid var(--gold);
            background: rgba(255, 255, 255, 0.05);
            color: white;
            font-size: 1.1rem;
            backdrop-filter: blur(5px);
            text-align: center;
            transition: 0.3s;
        }

        .search-container input:focus {
            outline: none;
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 0 20px rgba(212, 175, 55, 0.3);
        }

        /* Grid */
        .radios-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 25px;
            padding: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        /* Card Styling */
        .radio-card {
            background: var(--glass);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(212, 175, 55, 0.2);
            border-radius: 20px;
            padding: 25px;
            text-align: center;
            transition: 0.4s ease;
            position: relative;
            overflow: hidden;
        }

        .radio-card::before {
            content: "";
            position: absolute;
            top: -50%; left: -50%;
            width: 200%; height: 200%;
            background: radial-gradient(circle, rgba(212, 175, 55, 0.1) 0%, transparent 70%);
            opacity: 0; transition: 0.5s;
        }

        .radio-card:hover {
            transform: translateY(-10px) scale(1.02);
            border-color: var(--gold);
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
        }

        .radio-card:hover::before { opacity: 1; }

        .radio-card i {
            font-size: 2.5rem;
            color: var(--gold);
            margin-bottom: 15px;
        }

        .radio-card h3 {
            font-size: 1.2rem;
            margin-bottom: 20px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Audio Player Styling */
        audio {
            width: 100%;
            filter: sepia(20%) saturate(70%) grayscale(1) contrast(90%) invert(100%);
            height: 35px;
        }

        /* Footer */
        footer {
            text-align: center;
            padding: 50px 20px;
            margin-top: 50px;
            border-top: 1px solid rgba(212, 175, 55, 0.2);
        }

        .signature {
            color: var(--gold);
            font-weight: 800;
            letter-spacing: 1px;
        }

        /* Responsive */
        @media (max-width: 600px) {
            header h1 { font-size: 2.2rem; }
            .radios-grid { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

<div class="bg-overlay"></div>
<div class="islamic-pattern"></div>

<header>
    <h1 class="animate__animated animate__fadeInDown">صوتُ الهدى</h1>
    <p class="animate__animated animate__fadeInUp">بث مباشر لإذاعات القرآن الكريم بصوت كبار القراء</p>
</header>

<div class="search-container">
    <input type="text" id="quranSearch" placeholder="🔍 ابحث عن قارئ أو إذاعة...">
</div>

<main class="radios-grid" id="radios-list">
    </main>

<footer>
    <p>تم التطوير لخدمة الإسلام والمسلمين</p>
    <p>بواسطة المساعد الذكي: <span class="signature">Gemini AI</span></p>
</footer>

<script>
    const radiosList = document.getElementById('radios-list');
    const searchInput = document.getElementById('quranSearch');
    let radiosData = [];

    // جلب البيانات
    async function getRadios() {
        try {
            const response = await fetch('https://mp3quran.net/api/v3/radios');
            const data = await response.json();
            radiosData = data.radios;
            renderRadios(radiosData);
        } catch (error) {
            radiosList.innerHTML = `<p>عذراً، حدث خطأ في تحميل البيانات. تأكد من اتصالك بالإنترنت.</p>`;
        }
    }

    // عرض الكروت
    function renderRadios(list) {
        radiosList.innerHTML = '';
        list.forEach((radio, index) => {
            const card = document.createElement('div');
            card.className = 'radio-card animate__animated animate__fadeInUp';
            card.style.animationDelay = `${index * 0.05}s`;
            card.innerHTML = `
                <i class="fa-solid fa-kaaba"></i>
                <h3>${radio.name}</h3>
                <audio controls preload="none">
                    <source src="${radio.url}" type="audio/mpeg">
                </audio>
            `;
            radiosList.appendChild(card);
        });
    }

    // البحث
    searchInput.addEventListener('input', (e) => {
        const query = e.target.value.toLowerCase();
        const filtered = radiosData.filter(r => r.name.toLowerCase().includes(query));
        renderRadios(filtered);
    });

    getRadios();
</script>

</body>
</html>
