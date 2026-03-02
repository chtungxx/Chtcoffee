[2026 3.html](https://github.com/user-attachments/files/25681100/2026.3.html)
<!DOCTYPE html>
<html lang="zh-HKT">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>오늘커피 ☕️ Today’s Coffee v2 - 永久儲存版</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Arial', sans-serif;
            background-image: url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?ixlib=rb-4.0.3&auto=format&fit=crop&w=2070&q=80');
            background-size: cover; background-position: center; background-attachment: fixed;
            min-height: 100vh; color: #333; padding: 20px;
        }
        .container { max-width: 600px; margin: 0 auto; background: rgba(255,255,255,0.95); border-radius: 20px; padding: 30px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); }
        h1 { text-align: center; color: #6B4E31; margin-bottom: 30px; font-size: 2em; }
        .tab { display: none; }
        .tab.active { display: block; }
        .tab-btns { display: flex; justify-content: center; margin-bottom: 20px; }
        .tab-btn { padding: 12px 24px; background: #E8D5B7; border: none; border-radius: 25px; margin: 0 5px; cursor: pointer; font-size: 1em; transition: all 0.3s; }
        .tab-btn.active { background: #6B4E31; color: white; }
        .form-group { margin-bottom: 20px; }
        label { display: block; margin-bottom: 8px; font-weight: bold; color: #6B4E31; }
        input, textarea, select { width: 100%; padding: 12px; border: 2px solid #E8D5B7; border-radius: 10px; font-size: 1em; }
        textarea { height: 100px; resize: vertical; }
        button { background: #6B4E31; color: white; padding: 15px 30px; border: none; border-radius: 25px; font-size: 1.1em; cursor: pointer; width: 100%; margin-top: 10px; transition: background 0.3s; }
        button:hover { background: #8B4513; }
        .records-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 15px; margin-top: 20px; }
        .record-thumb { position: relative; border-radius: 15px; overflow: hidden; cursor: pointer; box-shadow: 0 5px 15px rgba(0,0,0,0.2); transition: transform 0.3s; }
        .record-thumb:hover { transform: scale(1.05); }
        .record-thumb img { width: 100%; height: 120px; object-fit: cover; }
        .record-date { position: absolute; bottom: 0; left: 0; right: 0; background: rgba(0,0,0,0.7); color: white; padding: 5px; text-align: center; font-size: 0.9em; }
        .guide { background: #FFF8E7; padding: 20px; border-radius: 15px; margin-top: 20px; }
        .backup-btns { display: flex; gap: 10px; margin-top: 20px; }
        .backup-btn { flex: 1; background: #4CAF50; font-size: 0.9em; padding: 10px; }
        .backup-btn:hover { background: #45a049; }
        #detailModal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 1000; }
        #detailModal img { max-width: 90%; max-height: 70%; display: block; margin: 50px auto; border-radius: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>오늘커피 ☕️<br>Today’s Coffee v2</h1>
        
        <div class="tab-btns">
            <button class="tab-btn active" onclick="showTab('log')">📸 記錄咖啡</button>
            <button class="tab-btn" onclick="showTab('view')">📋 查看紀錄</button>
            <button class="tab-btn" onclick="showTab('guide')">☕ 沖煮指南</button>
        </div>

        <!-- 記錄咖啡 tab -->
        <div id="log" class="tab active">
            <form id="coffeeForm">
                <div class="form-group">
                    <label>📷 上傳咖啡相片</label>
                    <input type="file" id="photo" accept="image/*" required>
                    <small style="color: #666;">會自動壓縮到200KB內，安全儲存！</small>
                </div>
                <div class="form-group">
                    <label>📅 日期</label>
                    <input type="date" id="date" required>
                </div>
                <div class="form-group">
                    <label>⏱️ 沖煮時間 (分鐘)</label>
                    <input type="number" id="brewTime" min="1" max="20" step="0.5">
                </div>
                <div class="form-group">
                    <label>☕ 沖煮方式</label>
                    <select id="method">
                        <option value="手沖">手沖</option>
                        <option value="浸泡">浸泡</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>🌾 咖啡豆烘焙度</label>
                    <select id="roast">
                        <option value="淺烘">淺烘</option>
                        <option value="中烘">中烘</option>
                        <option value="深烘">深烘</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>🌍 咖啡豆產區／名稱</label>
                    <input type="text" id="bean" placeholder="例如：衣索比亞耶加雪菲">
                </div>
                <div class="form-group">
                    <label>💭 今日心得</label>
                    <textarea id="notes" placeholder="今日感覺如何？酸度、苦澀、回甘..."></textarea>
                </div>
                <button type="submit">💾 保存今日咖啡</button>
            </form>
            <div class="backup-btns">
                <button class="backup-btn" onclick="exportData()">💾 匯出備份</button>
                <button class="backup-btn" onclick="importData()">📥 匯入備份</button>
            </div>
        </div>

        <!-- 查看紀錄 tab -->
        <div id="view" class="tab">
            <div id="recordsList"></div>
        </div>

        <!-- 沖煮指南 tab -->
        <div id="guide" class="tab">
            <div class="guide">
                <h3>🌊 手沖咖啡指南 (1:16 比例)</h3>
                <p><strong>水溫：90-92°C</strong></p>
                <ol>
                    <li>悶蒸 (40%)：30秒內注入總水量40%，輕攪勻</li>
                    <li>第一沖 (30%)：慢慢畫圈注入30%</li>
                    <li>第二沖 (20%)：繼續畫圈注入20%</li>
                    <li>滴濾完成 (10%)：最後注入剩餘水量</li>
                </ol>
                <hr>
                <h3>🥤 浸泡式 (冷萃) 指南 (1:15)</h3>
                <p><strong>水溫：室溫 (或冰水)</strong></p>
                <ol>
                    <li>粗磨咖啡粉：水 1:15</li>
                    <li>浸泡 12-18 小時，偶爾攪拌</li>
                    <li>過濾完成！</li>
                </ol>
            </div>
        </div>
    </div>

    <!-- 詳情彈窗 -->
    <div id="detailModal">
        <img id="detailImg" src="" onclick="closeModal()">
    </div>

    <script>
        let records = JSON.parse(localStorage.getItem('coffeeRecords')) || [];

        // 初始化日期
        document.getElementById('date').valueAsDate = new Date();

        // Tab 切換
        function showTab(tabName) {
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(tabName).classList.add('active');
            event.target.classList.add('active');
            if (tabName === 'view') loadRecords();
        }

        // 壓縮圖片並轉 base64
        async function compressImage(file, maxSize = 200 * 1024, quality = 0.8) {
            return new Promise((resolve) => {
                const canvas = document.createElement('canvas');
                const ctx = canvas.getContext('2d');
                const img = new Image();
                img.onload = () => {
                    // 調整尺寸
                    let { width, height } = img;
                    if (width > 800 || height > 800) {
                        const ratio = Math.min(800 / width, 800 / height);
                        width *= ratio; height *= ratio;
                    }
                    canvas.width = width; canvas.height = height;
                    ctx.drawImage(img, 0, 0, width, height);
                    
                    let dataUrl = canvas.toDataURL('image/jpeg', quality);
                    let size = dataUrl.length * 3 / 4;
                    
                    // 如果仲大，遞減 quality
                    while (size > maxSize && quality > 0.1) {
                        quality -= 0.1;
                        dataUrl = canvas.toDataURL('image/jpeg', quality);
                        size = dataUrl.length * 3 / 4;
                    }
                    resolve(dataUrl);
                };
                img.src = URL.createObjectURL(file);
            });
        }

        // 保存紀錄
        document.getElementById('coffeeForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const photoFile = document.getElementById('photo').files[0];
            if (!photoFile) return alert('請選擇相片！');
            
            const compressedImg = await compressImage(photoFile);
            
            const record = {
                id: Date.now(),
                date: document.getElementById('date').value,
                photo: compressedImg,
                brewTime: document.getElementById('brewTime').value,
                method: document.getElementById('method').value,
                roast: document.getElementById('roast').value,
                bean: document.getElementById('bean').value,
                notes: document.getElementById('notes').value
            };
            
            records.unshift(record); // 新增到最前
            localStorage.setItem('coffeeRecords', JSON.stringify(records));
            alert('✅ 保存成功！去『查看紀錄』睇返啦～');
            document.getElementById('coffeeForm').reset();
            document.getElementById('date').valueAsDate = new Date();
        });

        // 加載紀錄
        function loadRecords() {
            const container = document.getElementById('recordsList');
            if (records.length === 0) {
                container.innerHTML = '<p style="text-align:center; color:#666;">暫無紀錄，快去記錄今日咖啡吧！☕</p>';
                return;
            }
            
            // 分組按月
            const monthly = {};
            records.forEach(r => {
                const month = r.date.slice(0,7); // YYYY-MM
                if (!monthly[month]) monthly[month] = [];
                monthly[month].push(r);
            });
            
            let html = '';
            Object.keys(monthly).sort((a,b)=>b.localeCompare(a)).forEach(month => {
                html += `<h3 style="margin:20px 0 10px; color:#6B4E31;">${month}</h3>`;
                html += monthly[month].map(r => `
                    <div class="record-thumb" onclick="viewDetail(${r.id})">
                        <img src="${r.photo}" alt="${r.date}">
                        <div class="record-date">${r.date.slice(8)}日</div>
                    </div>
                `).join('');
            });
            container.innerHTML = html;
        }

        // 查看詳情
        function viewDetail(id) {
            const record = records.find(r => r.id === id);
            if (record) {
                document.getElementById('detailImg').src = record.photo;
                document.getElementById('detailModal').style.display = 'block';
            }
        }

        function closeModal() {
            document.getElementById('detailModal').style.display = 'none';
        }

        // 備份功能
        function exportData() {
            const dataStr = JSON.stringify(records, null, 2);
            const blob = new Blob([dataStr], {type: 'application/json'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url; a.download = `coffee_backup_${new Date().toISOString().slice(0,10)}.json`;
            a.click();
        }

        function importData() {
            const input = document.createElement('input');
            input.type = 'file'; input.accept = '.json';
            input.onchange = (e) => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onload = (ev) => {
                    try {
                        const imported = JSON.parse(ev.target.result);
                        records = imported;
                        localStorage.setItem('coffeeRecords', JSON.stringify(records));
                        alert('✅ 匯入成功！');
                        loadRecords();
                    } catch (err) {
                        alert('匯入失敗，請檢查檔案格式');
                    }
                };
                reader.readAsText(file);
            };
            input.click();
        }

        // 關閉彈窗
        window.onclick = (e) => {
            if (e.target.id === 'detailModal') closeModal();
        };
    </script>
</body>
</html>
