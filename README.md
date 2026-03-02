[index (2).html](https://github.com/user-attachments/files/25678255/index.2.html)
<html lang="zh-HKT">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
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
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input, select, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1em;
        }
        textarea {
            height: 100px;
            resize: vertical;
        }
        button.submit {
            background: #4CAF50;
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1.1em;
            width: 100%;
        }
        button.submit:hover {
            background: #45a049;
        }
        #records {
            display: none;
        }
        .record {
            border: 1px solid #ddd;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            background: white;
        }
        .record img {
            max-width: 200px;
            height: auto;
            border-radius: 5px;
        }
        .record-meta {
            margin-top: 10px;
        }
        .month-view {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 10px;
        }
        .thumbnail {
            text-align: center;
        }
        .thumbnail img {
            width: 100%;
            height: 150px;
            object-fit: cover;
            border-radius: 5px;
            cursor: pointer;
        }
        .guide {
            display: none;
        }
        .steps {
            list-style-position: inside;
        }
        #date-display {
            font-size: 1.2em;
            color: #6f4e37;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <header>
        <h1>오늘커피 ☕️ Today’s Coffee</h1>
    </header>
    <nav>
        <button onclick="showSection('record')">咖啡記錄</button>
        <button onclick="showSection('view')">查看記錄</button>
        <button onclick="showSection('methods')">沖煮方法步驟</button>
    </nav>
    <div class="container">
        <!-- 記錄區 -->
        <section id="record-section">
            <h2>今日咖啡記錄</h2>
            <div class="form-group">
                <label>上傳相片:</label>
                <input type="file" id="photo" accept="image/*">
                <img id="preview" style="max-width: 300px; margin-top: 10px; display: none;">
            </div>
            <div class="form-group">
                <label>選擇日期:</label>
                <input type="date" id="record-date">
            </div>
            <div class="form-group">
                <label>沖煮時間 (分鐘):</label>
                <input type="number" id="time" min="1" max="10">
            </div>
            <div class="form-group">
                <label>水溫 (°C):</label>
                <input type="number" id="temperature" placeholder="例: 90">
            </div>
            <div class="form-group">
                <label>水粉比例:</label>
                <input type="text" id="ratio" placeholder="例: 1:15 或 20g:300ml">
            </div>
            <div class="form-group">
                <label>沖煮方式:</label>
                <select id="method">
                    <option>手沖 (V60)</option>
                    <option>手沖 (Chemex)</option>
                    <option>手沖 (Kalita Wave)</option>
                    <option>意式咖啡 (Espresso)</option>
                    <option>French Press</option>
                    <option>Cold Brew</option>
                    <option>Moka Pot</option>
                    <option>Turkish Coffee</option>
                    <option>其他</option>
                </select>
            </div>
            <div class="form-group">
                <label>咖啡豆烘焙度:</label>
                <select id="roast">
                    <option>淺焙</option>
                    <option>中焙</option>
                    <option>深焙</option>
                </select>
            </div>
            <div class="form-group">
                <label>咖啡豆產區/名稱:</label>
                <input type="text" id="bean">
            </div>
            <div class="form-group">
                <label>感官風味:</label>
                <div style="display: flex; gap: 15px; margin-top: 8px;">
                    <label style="display: flex; align-items: center; font-weight: normal; margin: 0;">
                        <input type="checkbox" id="flavor-floral" style="width: auto; margin-right: 5px;">
                        花香
                    </label>
                    <label style="display: flex; align-items: center; font-weight: normal; margin: 0;">
                        <input type="checkbox" id="flavor-fruity" style="width: auto; margin-right: 5px;">
                        果酸
                    </label>
                    <label style="display: flex; align-items: center; font-weight: normal; margin: 0;">
                        <input type="checkbox" id="flavor-nutty" style="width: auto; margin-right: 5px;">
                        堅果
                    </label>
                </div>
            </div>
            <div class="form-group">
                <label>心得:</label>
                <textarea id="notes" placeholder="分享你的沖煮心得..."></textarea>
            </div>
            <button class="submit" onclick="saveRecord()">儲存記錄</button>
        </section>

        <!-- 沖煮方法步驟區 -->
        <section id="methods-section" style="display: none;">
            <h2>沖煮方法步驟</h2>
            
            <h3>一刀流沖煮法</h3>
            <p><strong>推薦比例:</strong> 1:15 至 1:17 (咖啡:水)</p>
            <p><strong>水溫:</strong> 90-92°C</p>
            <ol class="steps">
                <li>將咖啡粉放入濾杯，倒入濾紙。</li>
                <li>進行悶蒸 (Bloom)：倒入 2x 咖啡重量的水，靜置 40-45 秒。</li>
                <li>開始注水：以穩定的流速，一次性持續注水至達到目標總水量。</li>
                <li>保持穩定的注水流速，不中斷，直到所有水都通過咖啡粉。</li>
                <li>總萃取時間約 2.5-3 分鐘。</li>
                <li>享受您的咖啡！</li>
            </ol>
            
            <h3>四六沖法</h3>
            <p><strong>推薦比例:</strong> 1:16 (咖啡:水)</p>
            <p><strong>水溫:</strong> 92-94°C</p>
            <p><strong>說明:</strong> 分為四個階段，第一階段 (40%)、第二階段 (30%)、第三階段 (20%)、第四階段 (10%) 的注水方式</p>
            <ol class="steps">
                <li>進行悶蒸：倒入 2x 咖啡重量的水，靜置 45-50 秒。</li>
                <li><strong>第一階段 (40%):</strong> 注入 40% 的總水量，用小圓圈注水方式，時間約 50 秒。</li>
                <li>等待 30 秒，讓咖啡粉充分浸泡。</li>
                <li><strong>第二階段 (30%):</strong> 注入 30% 的總水量，繼續用小圓圈注水，時間約 40 秒。</li>
                <li>等待 20 秒。</li>
                <li><strong>第三階段 (20%):</strong> 注入 20% 的總水量，時間約 30 秒。</li>
                <li>等待 15 秒。</li>
                <li><strong>第四階段 (10%):</strong> 注入最後 10% 的水量，完成萃取。</li>
                <li>總萃取時間約 3-3.5 分鐘。</li>
                <li>享受層次豐富的咖啡風味！</li>
            </ol>
        </section>

        <!-- 查看記錄區 -->
        <section id="view-section" style="display: none;">
            <h2>查看記錄</h2>
            <div class="form-group">
                <label>選擇月份查看:</label>
                <select id="month-select" onchange="showMonth()">
                    <option value="">選擇月份</option>
                </select>
            </div>
            <div id="month-view" class="month-view"></div>
            
            <h3 style="margin-top: 30px;">所有記錄</h3>
            <div id="single-records"></div>
        </section>

        <!-- 指南區 -->
        <section id="guide-section" style="display: none;">
        </section>
    </div>

    <script>
        let records = JSON.parse(localStorage.getItem('coffeeRecords')) || [];
        const today = new Date().toISOString().split('T')[0];
        document.getElementById('record-date').value = today;
        document.getElementById('photo').addEventListener('change', previewPhoto);

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
            const time = document.getElementById('time').value;
            const temperature = document.getElementById('temperature').value;
            const ratio = document.getElementById('ratio').value;
            const method = document.getElementById('method').value;
            const roast = document.getElementById('roast').value;
            const bean = document.getElementById('bean').value;
            const flavors = [];
            if (document.getElementById('flavor-floral').checked) flavors.push('花香');
            if (document.getElementById('flavor-fruity').checked) flavors.push('果酸');
            if (document.getElementById('flavor-nutty').checked) flavors.push('堅果');
            const notes = document.getElementById('notes').value;

            if (!photoFile || !recordDate || !notes) {
                alert('請上傳相片、選擇日期並填寫心得！');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const record = {
                    id: Date.now(),
                    date: recordDate,
                    photo: e.target.result,
                    time,
                    temperature,
                    ratio,
                    method,
                    roast,
                    bean,
                    flavors,
                    notes
                };
                records.unshift(record);
                localStorage.setItem('coffeeRecords', JSON.stringify(records));
                alert('記錄已儲存！');
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
            document.getElementById('temperature').value = '';
            document.getElementById('ratio').value = '';
            document.getElementById('bean').value = '';
            document.getElementById('flavor-floral').checked = false;
            document.getElementById('flavor-fruity').checked = false;
            document.getElementById('flavor-nutty').checked = false;
            document.getElementById('notes').value = '';
            document.getElementById('preview').style.display = 'none';
        }

        function showSection(section) {
            document.querySelectorAll('section').forEach(s => s.style.display = 'none');
            document.querySelectorAll('button').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
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
                singleDiv.innerHTML = '<p style="color: #999;">暫沒有記錄，開始紀錄你的咖啡故事吧！</p>';
            } else {
                singleDiv.innerHTML = records.map(r => `
                    <div class="record">
                        <img src="${r.photo}" alt="咖啡相片">
                        <div class="record-meta">
                            <p><strong>日期:</strong> ${r.date}</p>
                            <p><strong>時間:</strong> ${r.time} 分</p>
                            <p><strong>水溫:</strong> ${r.temperature ? r.temperature + '°C' : '未記錄'}</p>
                            <p><strong>水粉比例:</strong> ${r.ratio || '未記錄'}</p>
                            <p><strong>方式:</strong> ${r.method}</p>
                            <p><strong>烘焙:</strong> ${r.roast}</p>
                            <p><strong>豆子:</strong> ${r.bean}</p>
                            <p><strong>感官風味:</strong> ${r.flavors && r.flavors.length > 0 ? r.flavors.join('、') : '未選擇'}</p>
                            <p><strong>心得:</strong> ${r.notes}</p>
                            <button onclick="deleteRecord(${r.id})" style="background: #e74c3c; padding: 5px 10px; margin-top: 10px;">刪除</button>
                        </div>
                    </div>
                `).join('');
            }

            const months = [...new Set(records.map(r => r.date.slice(0,7)))].sort().reverse();
            const select = document.getElementById('month-select');
            select.innerHTML = '<option value="">選擇月份</option>' + months.map(m => `<option value="${m}">${m}</option>`).join('');
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
                        <img src="${r.photo}" onclick="viewDetail(${r.id})" title="${r.notes.substring(0,50)}...">
                        <p>${r.date.slice(8)} 日</p>
                    </div>
                `).join('');
            }
        }

        function viewDetail(id) {
            const record = records.find(r => r.id === id);
            if (record) {
                const flavorsText = record.flavors && record.flavors.length > 0 ? record.flavors.join('、') : '未選擇';
                alert(`日期: ${record.date}\n時間: ${record.time} 分鐘\n水溫: ${record.temperature ? record.temperature + '°C' : '未記錄'}\n水粉比例: ${record.ratio || '未記錄'}\n方式: ${record.method}\n烘焙: ${record.roast}\n豆子: ${record.bean}\n感官風味: ${flavorsText}\n心得: ${record.notes}`);
            }
        }

        function deleteRecord(id) {
            if (confirm('確定要刪除這筆記錄嗎？')) {
                records = records.filter(r => r.id !== id);
                localStorage.setItem('coffeeRecords', JSON.stringify(records));
                updateRecords();
                showMonth();
                alert('記錄已刪除！');
            }
        }
    </script>
</body>
</html>
