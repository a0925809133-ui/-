<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vibe.Aura 生命靈數客製化分析</title>
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="apple-touch-icon" href="https://i.imgur.com/8N4N5R8.png"> <style>
        :root { --primary: #7b68ee; --gold: #c6a059; --bg: #fdfdfd; }
        body { font-family: -apple-system, sans-serif; background: var(--bg); padding: 15px; color: #333; margin: 0; padding-bottom: 50px;}
        .card { background: white; border-radius: 20px; padding: 25px; box-shadow: 0 10px 25px rgba(123, 104, 238, 0.1); max-width: 500px; margin: auto; }
        
        /* Logo 區域 */
        .brand-header { text-align: center; margin-bottom: 25px; }
        .brand-logo { width: 100px; height: 100px; border-radius: 50%; object-fit: cover; box-shadow: 0 4px 8px rgba(198, 160, 89, 0.3); }
        .brand-name { font-size: 1.5em; color: var(--gold); font-weight: bold; margin-top: 10px; }
        
        h2 { color: var(--primary); text-align: center; font-size: 1.3em; margin-bottom: 20px; }
        .input-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; color: #555; }
        input, select { width: 100%; padding: 12px; border: 1px solid #eee; border-radius: 10px; box-sizing: border-box; background: #fafafa; transition: 0.3s; }
        input:focus { border-color: var(--primary); outline: none; background: #fff; }
        
        button.btn-calc { width: 100%; padding: 15px; background: linear-gradient(135deg, var(--primary), #6a5acd); color: white; border: none; border-radius: 12px; font-size: 16px; font-weight: bold; cursor: pointer; transition: 0.3s; margin-top: 10px; }
        button.btn-calc:hover { box-shadow: 0 4px 12px rgba(123, 104, 238, 0.3); }
        
        #result { margin-top: 25px; display: none; border-top: 2px solid #f0f0f0; padding-top: 20px; animation: fadeIn 0.5s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        
        .highlight { color: #d63384; font-weight: bold; font-size: 1.1em; }
        .crystal-box { background: #f8f8ff; padding: 15px; border-radius: 12px; margin-top: 15px; border-left: 4px solid var(--gold); }
        .crystal-box ul { padding-left: 20px; margin: 10px 0 0 0; }
        .crystal-box li { margin-bottom: 8px; line-height: 1.4; }
        
        .info-footer { font-size: 0.85em; color: #888; text-align: center; margin-top: 25px; border-top: 1px solid #eee; padding-top: 15px;}
        
        /* IG 導流按鈕 */
        .ig-connect { display: block; width: 80%; margin: 25px auto 0 auto; padding: 12px; background: #fff; color: var(--gold); border: 2px solid var(--gold); text-align: center; text-decoration: none; border-radius: 30px; font-weight: bold; transition: 0.3s; }
        .ig-connect:hover { background: var(--gold); color: white; box-shadow: 0 4px 10px rgba(198, 160, 89, 0.4); }
        
    </style>
</head>
<body>

<div class="card">
    <div class="brand-header">
        <img src="https://i.imgur.com/HnUjUbe.png" alt="Vibe.Aura Logo" class="brand-logo">
        <div class="brand-name">Vibe.Aura</div>
    </div>
    
    <h2>✨ 生命靈數與水晶分析</h2>
    
    <div class="input-group">
        <label>顧客姓名</label>
        <input type="text" id="name" placeholder="請輸入姓名">
    </div>
    <div class="input-group">
        <label>西元生日</label>
        <input type="date" id="birthday">
    </div>
    <div class="input-group">
        <label>性別</label>
        <select id="gender">
            <option value="女">女</option>
            <option value="男">男</option>
            <option value="其他">其他</option>
        </select>
    </div>
    <div class="input-group">
        <label>手圍 (cm)</label>
        <input type="number" id="wrist" placeholder="例如: 15.5" step="0.1">
    </div>
    <button onclick="calculate()" class="btn-calc">開始分析</button>

    <div id="result">
        <h3>分析報告：<span id="resName"></span></h3>
        <p>您的主命數為：<span id="resMainNum" class="highlight"></span></p>
        <p id="masterNote" style="color: var(--primary); font-size: 0.95em; font-weight: bold; display: none; background: #eeeefd; padding: 8px; border-radius: 5px;"></p>
        <p>您的缺數有：<span id="resMissNums" class="highlight"></span></p>
        
        <div class="crystal-box">
            <strong>💎 推薦補強水晶：</strong>
            <ul id="resCrystals"></ul>
        </div>
        
        <div class="info-footer">
            建議手圍：<span id="resWrist"></span> cm<br>
            *本分析僅供參考，請依直覺選擇最喜愛的水晶。
        </div>

        <a href="https://www.instagram.com/vibe.aura2026?igsh=MWxwdmkyMnJjeGNkbw%3D%3D&utm_source=qr" target="_blank" class="ig-connect">
            私訊 Vibe.Aura 客製化
        </a>
    </div>
</div>

<script>
    const crystalMap = {
        1: "紅瑪瑙、金髮晶 (補強勇氣、獨立)",
        2: "月光石、粉晶 (補強溝通、合作)",
        3: "天河石、藍瑪瑙 (補強創意、表達)",
        4: "黑曜石、茶晶 (補強穩定、秩序)",
        5: "黃虎眼、白水晶 (補強自由、變通)",
        6: "綠幽靈、孔雀石 (補強責任、治癒)",
        7: "紫水晶、拉長石 (補強真理、分析)",
        8: "黃水晶、金髮晶 (補強豐盛、物質)",
        9: "舒俱徠、白幽靈 (補強大愛、智慧)"
    };

    function calculate() {
        const name = document.getElementById('name').value;
        const bday = document.getElementById('birthday').value;
        const wrist = document.getElementById('wrist').value;
        if (!bday) { alert("請選擇生日！"); return; }
        if (!name) { alert("請輸入姓名！"); return; }

        const nums = bday.replace(/-/g, "").split("").map(Number);
        
        // 1. 計算主命數
        let sum = nums.reduce((a, b) => a + b, 0);
        let masterNum = sum;
        let isMaster = false;
        
        // 卓越數判斷 (11, 22, 33)
        if (sum === 11 || sum === 22 || sum === 33) {
            isMaster = true;
        }

        // 縮小到個位數
        while (sum > 9) {
            sum = sum.toString().split("").map(Number).reduce((a, b) => a + b, 0);
        }

        // 2. 判斷缺數 (1-9)
        const birthSet = new Set(nums);
        let missing = [];
        for (let i = 1; i <= 9; i++) {
            if (!birthSet.has(i)) missing.push(i);
        }

        // 3. 渲染結果
        document.getElementById('result').style.display = 'block';
        document.getElementById('resName').innerText = name;
        document.getElementById('resMainNum').innerText = sum;
        document.getElementById('resWrist').innerText = wrist ? wrist : "--";
        
        const masterNote = document.getElementById('masterNote');
        if (isMaster) {
            masterNote.innerText = `✨ 特殊：卓越數 ${masterNum}，具備強大的靈性與療癒天賦！`;
            masterNote.style.display = 'block';
        } else {
            masterNote.style.display = 'none';
        }

        document.getElementById('resMissNums').innerText = missing.length > 0 ? missing.join(", ") : "無 (數字全滿)";

        // 4. 推薦水晶
        const crystalUl = document.getElementById('resCrystals');
        crystalUl.innerHTML = "";
        missing.forEach(num => {
            let li = document.createElement('li');
            li.innerText = `${num} 號能量：${crystalMap[num]}`;
            crystalUl.appendChild(li);
        });
        
        // 自動捲動到結果區域
        document.getElementById('result').scrollIntoView({ behavior: 'smooth' });
    }
</script>

</body>
</html>
