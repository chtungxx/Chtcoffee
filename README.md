<!DOCTYPE html>
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
        <button onclick="showSection('guide')">手沖指南</button>
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
                <label>今日日期:</label>
                <div id="date-display"></div>
            </div>
            <div class="form-group">
                <label>沖煮時間 (分鐘):</label>
                <input type="number" id="time" min="1" max="10">
            </div>
            <div class="form-group">
                <label>沖煮方式:</label>
                <select id="method">
                    <option>手沖 (V60)</option>
                    <option>手沖 (Chemex)</option>
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
                <label>心得:</label>
                <textarea id="notes" placeholder="分享你的沖煮心得..."></textarea>
            </div>
            <button class="submit" onclick="saveRecord()">儲存記錄</button>
        </section>

        <!-- 查看記錄區 -->
        <section id="view-section" style="display: none;">
            <h2>個人記錄</h2>
            <div id="single-records"></div>
            <h3>月曆檢視</h3>
            <select id="month-select" onchange="showMonth()">
                <option value="">選擇月份</option>
            </select>
            <div id="month-view" class="month-view"></div>
        </section>

        <!-- 指南區 -->
        <section id="guide-section" style="display: none;">
            <h2>手沖咖啡指南</h2>
            <p>推薦比例: 1:17 (咖啡:水，例如 20g 咖啡 + 340g 水) [web:11][page:0]</p>
            <h3>V60 手沖步驟 [page:1]</h3>
            <ol class="steps">
                <li>水溫約 93°C (200°F)。</li>
                <li>咖啡磨成海鹽粗細，放入濾紙。</li>
                <li>Bloom: 倒 2x 咖啡重量的水 (e.g. 40g)，等 40 秒。</li>
                <li>分次注水: 總水 350g，分 3-4 次螺旋注水，總時間 2-3 分鐘。</li>
                <li>完成後享用！</li>
            </ol>
            <h3>比例建議</h3>
            <ul>
                <li>淺焙: 1:16 較強烈。</li>
                <li>中深焙: 1:17 平衡 [web:11][web:12]。</li>
            </ul>
        </section>
    </div>

    <script>
        let records = JSON.parse(localStorage.getItem('coffeeRecords')) || [];
        let currentDate = new Date().toISOString().split('T')[0];

        document.getElementById('date-display').textContent = new Date().toLocaleDateString('zh-TW');
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
            const time = document.getElementById('time').value;
            const method = document.getElementById('method').value;
            const roast = document.getElementById('roast').value;
            const bean = document.getElementById('bean').value;
            const notes = document.getElementById('notes').value;

            if (!photoFile || !notes) {
                alert('請上傳相片並填寫心得！');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const record = {
                    id: Date.now(),
                    date: currentDate,
                    photo: e.target.result,
                    time,
                    method,
                    roast,
                    bean,
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
            document.getElementById('time').value = '';
            document.getElementById('bean').value = '';
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
            } else if (section === 'guide') {
                document.getElementById('guide-section').style.display = 'block';
            }
        }

        function updateRecords() {
            const singleDiv = document.getElementById('single-records');
            singleDiv.innerHTML = records.map(r => `
                <div class="record">
                    <img src="${r.photo}" alt="咖啡相片">
                    <div class="record-meta">
                        <p><strong>日期:</strong> ${r.date}</p>
                        <p><strong>時間:</strong> ${r.time} 分</p>
                        <p><strong>方式:</strong> ${r.method}</p>
                        <p><strong>烘焙:</strong> ${r.roast}</p>
                        <p><strong>豆子:</strong> ${r.bean}</p>
                        <p><strong>心得:</strong> ${r.notes}</p>
                    </div>
                </div>
            `).join('');

            const months = [...new Set(records.map(r => r.date.slice(0,7)))];
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
            monthDiv.innerHTML = monthRecords.map(r => `
                <div class="thumbnail">
                    <img src="${r.photo}" onclick="viewDetail(${r.id})" title="${r.notes.substring(0,50)}...">
                    <p>${r.date.slice(8)}</p>
                </div>
            `).join('');
        }

        function viewDetail(id) {
            const record = records.find(r => r.id === id);
            if (record) {
                alert(`日期: ${record.date}\n心得: ${record.notes}`);
            }
        }
    </script>
</body>
</html>
