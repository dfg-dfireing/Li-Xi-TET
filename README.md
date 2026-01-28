<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lì Xì Tết 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        /* --- GIAO DIỆN & MÀU SẮC --- */
        :root {
            --red: #b71c1c;
            --gold: #ffd700;
            --bg: #8b0000;
        }
        body {
            font-family: 'Roboto', sans-serif;
            background: var(--bg);
            background-image: radial-gradient(circle, #d32f2f, #8b0000);
            color: white;
            text-align: center;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
            overflow-x: hidden;
        }
        .container {
            width: 90%;
            max-width: 500px;
            background: rgba(0,0,0,0.4);
            padding: 20px;
            border-radius: 20px;
            border: 2px solid var(--gold);
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
        }
        h1, h2 {
            font-family: 'Dancing Script', cursive;
            color: var(--gold);
            text-shadow: 2px 2px 4px black;
        }

        /* --- MÀN HÌNH ĐĂNG KÝ --- */
        #registration-screen input {
            padding: 15px 30px;
            font-size: 18px;
            border-radius: 30px;
            border: 2px solid var(--gold);
            background: rgba(255,255,255,0.9);
            color: black;
            text-align: center;
            width: 80%;
            margin-bottom: 20px;
        }

        /* --- NÚT BẤM --- */
        .btn {
            background: linear-gradient(135deg, var(--gold), #ffeb3b);
            color: #8b0000;
            padding: 12px 35px;
            font-size: 20px;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(0,0,0,0.4);
            transition: all 0.3s;
        }
        .btn:hover { transform: translateY(-3px); box-shadow: 0 8px 25px rgba(255,215,0,0.6); }

        /* --- MÀN HÌNH CHỌN BAO --- */
        .grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 20px;
        }
        .envelope {
            aspect-ratio: 2/3;
            background: #d32f2f;
            border: 2px solid var(--gold);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 40px;
            cursor: pointer;
            transition: transform 0.2s;
            box-shadow: 0 5px 10px rgba(0,0,0,0.3);
        }
        .envelope:hover { transform: scale(1.1); }
        .hidden { display: none !important; }

        /* --- KẾT QUẢ & THÔNG BÁO --- */
        .result-box {
            background: white;
            color: black;
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        .status-badge {
            background: #ff9800; /* Cam - Đang xử lý */
            color: white;
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 14px;
            font-weight: bold;
            display: inline-block;
            margin-top: 10px;
        }
        .status-success { background: #4caf50; } /* Xanh lá - Thành công */

        /* --- QR CODE --- */
        .qr-img {
            width: 200px;
            height: 200px;
            object-fit: contain;
            margin-top: 10px;
            border: 1px solid #ddd;
            padding: 5px;
        }
    </style>
</head>
<body>

    <div class="container">
        <div id="screen-register">
            <h1>🧧 HÁI LỘC ĐẦU NĂM 🧧</h1>
            <p>Bạn Hãy nhập tên để "thần tài" gửi quà đúng địa chỉ!</p>
            <input type="text" id="username" placeholder="Nhập tên của bạn...">
            <br>
            <button class="btn" onclick="startPlay()">Bắt đầu chơi</button>
        </div>

        <div id="screen-game" class="hidden">
            <h2>Chọn một bao lì xì</h2>
            <div class="grid">
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
                <div class="envelope" onclick="pickGift(this)">🧧</div>
            </div>
        </div>

        <div id="screen-result" class="hidden">
            <h2>🎉 KẾT QUẢ 🎉</h2>
            <div class="result-box">
                <p>Chúc mừng <b id="display-name" style="color: red;"></b> đã nhận được:</p>
                <h3 id="gift-name" style="color: #b71c1c; font-size: 24px;">...</h3>
                
                <div id="money-section" class="hidden">
                    <hr>
                </div>

                <div id="status-section">
                    <span id="status-label" class="status-badge">⏳ Đang xử lý...</span>
                    <p style="font-size: 12px; margin-top: 5px; color: #666;">Hệ thống đang gửi kết quả cho DFG...</p>
                </div>
            </div>
            <br>
            <button class="btn" onclick="location.reload()" style="background: #555; font-size: 16px;">Chơi lại</button>
        </div>
    </div>

    <script>
        // --- CẤU HÌNH ---
        // 1. Dán cái link Google Web App bạn lấy ở Bước 1 vào đây:
        const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbxzAd2lKjOe6knNYIXoD_axsywZw9ZatNWLA7VIla5PYT5BPCVAHgMBuElczgtFy1vlyQ/exec'; 
        
        let currentPlayer = "";

        const gifts = [
            { name: "Chúc Bạn Vạn sự như ý!", money: false },
            { name: "5.000 VNĐ - Lộc lá đầu xuân", money: true },
            { name: "Bạn Vui vẻ không quạo nha", money: false },
            { name: "16.000 VNĐ - May mắn cả năm", money: true },
            { name: "Chúc Bạn Sức khỏe dồi dào!", money: false },
            { name: "10.000 VNĐ - Phát tài phát lộc", money: true },
            { name: "50.000 VNĐ - Tấn tài tấn lộc", money: true },
            { name: "Chúc Bạn May Mắn Lần Sau", money: false },
            { name: "Chúc Bạn xuân mới tài lộc đủ đầy", money: false }
        ];

        function startPlay() {
            const nameInput = document.getElementById('username').value;
            if (!nameInput) {
                alert("Bạn ơi, nhập tên vào đã chứ!");
                return;
            }
            currentPlayer = nameInput;
            document.getElementById('screen-register').classList.add('hidden');
            document.getElementById('screen-game').classList.remove('hidden');
        }

        function pickGift(el) {
            // Chọn quà ngẫu nhiên
            const gift = gifts[Math.floor(Math.random() * gifts.length)];
            
            // Chuyển màn hình
            document.getElementById('screen-game').classList.add('hidden');
            document.getElementById('screen-result').classList.remove('hidden');

            // Hiển thị thông tin
            document.getElementById('display-name').innerText = currentPlayer;
            document.getElementById('gift-name').innerText = gift.name;

            // Nếu trúng tiền thì hiện QR
            if (gift.money) {
                document.getElementById('money-section').classList.remove('hidden');
            }

            // Gửi dữ liệu về Google Sheet
            sendDataToSheet(currentPlayer, gift.name);
        }

        function sendDataToSheet(name, gift) {
            const statusLabel = document.getElementById('status-label');
            
            // Tạo form ảo để gửi dữ liệu
            const formData = new FormData();
            formData.append('Tên người chơi', name);
            formData.append('Phần thưởng', gift);

            fetch(SCRIPT_URL, { method: 'POST', body: formData })
                .then(response => {
                    statusLabel.innerText = "✅ Đã gửi thành công!";
                    statusLabel.classList.add('status-success');
                    statusLabel.style.background = "#4caf50";
                    document.querySelector('#status-section p').innerText = "Chủ nhà đã nhận được thông tin. Vui lòng chờ nhận thưởng!";
                })
                .catch(error => {
                    console.error('Error!', error.message);
                    statusLabel.innerText = "⚠️ Lỗi kết nối";
                    statusLabel.style.background = "#f44336";
                });
        }
    </script>
</body>
</html>
