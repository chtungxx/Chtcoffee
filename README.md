[Index.html](https://github.com/user-attachments/files/25677399/Index.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>오늘커피 ☕️ Today’s coffee</title>
    <style>
        :root {
            --main-brown: #5D4037;
            --glass-bg: rgba(255, 255, 255, 0.6); /* 控制整體容器透明度 */
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            min-height: 100vh;
            /* 海邊咖啡店背景圖 */
            background: linear-gradient(rgba(255,255,255,0.4), rgba(255,255,255,0.4)), 
                        url('https://images.unsplash.com');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: var(--glass-bg);
            backdrop-filter: blur(8px); /* 毛玻璃效果 */
            padding: 30px;
            border-radius: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            box-sizing: border-box;
        }

        header { text-align: center; margin-bottom: 25px; }
        h1 { color: var(--main-brown); margin: 0; font-size: 28px; }
        h2 { font-size: 18px; color: #777; margin-top: 5px; }

        /* 表單樣式 */
        .section-title { border-left: 4px solid var(--main-brown); padding-left: 10px; margin: 25px 0 15px; color: var(--main-brown); }
        
        .input-group { background: rgba(255,255,255,0.8); padding: 20px; border-radius: 15px; margin-bottom: 20px; }
        
        label { display: block; font-size: 14px; font-weight: bold; margin-bottom: 5px; color: #555; }
        input, select, textarea { 
            width: 100%; padding: 12px; margin-bottom: 15px; 
            border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box;
            font-size: 15px;
        }

        #preview-container { 
            width: 100%; height: 200px; background: #eee; border-radius: 10px; 
            margin-bottom: 15px; overflow: hidden; display: flex; align-items: center; justify-content: center;
        }
        #preview-img { width: 100%; height: 100%; object-fit: cover; display: none; }

        .btn-save { 
            background: var(--main-brown); color: white; border: none; 
            padding: 15px; width: 100%; border-radius: 12px; cursor: pointer; font-size: 16px; font-weight: bold;
        }

        /* 歷史檢視與縮圖 */
        .month-block { margin-top: 20px; }
        .month-label { font-weight: bold; background: var(--main-brown); color: white; padding: 4px 12px; border-radius: 15px; font-size: 13px; display: inline-block; margin-bottom: 10px; }
        
        .history-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; }
        .history-item { 
            aspect-ratio: 1/1; border-radius: 10px; overflow: hidden; cursor: pointer; 
            border: 2px solid white; transition: transform 0.2s;
        }
        .history-item:hover { transform: scale(1.05); }
        .history-item img { width: 100%; height: 100%; object-fit: cover; }

        /* 指南樣式 */
        .guide-box { background: rgba(111, 78, 55, 0.1); padding: 15px; border-radius: 15px; font-size: 14px; line-height: 1.6; }

        /* 詳細資料彈窗 */
        .modal {
            display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
            width: 85%; max-width: 400px; background: white; padding: 25px; border-radius: 20px; z-index: 100;
            box-shadow: 0 0 50px rgba(0,0,0,0.3);
        }
        .modal-overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 99; }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>오늘커피 ☕️</h1>
        <h2>Today’s coffee</h2>
    </header>

    <!-- 1. 紀錄功能 -->
    <div class="input-group">
        <label>📸 上傳今日咖啡相片</label>
        <input type="file" accept="image/*" onchange="previewImage(this)">
        <div id="preview-container">
            <span id="placeholder-text" style="color:#aaa">尚未選擇相片</span>
            <img id="preview-img">
        </div>

        <label>📅 日期</label>
        <input type="date" id="date-input">

        <label>📍 產區 / 豆名 (選填)</label>
        <input type="text" id="bean-name" placeholder="例如：埃塞俄比亞 谷吉">

        <div style="display: flex; gap: 10px;">
            <div style="flex:1">
                <label>🔥 烘焙度</label>
                <select id="roast-level">
                    <option>淺焙</option>
                    <option>中焙</option>
                    <option>深焙</option>
                </select>
            </div>
            <div style="flex:1">
                <label>⏱ 沖煮時間</label>
                <input type="text" id="brew-time" placeholder="2:30">
            </div>
        </div>

        <label>💡 沖煮心得</label>
        <textarea id="brew-note" rows="3" placeholder="今天的口感如何？"></textarea>

        <button class="btn-save" onclick="saveRecord()">儲存今日紀錄</button>
    </div>

    <!-- 2. 手沖指南 -->
    <div class="section-title">手沖指南 Pourover Guide</div>
    <div class="guide-box">
        <strong>黃金比例：1:15</strong> (例如 20g 粉對 300ml 水)<br>
        <strong>步驟說明：</strong><br>
        1. 悶蒸：注入 40g 水，靜置 30 秒。<br>
        2. 第一段：中心繞圈注入至 150g。<br>
        3. 第二段：緩慢注水至 300g，等待濾乾。<br>
        4. 總時長：建議在 2'15" - 2'45" 之間。
    </div>

    <!-- 3. 按月檢視 -->
    <div class="section-title">歷史紀錄回顧</div>
    <div id="history-list">
        <!-- 紀錄會顯示在這裡 -->
    </div>
</div>

<!-- 彈出詳細資料 -->
<div class="modal-overlay" id="overlay" onclick="closeModal()"></div>
<div class="modal" id="detail-modal">
    <h3 id="modal-date" style="margin-top:0; color:var(--main-brown);"></h3>
    <img id="modal-img" style="width:100%; border-radius:10px; margin-bottom:15px;">
    <div id="modal-body" style="font-size: 14px; line-height: 1.8;"></div>
    <button class="btn-save" style="margin-top:20px;" onclick="closeModal()">關閉</button>
</div>

<script>
    let uploadedImageBase64 = "";

    // 頁面加載時：設定預設日期、讀取紀錄
    window.onload = () => {
        document.getElementById('date-input').valueAsDate = new Date();
        loadHistory();
    };

    // 處理圖片預覽
    function previewImage(input) {
        if (input.files && input.files[0]) {
            const reader = new FileReader();
            reader.onload = (e) => {
                uploadedImageBase64 = e.target.result;
                document.getElementById('preview-img').src = uploadedImageBase64;
                document.getElementById('preview-img').style.display = 'block';
                document.getElementById('placeholder-text').style.display = 'none';
            };
            reader.readAsDataURL(input.files[0]);
        }
    }

    // 儲存紀錄
    function saveRecord() {
        const record = {
            id: Date.now(),
            date: document.getElementById('date-input').value,
            bean: document.getElementById('bean-name').value || "未命名豆子",
            roast: document.getElementById('roast-level').value,
            time: document.getElementById('brew-time').value,
            note: document.getElementById('brew-note').value,
            img: uploadedImageBase64 || "https://via.placeholder.com"
        };

        const history = JSON.parse(localStorage.getItem('coffee_history') || '[]');
        history.unshift(record);
        localStorage.setItem('coffee_history', JSON.stringify(history));

        alert("紀錄成功！");
        location.reload();
    }

    // 載入歷史紀錄並按月分組
    function loadHistory() {
        const history = JSON.parse(localStorage.getItem('coffee_history') || '[]');
        const container = document.getElementById('history-list');
        const groups = {};

        // 按月份分組 (YYYY-MM)
        history.forEach(item => {
            const month = item.date.substring(0, 7);
            if (!groups[month]) groups[month] = [];
            groups[month].push(item);
        });

        for (const month in groups) {
            const monthDiv = document.createElement('div');
            monthDiv.className = 'month-block';
            monthDiv.innerHTML = `<div class="month-label">${month}</div>`;
            
            const grid = document.createElement('div');
            grid.className = 'history-grid';
            
            groups[month].forEach(item => {
                const el = document.createElement('div');
                el.className = 'history-item';
                el.innerHTML = `<img src="${item.img}">`;
                el.onclick = () => showDetail(item);
                grid.appendChild(el);
            });

            monthDiv.appendChild(grid);
            container.appendChild(monthDiv);
        }
    }

    // 顯示詳細資訊
    function showDetail(item) {
        document.getElementById('modal-date').innerText = item.date + " 紀錄";
        document.getElementById('modal-img').src = item.img;
        document.getElementById('modal-body').innerHTML = `
            <strong>豆子：</strong> ${item.bean} <br>
            <strong>烘焙：</strong> ${item.roast} <br>
            <strong>時間：</strong> ${item.time} <br>
            <strong>心得：</strong> ${item.note || "無"}
        `;
        document.getElementById('detail-modal').style.display = 'block';
        document.getElementById('overlay').style.display = 'block';
    }

    function closeModal() {
        document.getElementById('detail-modal').style.display = 'none';
        document.getElementById('overlay').style.display = 'none';
    }
</script>

</body>
</html>
