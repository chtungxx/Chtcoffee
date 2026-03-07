<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <meta name="theme-color" content="#8B4513">
    <meta name="description" content="咖啡沖煮記錄應用 - 記錄每一杯咖啡的故事">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Today's Coffee">
    <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 192 192'><rect fill='%238B4513' width='192' height='192'/><text x='50%' y='50%' font-size='80' fill='%23fff' text-anchor='middle' dominant-baseline='central'>☕</text></svg>">
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 180 180'><rect fill='%238B4513' width='180' height='180' rx='40'/><text x='90' y='90' font-size='80' fill='%23fff' text-anchor='middle' dominant-baseline='central'>☕</text></svg>">
    <title>오늘커피 ☕️ Today’s Coffee</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Arial', sans-serif;
            background-image: url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?ixlib=rb-4.0.3&auto=format&fit=crop&w=2070&q=80');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            opacity: 0.9;
            min-height: 100vh;
            color: #333;
        }
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.6);
            z-index: -1;
        }
        header {
            text-align: center;
            padding: 20px;
            background: rgba(255, 255, 255, 0.9);
            margin-bottom: 20px;
        }
        h1 {
            font-size: 2.5em;
            color: #6f4e37;
        }
        nav {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 20px;
        }
        nav button {
            padding: 10px 20px;
            background: #8B4513;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
        }
        nav button.active, nav button:hover {
            background: #A0522D;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 40px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #555;
        }
        .optional-badge {
            font-size: 0.8em;
            color: #888;
            font-weight: normal;
        }
        input, select, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1em;
        }
        textarea {
            height: 120px;
            resize: vertical;
        }
        button.submit {
            background: #6f4e37;
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1.1em;
            width: 100%;
            margin-top: 10px;
        }
        button.submit:hover {
            background: #5c402d;
        }
        .record {
            border: 1px solid #ddd;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            background: white;
        }
        .record img {
            max-width: 100%;
            max-height: 300px;
            object-fit: contain;
            border-radius: 5px;
            margin-bottom: 10px;
        }
        .record-meta p {
            margin-bottom: 5px;
            line-height: 1.4;
        }
        .month-view {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
        }
        .thumbnail {
            text-align: center;
            background: white;
            padding: 10px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }
        .thumbnail img {
            width: 100%;
            height: 150px;
            object-fit: cover;
            border-radius: 5px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        .thumbnail img:hover {
            transform: scale(1.05);
        }
        .thumbnail p {
            margin-top: 8px;
            font-weight: bold;
            color: #6f4e37;
        }
        .guide-box {
            background: #fff8f0;
            border: 1px solid #e2d1c3;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 20px;
        }
        .guide-box h3 {
            color: #6f4e37;
            margin-bottom: 10px;
            border-bottom: 2px solid #e2d1c3;
            padding-bottom: 5px;
        }
        .steps {
            list-style-position: inside;
            margin-top: 10px;
            line-height: 1.6;
        }
        .steps li {
            margin-bottom: 8px;
        }
    </style>
</head>
<body>
    <header>
        <h1>오늘커피 ☕️ Today’s Coffee</h1>
    </header>
    <nav>
        <button onclick="showSection('record')" class="active">咖啡記錄</button>
        <button onclick="showSection('view')">查看記錄</button>
        <button onclick="showSection('methods')">沖煮指南</button>
    </nav>
    <div class="container">
        
        <!-- ================= 1. 記錄區 ================= -->
        <section id="record-section">
            <h2 style="margin-bottom: 20px; color: #6f4e37; text-align: center;">紀錄今天的味道</h2>
            
            <!-- 必填：相片與日期 -->
            <div class="form-group">
                <label>上傳相片 <span style="color:red">*</span></label>
                <input type="file" id="photo" accept="image/*">
                <img id="preview" style="max-width: 100%; max-height: 300px; margin-top: 10px; display: none; border-radius: 8px;">
            </div>
            <div class="form-group">
                <label>選擇日期 <span style="color:red">*</span></label>
                <input type="date" id="record-date">
            </div>

            <hr style="border: 0; border-top: 1px dashed #ccc; margin: 20px 0;">

            <!-- 選填：沖煮參數 -->
            <div class="form-group">
                <label>沖煮時間 <span class="optional-badge">(選填)</span></label>
                <input type="text" id="time" placeholder="例如: 2:30">
            </div>
            <div class="form-group">
                <label>沖煮方式 <span class="optional-badge">(選填)</span></label>
                <select id="method">
                    <option value="">請選擇</option>
                    <option>手沖 (V60)</option>
                    <option>手沖 (Origami / Kalita)</option>
                    <option>義式濃縮 (Espresso)</option>
                    <option>美式咖啡 (Americano)</option>
                    <option>拿鐵 (Latte)</option>
                    <option>冷萃 / 浸泡 (Cold Brew)</option>
                    <option>其他</option>
                </select>
            </div>
            <div class="form-group">
                <label>咖啡豆烘焙度 <span class="optional-badge">(選填)</span></label>
                <select id="roast">
                    <option value="">請選擇</option>
                    <option>極淺焙</option>
                    <option>淺焙</option>
                    <option>中焙</option>
                    <option>深焙</option>
                </select>
            </div>
            <div class="form-group">
                <label>咖啡豆產區或名稱 <span class="optional-badge">(選填)</span></label>
                <input type="text" id="bean" placeholder="例如: 衣索比亞 耶加雪菲">
            </div>

            <!-- 自由打字區 -->
            <div class="form-group">
                <label>心得筆記 <span class="optional-badge">(Feel free 自由填寫)</span></label>
                <textarea id="notes" placeholder="今天這杯咖啡喝起來感覺如何呢？留下你的感受吧..."></textarea>
            </div>

            <button class="submit" onclick="saveRecord()">儲存記錄</button>
        </section>


        <!-- ================= 2. 沖煮方法指南區 ================= -->
        <section id="methods-section" style="display: none;">
            <h2 style="margin-bottom: 20px; color: #6f4e37; text-align: center;">常用沖煮指南</h2>
            
            <div class="guide-box">
                <h3>☕ 手沖咖啡 (Pour-over)</h3>
                <p><strong>推薦粉水比例:</strong> 1:15 (例：20g 咖啡粉對 300ml 水)</p>
                <p><strong>建議水溫:</strong> 90°C - 92°C (淺焙可略高，深焙可略低)</p>
                <ol class="steps">
                    <li><strong>悶蒸 (Bloom):</strong> 注入約 2 倍咖啡粉重量的水 (約 40ml)，等待 30 秒，讓咖啡粉排出二氧化碳。</li>
                    <li><strong>第一段注水:</strong> 由中心向外穩定繞圈注水至 120ml，這段主要萃取咖啡的酸甜風味與香氣。</li>
                    <li><strong>第二段注水:</strong> 等待水位略降，繼續繞圈注水至 200ml，這段決定咖啡的醇厚度 (Body)。</li>
                    <li><strong>最後補水:</strong> 最後將水注滿至 300ml，等待水流乾。總沖煮時間約為 2分30秒 至 3分鐘。</li>
                </ol>
            </div>
            
            <div class="guide-box">
                <h3>🔥 義式濃縮 (Espresso)</h3>
                <p><strong>推薦粉水比例:</strong> 1:2 (例：18g 咖啡粉萃取 36g 咖啡液)</p>
                <p><strong>建議水溫:</strong> 92°C - 94°C</p>
                <ol class="steps">
                    <li><strong>佈粉與壓粉:</strong> 將研磨好的咖啡粉均勻分佈在濾杯中，並使用壓粉器垂直向下施加均勻的力道壓平。</li>
                    <li><strong>預浸泡:</strong> (若機器支援) 進行 3-5 秒的低壓預浸，讓粉餅均勻吸水。</li>
                    <li><strong>正式萃取:</strong> 啟動萃取，觀察咖啡液應呈現「老鼠尾巴」般細長且連續的流動。</li>
                    <li><strong>時間控制:</strong> 理想的萃取時間約落在 25秒 至 30秒 之間。</li>
                    <li><strong>完成:</strong> 觀察表面是否有一層金黃色的 Crema (咖啡脂)，即可享受濃郁的義式濃縮！</li>
                </ol>
            </div>
        </section>


        <!-- ================= 3. 查看記錄區 ================= -->
        <section id="view-section" style="display: none;">
            <h2 style="margin-bottom: 20px; color: #6f4e37; text-align: center;">歷史記錄</h2>
            
            <div class="form-group">
                <label>選擇月份檢視縮圖:</label>
                <select id="month-select" onchange="showMonth()">
                    <option value="">選擇月份</option>
                </select>
            </div>
            
            <!-- 月份縮圖區 -->
            <div id="month-view" class="month-view"></div>
            
            <hr style="border: 0; border-top: 1px solid #ddd; margin: 30px 0;">

            <h3 style="margin-bottom: 15px; color: #6f4e37;">詳細記錄列表</h3>
            <!-- 單筆詳細記錄區 -->
            <div id="single-records"></div>
        </section>

    </div>

    <!-- JavaScript 邏輯 -->
    <script>
        // 初始化讀取 LocalStorage
        let records = JSON.parse(localStorage.getItem('coffeeRecords_v4')) || [];
        
        // 設定今日日期預設值
        const today = new Date().toISOString().split('T')[0];
        document.getElementById('record-date').value = today;
        
        // 監聽照片上傳並預覽
        document.getElementById('photo').addEventListener('change', previewPhoto);
        
        // 初始更新 UI
        updateRecords();

        function previewPhoto(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (ev) => {
                    document.getElementById('preview').src = ev.target.result;
                    document.getElementById('preview').style.display = 'block';
                };
                reader.readAsDataURL(file);
            }
        }

        function saveRecord() {
            const photoFile = document.getElementById('photo').files[0];
            const recordDate = document.getElementById('record-date').value;
            
            // 選填欄位
            const time = document.getElementById('time').value;
            const method = document.getElementById('method').value;
            const roast = document.getElementById('roast').value;
            const bean = document.getElementById('bean').value;
            const notes = document.getElementById('notes').value;

            // 必填檢查
            if (!photoFile || !recordDate) {
                alert('請至少上傳一張相片並確認日期！');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const record = {
                    id: Date.now(),
                    date: recordDate,
                    photo: e.target.result,
                    time: time,
                    method: method,
                    roast: roast,
                    bean: bean,
                    notes: notes
                };
                // 新記錄放在最前面
                records.unshift(record);
                localStorage.setItem('coffeeRecords_v4', JSON.stringify(records));
                
                alert('今日咖啡記錄已儲存！☕️');
                clearForm();
                showSection('view');
                updateRecords();
            };
            reader.readAsDataURL(photoFile);
        }

        function clearForm() {
            document.getElementById('photo').value = '';
            document.getElementById('record-date').value = today;
            document.getElementById('time').value = '';
            document.getElementById('method').value = '';
            document.getElementById('roast').value = '';
            document.getElementById('bean').value = '';
            document.getElementById('notes').value = '';
            document.getElementById('preview').style.display = 'none';
            document.getElementById('preview').src = '';
        }

        function showSection(section) {
            document.querySelectorAll('section').forEach(s => s.style.display = 'none');
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
            
            const btn = document.querySelector(`nav button[onclick="showSection('${section}')"]`);
            if (btn) btn.classList.add('active');
            
            if (section === 'record') {
                document.getElementById('record-section').style.display = 'block';
            } else if (section === 'view') {
                document.getElementById('view-section').style.display = 'block';
                updateRecords();
            } else if (section === 'methods') {
                document.getElementById('methods-section').style.display = 'block';
            }
        }

        function updateRecords() {
            const singleDiv = document.getElementById('single-records');
            
            if (records.length === 0) {
                singleDiv.innerHTML = '<p style="color: #999; text-align: center; padding: 20px;">暫沒有記錄，趕快開始紀錄你的第一杯咖啡故事吧！</p>';
            } else {
                singleDiv.innerHTML = records.map(r => `
                    <div class="record">
                        <img src="${r.photo}" alt="咖啡相片">
                        <div class="record-meta">
                            <p><strong>📆 日期:</strong> ${r.date}</p>
                            ${r.time ? `<p><strong>⏱ 時間:</strong> ${r.time}</p>` : ''}
                            ${r.method ? `<p><strong>☕️ 方式:</strong> ${r.method}</p>` : ''}
                            ${r.roast ? `<p><strong>🔥 烘焙:</strong> ${r.roast}</p>` : ''}
                            ${r.bean ? `<p><strong>🌱 產地/名稱:</strong> ${r.bean}</p>` : ''}
                            ${r.notes ? `<p style="margin-top:10px; padding:10px; background:#f9f9f9; border-radius:5px;"><strong>📝 心得:</strong><br>${r.notes.replace(/\n/g, '<br>')}</p>` : ''}
                            
                            <button onclick="deleteRecord(${r.id})" style="background: #e74c3c; color: white; border: none; border-radius: 4px; padding: 6px 12px; margin-top: 15px; cursor: pointer; font-size: 0.9em;">
                                刪除此記錄
                            </button>
                        </div>
                    </div>
                `).join('');
            }

            // 更新月份下拉選單
            const months = [...new Set(records.map(r => r.date.slice(0,7)))].sort().reverse();
            const select = document.getElementById('month-select');
            
            // 記住當前選中的月份（如果有的話）
            const currentSelected = select.value;
            
            select.innerHTML = '<option value="">選擇月份檢視縮圖</option>' + months.map(m => `<option value="${m}">${m}</option>`).join('');
            
            if (currentSelected && months.includes(currentSelected)) {
                select.value = currentSelected;
            } else if (months.length > 0) {
                // 預設顯示最新月份
                select.value = months[0];
            }
            
            // 觸發顯示縮圖
            showMonth();
        }

        function showMonth() {
            const month = document.getElementById('month-select').value;
            const monthDiv = document.getElementById('month-view');
            
            if (!month) {
                monthDiv.innerHTML = '';
                return;
            }
            
            const monthRecords = records.filter(r => r.date.startsWith(month));
            if (monthRecords.length === 0) {
                monthDiv.innerHTML = '<p style="color: #999;">該月份暫無記錄</p>';
            } else {
                monthDiv.innerHTML = monthRecords.map(r => `
                    <div class="thumbnail">
                        <img src="${r.photo}" onclick="viewDetail(${r.id})" title="點擊查看詳細">
                        <p>${r.date.slice(8)} 日</p>
                    </div>
                `).join('');
            }
        }

        // 點擊縮圖時彈出提示框顯示詳情
        function viewDetail(id) {
            const r = records.find(r => r.id === id);
            if (r) {
                let msg = `【 ${r.date} 的咖啡紀錄 】\n\n`;
                if(r.time) msg += `⏱ 沖煮時間: ${r.time}\n`;
                if(r.method) msg += `☕️ 沖煮方式: ${r.method}\n`;
                if(r.roast) msg += `🔥 烘焙度: ${r.roast}\n`;
                if(r.bean) msg += `🌱 產區名稱: ${r.bean}\n`;
                if(r.notes) msg += `\n📝 心得筆記:\n${r.notes}`;
                
                alert(msg);
            }
        }

        function deleteRecord(id) {
            if (confirm('確定要刪除這筆記錄嗎？(刪除後無法恢復)')) {
                records = records.filter(r => r.id !== id);
                localStorage.setItem('coffeeRecords_v4', JSON.stringify(records));
                updateRecords(); // 這會連帶觸發 showMonth()
                alert('記錄已刪除！');
            }
        }
    </script>
</body>
</html>
