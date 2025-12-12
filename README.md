<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>沖繩之旅 OKINAWA TRIP</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@400;700&display=swap" rel="stylesheet">

    <style>
        /* 全域設定：微軟正黑體 + 無印良品風格 */
        :root {
            --muji-bg: #f7f7f7;
            --muji-card: #ffffff;
            --muji-text: #333333;
            --muji-accent: #7b7b7b;
            --muji-highlight: #8c9fa3;
            --color-blue: #e9f5ff; /* 淺藍色背景 */
            --color-yellow: #fff8e1; /* 淺黃色背景 */
        }

        body {
            background-color: var(--muji-bg);
            color: var(--muji-text);
            /* 優先使用微軟正黑體，若瀏覽器不支援則依序降級 */
            font-family: "Microsoft JhengHei", "Noto Sans TC", system-ui, sans-serif;
            -webkit-font-smoothing: antialiased;
            padding-bottom: 80px;
        }

        h1, h2, h3, .serif {
            /* 標題使用襯線體 */
            font-family: 'Noto Serif TC', serif;
        }

        /* 隱藏 Scrollbar 但保留功能 */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }

        /* 卡片風格 */
        .card {
            background-color: var(--muji-card);
            border-radius: 8px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.04);
            border: 1px solid #e5e5e5;
            transition: transform 0.2s;
        }

        /* 按鈕風格 */
        .btn-muji {
            background-color: #555;
            color: white;
            border-radius: 4px;
            padding: 8px 16px;
            font-size: 0.9rem;
            transition: opacity 0.2s;
        }
        .btn-muji:active { opacity: 0.8; }
        
        .btn-outline {
            border: 1px solid #aaa;
            color: #555;
            background: transparent;
            border-radius: 4px;
            padding: 6px 12px;
        }

        /* 底部導覽列 */
        .nav-item.active {
            color: #333;
            font-weight: bold;
        }
        .nav-item.active i {
            color: #333;
        }
        .nav-item {
            color: #aaa;
            font-size: 0.75rem;
        }

        /* 自定義 Checkbox */
        .custom-checkbox:checked + div {
            text-decoration: line-through;
            color: #bbb;
        }
        
        /* 新增表格配色 */
        .table-blue { background-color: var(--color-blue); }
        .table-yellow { background-color: var(--color-yellow); }
    </style>
</head>
<body class="max-w-md mx-auto min-h-screen shadow-lg relative bg-[#f9f9f9]">

    <header class="sticky top-0 z-50 bg-[#f9f9f9]/90 backdrop-blur-sm p-4 border-b border-gray-200 flex justify-between items-center">
        <div>
            <h1 class="text-xl font-bold tracking-widest text-gray-800">OKINAWA</h1>
            <p class="text-xs text-gray-500 tracking-wide">12.13 - 12.16</p>
        </div>
        <div id="header-weather" class="text-right text-xs text-gray-500">
            <a href="https://www.jma.go.jp/bosai/forecast/" target="_blank" class="hover:underline">
                <i class="fas fa-cloud-sun"></i> 查看天氣
            </a>
        </div>
    </header>

    <main id="main-content" class="p-4 space-y-6">
        </main>

    <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto bg-white border-t border-gray-200 flex justify-around py-3 z-50 pb-safe">
        <button onclick="router('plan')" class="nav-item flex flex-col items-center w-1/5 active" id="nav-plan">
            <i class="fas fa-calendar-alt text-lg mb-1"></i>
            <span>行程</span>
        </button>
        <button onclick="router('guide')" class="nav-item flex flex-col items-center w-1/5" id="nav-guide">
            <i class="fas fa-book-open text-lg mb-1"></i>
            <span>導覽</span>
        </button>
        <button onclick="router('wallet')" class="nav-item flex flex-col items-center w-1/5" id="nav-wallet">
            <i class="fas fa-wallet text-lg mb-1"></i>
            <span>記帳</span>
        </button>
        <button onclick="router('lists')" class="nav-item flex flex-col items-center w-1/5" id="nav-lists">
            <i class="fas fa-check-square text-lg mb-1"></i>
            <span>清單</span>
        </button>
        <button onclick="router('info')" class="nav-item flex flex-col items-center w-1/5" id="nav-info">
            <i class="fas fa-info-circle text-lg mb-1"></i>
            <span>資訊</span>
        </button>
        </nav>

    <script>
        // --- DATA ---
        const itinerary = [
            {
                date: "12/13 (六)",
                title: "D1 啟程 / 那霸",
                activities: [
                    { time: "13:50", title: "集合｜高雄小港機場", desc: "國際線 3 樓華航櫃台，領隊：林金祥（黃色行李牌）", highlight: true },
                    { time: "15:50", title: "起飛 ✈️ CI132", desc: "前往琉球那霸機場" },
                    { time: "18:30", title: "抵達那霸", desc: "辦理入境手續" },
                    { time: "晚間", title: "入住飯店", desc: "那霸格蘭蒂亞飯店 (Granvia/Grandtia)", link: "那霸格蘭蒂亞飯店" }
                ]
            },
            {
                date: "12/14 (日)",
                title: "D2 山原探險 / 北部",
                activities: [
                    { time: "09:00", title: "山原森林遊客中心", desc: "了解沖繩北部自然景觀", link: "山原森林遊客中心" },
                    { time: "10:30", title: "大石林山 輕健行", desc: "奇岩怪石、海景眺望 (ASMUI Spiritual Hikes)", link: "大石林山" },
                    { time: "12:30", title: "午餐：琉風定食", desc: "" },
                    { time: "14:00", title: "邊戶岬展望台", desc: "沖繩最北端壯麗海景", link: "邊戶岬" },
                    { time: "17:00", title: "入住飯店 / 晚餐", desc: "喜璃癒志海灘渡假飯店 (自助晚餐)", link: "Okinawa Kariyushi Beach Resort Ocean Spa" }
                ]
            },
            {
                date: "12/15 (一)",
                title: "D3 海洋與購物 / 中南部",
                activities: [
                    { time: "09:00", title: "古宇利島", desc: "海洋塔、無人電瓶車、貝殼館", link: "古宇利島海洋塔" },
                    { time: "11:30", title: "美麗海水族館", desc: "海洋博公園、黑潮之海、海豚表演", link: "沖繩美麗海水族館" },
                    { time: "13:00", title: "午餐：琉球定食", desc: "" },
                    { time: "16:00", title: "國際通大道", desc: "自由逛街、藥妝、晚餐自理", link: "那霸國際通" },
                    { time: "晚間", title: "入住飯店", desc: "國際通蘭塔納酒店 (Hotel Lantana)", link: "HOTEL LANTANA NAHA" }
                ]
            },
            {
                date: "12/16 (二)",
                title: "D4 文化巡禮 / 返程",
                activities: [
                    { time: "09:00", title: "玉泉洞", desc: "日本第二長鐘乳石洞", link: "沖繩世界文化王國" },
                    { time: "10:30", title: "王國村", desc: "琉球太鼓隊表演", link: "王國村" },
                    { time: "12:00", title: "午餐", desc: "玉泉洞或百匯自助餐" },
                    { time: "13:30", title: "ASHIBINAA Outlet", desc: "名牌折扣購物城", link: "ASHIBINAA Outlet" },
                    { time: "15:30", title: "瀨長島營區", desc: "UMIKAJI Terrace 白色階梯海景", link: "瀨長島" },
                    { time: "17:30", title: "前往機場", desc: "準備搭機返台" },
                    { time: "19:30", title: "起飛 ✈️ CI133", desc: "返回高雄" },
                    { time: "20:25", title: "抵達高雄", desc: "甜蜜的家" }
                ]
            }
        ];

        const packingList = [
            { cat: "證件", items: ["護照 (效期>6個月)", "身分證/健保卡", "保險單影本"] },
            { cat: "電子", items: ["手機", "行動電源", "充電線", "漫遊/網卡"] },
            { cat: "衣物", items: ["輕便外套", "好走的鞋 (健行)", "雨具", "泳衣", "帽子/墨鏡"] },
            { cat: "生活", items: ["個人藥品", "防曬乳", "牙刷/牙膏 (日本環保旅館)", "環保袋"] },
            { cat: "錢包", items: ["日圓現金", "信用卡 (海外回饋高)"] }
        ];

        const guideData = [
            {
                title: "美麗海水族館",
                subtitle: "Churaumi Aquarium",
                desc: "擁有世界級的巨大水槽「黑潮之海」，可近距離觀賞鯨鯊與鬼蝠魟。別忘了海豚劇場的表演是戶外免費觀賞的！",
                map: "https://www.openstreetmap.org/export/embed.html?bbox=127.873,26.690,127.883,26.700&layer=mapnik"
            },
            {
                title: "玉泉洞 & 王國村",
                subtitle: "Gyokusendo Cave",
                desc: "歷經 30 萬年歲月形成的鐘乳石洞，全長 5 公里（目前開放約 890 公尺）。參觀完洞穴後，王國村的太鼓表演（EISA）震撼力十足，必看！",
                map: null
            },
            {
                title: "瀨長島",
                subtitle: "Umikaji Terrace",
                desc: "位於那霸機場南方的小島，純白建築依山而建，有「沖繩小希臘」之稱。這裡是看飛機起降與夕陽的絕佳地點，鬆餅店非常有名。",
                map: null
            }
        ];

        // --- APP STATE ---
        let currentTab = 'plan';
        let exchangeRate = 0.22; // Default TWD/JPY
        let expenses = JSON.parse(localStorage.getItem('ok_expenses')) || [];
        let checklistState = JSON.parse(localStorage.getItem('ok_checklist')) || {};
        let memoText = localStorage.getItem('ok_memo') || "";
        
        // --- ROUTING ---
        function router(tab) {
            currentTab = tab;
            const main = document.getElementById('main-content');
            
            // 隱藏所有頁面，並清空主內容
            main.innerHTML = '';
            
            // Update Nav UI
            document.querySelectorAll('.nav-item').forEach(el => {
                // 減少導航列按鈕寬度，以適應 5 個按鈕
                el.classList.remove('active', 'text-gray-800', 'w-1/6');
                el.classList.add('w-1/5'); 
            });
            const navElement = document.getElementById(`nav-${tab}`);
            if (navElement) {
                navElement.classList.add('active', 'text-gray-800');
            }
            
            window.scrollTo(0,0);

            // Render Content
            switch(tab) {
                case 'plan': renderPlan(main); break;
                case 'guide': renderGuide(main); break;
                case 'wallet': renderWallet(main); break;
                case 'lists': renderLists(main); break;
                case 'info': renderInfo(main); break;
                // 移除對 'map' 和 'help' 的處理
            }
        }

        // --- RENDER FUNCTIONS ---

        function renderPlan(container) {
            let html = `<div class="space-y-8 pb-4">`;
            itinerary.forEach((day, idx) => {
                html += `
                    <div class="relative pl-4 border-l-2 border-gray-200 ml-2">
                        <div class="mb-4">
                            <span class="bg-gray-800 text-white text-xs px-2 py-1 rounded tracking-wider">${day.date}</span>
                            <h2 class="text-xl font-bold mt-2 serif text-gray-800">${day.title}</h2>
                        </div>
                        <div class="space-y-4">
                `;
                day.activities.forEach(act => {
                    const highlightClass = act.highlight ? "border-l-4 border-yellow-400 table-yellow" : "";
                    const mapUrl = `https://www.google.com/maps/search/?api=1&query=$${encodeURIComponent(act.link)}`;
                    const linkBtn = act.link ? `<a href="${mapUrl}" target="_blank" class="text-gray-400 hover:text-blue-500 float-right"><i class="fas fa-map-marker-alt"></i></a>` : "";
                    
                    html += `
                        <div class="card p-4 ${highlightClass}">
                            <div class="flex justify-between items-start">
                                <div>
                                    <div class="text-sm font-bold text-gray-500 mb-1">${act.time}</div>
                                    <h3 class="font-bold text-gray-800 text-lg">${act.title}</h3>
                                    <p class="text-sm text-gray-600 mt-1">${act.desc}</p>
                                </div>
                                ${linkBtn}
                            </div>
                        </div>
                    `;
                });
                html += `</div></div>`;
            });
            html += `</div>`;
            container.innerHTML = html;
        }

        function renderGuide(container) {
            let html = `<div class="space-y-6">`;
            html += `<div class="text-center serif italic text-gray-400 mb-6">Explore the Deep Okinawa</div>`;
            
            guideData.forEach(item => {
                let mapEmbed = item.map ? `<iframe width="100%" height="200" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" src="${item.map}" class="rounded mt-4 border border-gray-200"></iframe>` : '';
                
                html += `
                    <div class="card p-0 overflow-hidden">
                        <div class="p-6">
                            <h2 class="text-2xl serif font-bold text-gray-800">${item.title}</h2>
                            <p class="text-xs tracking-widest text-gray-400 uppercase mb-4">${item.subtitle}</p>
                            <p class="text-gray-600 leading-relaxed text-justify text-sm">${item.desc}</p>
                            ${mapEmbed}
                        </div>
                    </div>
                `;
            });
            container.innerHTML = html;
        }

        function renderWallet(container) {
            // Calculate Total
            let totalYen = expenses.reduce((sum, item) => sum + item.amount, 0);
            let totalTwd = Math.round(totalYen * exchangeRate);

            let html = `
                <div class="space-y-6">
                    <div class="card p-4 bg-gray-800 text-white">
                        <div class="flex justify-between items-center mb-2">
                            <span class="text-xs text-gray-400">當前匯率 (TWD/JPY)</span>
                            <input type="number" id="rate-input" value="${exchangeRate}" step="0.001" class="bg-gray-700 text-white text-right w-16 text-xs p-1 rounded" onchange="updateRate(this.value)">
                        </div>
                        <div class="mb-4">
                            <label class="text-xs text-gray-400 block mb-1">簡易計算機 (輸入算式)</label>
                            <div class="flex gap-2">
                                <input type="text" id="calc-input" placeholder="輸入算式..." class="w-full bg-white text-gray-800 p-2 rounded text-lg font-mono">
                                <button onclick="runCalc()" class="bg-gray-600 px-4 rounded font-bold">=</button>
                            </div>
                            <div id="calc-result" class="text-right text-xl font-bold mt-2 h-6 text-yellow-400"></div>
                        </div>
                    </div>

                    <div class="card p-4 table-blue">
                        <h3 class="serif font-bold text-lg mb-3">新增支出</h3>
                        <div class="space-y-3">
                            <input type="text" id="exp-name" placeholder="品項名稱 (如: 拉麵)" class="w-full border-b border-gray-300 p-2 focus:outline-none focus:border-gray-500 bg-transparent">
                            <div class="flex gap-2">
                                <input type="number" id="exp-amount" placeholder="日幣金額" class="w-2/3 border-b border-gray-300 p-2 focus:outline-none focus:border-gray-500 bg-transparent">
                                <label class="w-1/3 flex items-center justify-center bg-gray-50 rounded cursor-pointer hover:bg-gray-200 transition text-gray-500">
                                    <i class="fas fa-camera mr-2"></i>
                                    <input type="file" id="exp-photo" accept="image/*" class="hidden" onchange="previewImage(this)">
                                    <span id="photo-label" class="text-xs">照片</span>
                                </label>
                            </div>
                            <button onclick="addExpense()" class="w-full btn-muji mt-2 bg-gray-600">記錄</button>
                        </div>
                    </div>

                    <div>
                        <div class="flex justify-between items-end mb-2 px-1">
                            <h3 class="serif font-bold text-lg">消費紀錄</h3>
                            <div class="text-right">
                                <div class="text-xs text-gray-500">總計</div>
                                <div class="font-bold">¥${totalYen.toLocaleString()} <span class="text-gray-400 text-xs">≈ NT$${totalTwd.toLocaleString()}</span></div>
                            </div>
                        </div>
                        <div id="expense-list" class="space-y-3">
                            ${expenses.map((item, idx) => `
                                <div class="card p-3 flex justify-between items-center ${idx % 2 === 0 ? 'table-yellow' : ''}">
                                    <div class="flex items-center gap-3">
                                        ${item.img ? `<div class="w-12 h-12 bg-gray-200 rounded bg-cover bg-center flex-shrink-0" style="background-image:url('${item.img}')" onclick="showImage('${item.img}')"></div>` : `<div class="w-12 h-12 bg-gray-100 rounded flex items-center justify-center text-gray-300 flex-shrink-0"><i class="fas fa-shopping-bag"></i></div>`}
                                        <div>
                                            <div class="font-bold text-gray-800">${item.name}</div>
                                            <div class="text-xs text-gray-500">${new Date(item.date).toLocaleDateString()}</div>
                                        </div>
                                    </div>
                                    <div class="text-right">
                                        <div class="font-bold">¥${item.amount}</div>
                                        <div class="text-xs text-gray-400">≈ $${Math.round(item.amount * exchangeRate)}</div>
                                        <button onclick="removeExpense(${idx})" class="text-red-300 text-xs mt-1 hover:text-red-500">刪除</button>
                                    </div>
                                </div>
                            `).join('')}
                            ${expenses.length === 0 ? '<div class="text-center text-gray-300 py-8">暫無紀錄</div>' : ''}
                        </div>
                    </div>
                </div>
            `;
            container.innerHTML = html;
        }

        function renderLists(container) {
            let html = `
                <div class="space-y-6">
                    <div class="card p-5 table-blue">
                        <h3 class="serif font-bold text-lg mb-4 border-b pb-2">行前清單</h3>
                        <div class="space-y-4">
            `;
            
            packingList.forEach((cat, catIdx) => {
                html += `<div><h4 class="text-xs font-bold text-gray-600 mb-2">${cat.cat}</h4><div class="space-y-2">`;
                cat.items.forEach((item, itemIdx) => {
                    const key = `check_${catIdx}_${itemIdx}`;
                    const checked = checklistState[key] ? 'checked' : '';
                    html += `
                        <label class="flex items-center gap-3 cursor-pointer">
                            <input type="checkbox" class="custom-checkbox w-5 h-5 rounded border-gray-300 text-gray-600 focus:ring-gray-500" ${checked} onchange="toggleCheck('${key}')">
                            <div class="text-sm text-gray-700 transition">${item}</div>
                        </label>
                    `;
                });
                html += `</div></div>`;
            });
            
            html += `
                        </div>
                    </div>

                    <div class="card p-5 table-yellow">
                        <h3 class="serif font-bold text-lg mb-2">備忘錄</h3>
                        <p class="text-xs text-gray-500 mb-2">輸入網址會自動轉換為連結</p>
                        <textarea id="memo-area" class="w-full h-32 p-2 bg-white border border-gray-200 rounded text-sm focus:outline-none focus:border-gray-400" placeholder="寫點什麼..." onblur="saveMemo(this.value)">${memoText}</textarea>
                        <div id="memo-links" class="mt-3 flex flex-wrap gap-2"></div>
                    </div>
                </div>
            `;
            container.innerHTML = html;
            parseMemoLinks(memoText);
        }

        function renderInfo(container) {
            container.innerHTML = `
                <div class="space-y-4">
                    <div class="card p-5 border-l-4 border-red-400 bg-red-50">
                        <h3 class="font-bold text-lg mb-2 text-red-800">緊急聯絡</h3>
                        <ul class="space-y-2 text-sm text-red-700">
                            <li class="flex justify-between"><span>👮 警察</span> <span class="font-mono font-bold">110</span></li>
                            <li class="flex justify-between"><span>🚑 救護/火警</span> <span class="font-mono font-bold">119</span></li>
                            <li class="mt-2 pt-2 border-t border-red-200 text-gray-700">
                                台北駐日經濟文化代表處<br>那霸分處：<a href="tel:098-862-7008" class="text-blue-500">098-862-7008</a>
                            </li>
                        </ul>
                    </div>

                    <div class="card p-5 table-blue">
                        <h3 class="font-bold text-lg mb-2">實用連結</h3>
                        <div class="grid grid-cols-2 gap-3">
                            <a href="https://www.jma.go.jp/bosai/forecast/" target="_blank" class="btn-outline text-center text-sm bg-white hover:bg-gray-100">日本氣象廳</a>
                            <a href="https://www.google.com/maps" target="_blank" class="btn-outline text-center text-sm bg-white hover:bg-gray-100">Google Maps</a>
                        </div>
                    </div>

                    <div class="card p-5 table-yellow">
                        <h3 class="font-bold text-lg mb-2">注意事項</h3>
                        <ul class="list-disc pl-5 text-sm text-gray-700 space-y-1">
                            <li>日本電壓 100V，插座與台灣相同（雙孔扁插）。</li>
                            <li>日本駕駛座在右邊，過馬路請先看右再看左。</li>
                            <li>垃圾請分類，路上垃圾桶較少，建議自備塑膠袋帶回飯店。</li>
                            <li>室內大多禁菸，請至指定吸菸區。</li>
                        </ul>
                    </div>
                </div>
            `;
        }


        // --- WALLET LOGIC ---
        let tempImgBase64 = null;

        function updateRate(val) {
            exchangeRate = parseFloat(val);
            localStorage.setItem('ok_rate', exchangeRate);
            renderWallet(document.getElementById('main-content'));
        }

        function runCalc() {
            const input = document.getElementById('calc-input').value;
            const display = document.getElementById('calc-result');
            try {
                if (!/^[0-9+\-*/().\s]+$/.test(input)) throw new Error("Invalid Char");
                const res = Function('"use strict";return (' + input + ')')();
                display.innerText = `¥ ${Math.round(res).toLocaleString()} (≈$${Math.round(res*exchangeRate)})`;
            } catch (e) {
                display.innerText = "Error";
            }
        }

        function previewImage(input) {
            if (input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const img = new Image();
                    img.src = e.target.result;
                    img.onload = function() {
                        const canvas = document.createElement('canvas');
                        const MAX_WIDTH = 300;
                        const scaleSize = MAX_WIDTH / img.width;
                        canvas.width = MAX_WIDTH;
                        canvas.height = img.height * scaleSize;
                        const ctx = canvas.getContext('2d');
                        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                        tempImgBase64 = canvas.toDataURL('image/jpeg', 0.6);
                        document.getElementById('photo-label').innerText = "已選取";
                        document.getElementById('photo-label').classList.add("text-blue-500", "font-bold");
                    }
                }
                reader.readAsDataURL(input.files[0]);
            }
        }

        function addExpense() {
            const name = document.getElementById('exp-name').value;
            const amount = parseFloat(document.getElementById('exp-amount').value);
            
            if (!name || isNaN(amount)) return alert("請輸入名稱與金額");

            const newExp = {
                name,
                amount,
                date: new Date().toISOString(),
                img: tempImgBase64
            };

            try {
                expenses.unshift(newExp);
                localStorage.setItem('ok_expenses', JSON.stringify(expenses));
                tempImgBase64 = null; // Reset
                
                // Clear inputs
                document.getElementById('exp-name').value = '';
                document.getElementById('exp-amount').value = '';
                document.getElementById('exp-photo').value = '';
                document.getElementById('photo-label').innerText = "照片";
                document.getElementById('photo-label').classList.remove("text-blue-500", "font-bold");

                renderWallet(document.getElementById('main-content'));
            } catch (e) {
                alert("儲存空間已滿，請刪除舊紀錄或不附照片。");
                expenses.shift(); // Revert
            }
        }

        function removeExpense(idx) {
            if(confirm("確定刪除此筆紀錄？")) {
                expenses.splice(idx, 1);
                localStorage.setItem('ok_expenses', JSON.stringify(expenses));
                renderWallet(document.getElementById('main-content'));
            }
        }
        
        function showImage(src) {
            const overlay = document.createElement('div');
            overlay.className = "fixed inset-0 bg-black bg-opacity-90 z-50 flex items-center justify-center p-4";
            overlay.onclick = () => document.body.removeChild(overlay);
            overlay.innerHTML = `<img src="${src}" class="max-w-full max-h-full rounded border-2 border-white">`;
            document.body.appendChild(overlay);
        }

        // --- LIST LOGIC ---
        function toggleCheck(key) {
            checklistState[key] = !checklistState[key];
            localStorage.setItem('ok_checklist', JSON.stringify(checklistState));
            renderLists(document.getElementById('main-content'));
        }

        function saveMemo(txt) {
            memoText = txt;
            localStorage.setItem('ok_memo', txt);
            parseMemoLinks(txt);
        }

        function parseMemoLinks(txt) {
            const container = document.getElementById('memo-links');
            if(!container) return;
            container.innerHTML = '';
            const urlRegex = /(https?:\/\/[^\s]+)/g;
            const matches = txt.match(urlRegex);
            if (matches) {
                matches.forEach(url => {
                    const a = document.createElement('a');
                    a.href = url;
                    a.target = "_blank";
                    a.className = "text-xs bg-blue-100 text-blue-600 px-2 py-1 rounded hover:bg-blue-200 truncate max-w-full block";
                    let displayUrl = url.replace(/^https?:\/\/(www\.)?/, '').split('/')[0];
                    a.innerText = `🔗 ${displayUrl}`;
                    container.appendChild(a);
                });
            }
        }
        
        // --- INIT ---
        window.addEventListener('load', () => {
            // 從 LocalStorage 載入匯率
            const savedRate = localStorage.getItem('ok_rate');
            if (savedRate) {
                exchangeRate = parseFloat(savedRate);
            }

            router('plan'); // Start at Plan
        });

    </script>
</body>
</html>
