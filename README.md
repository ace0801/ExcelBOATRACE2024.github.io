<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Excel BOATRACE</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
            position: relative; /* 画像の位置決めの基準にする */
        }
        header {
            background-color: #0077cc;
            color: #fff;
            padding: 15px 20px;
            text-align: center;
            width: 100%;
        }
        nav {
            background-color: #005a99;
            overflow: hidden;
            width: 100%;
        }
        nav a {
            float: left;
            display: block;
            color: #fff;
            text-align: center;
            padding: 14px 16px;
            text-decoration: none;
        }
        nav a:hover {
            background-color: #004477;
        }
        main {
            padding: 20px;
            box-sizing: border-box;
        }
        footer {
            background-color: #0077cc;
            color: #fff;
            text-align: center;
            padding: 10px 0;
            width: 100%;
        }
        .hidden {
            display: none;
        }
        .image-container {
            position: fixed; /* 固定位置 */
            bottom: 0;
            right: 0;
            margin: 10px;
        }
        .image-container img {
            max-width: 500px; /* 適度なサイズに設定 */
            height: auto;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }
    </style>
</head>
<body>
    <header>
        <h1>Excel BOATRACE</h1>
    </header>
    <nav>
        <a href="#home">ホーム</a>
        <a href="#predictions">予想情報</a>
        <a href="#purchase">購入</a>
        <a href="#contact">お問い合わせ</a>
    </nav>
    <main>
        <section id="home">
            <h2>ホーム</h2>
            <p>このサイトでは、最新のボートレース予想を提供しています。的中率の高い情報をお届けします。</p>
        </section>
        <section id="predictions">
            <h2>予想情報</h2>
            <p>今日のレース予想を以下から選択してください:</p>
            <label for="folderSelect">フォルダを選択:</label>
            <select id="folderSelect" onchange="updateFileOptions()">
                <option value="">選択してください</option>
                <option value="nige">【逃げ重視型】</option>
                <option value="sogo">【総合評価型】</option>
            </select>
            
            <div id="fileSelection" class="hidden">
                <label for="fileSelect">ファイルを選択:</label>
                <select id="fileSelect">
                    <!-- JavaScriptでファイルオプションを追加 -->
                </select>
                <button onclick="checkPurchaseAndOpenFile()">ファイルを表示</button>
            </div>
        </section>
        <section id="purchase">
            <h2>購入</h2>
            <p>予想情報を購入するには以下のリンクをクリックしてください。</p>
            <a href="#" onclick="simulatePayment()">PayPalで購入 (シミュレーション)</a>
        </section>
        <section id="contact">
            <h2>お問い合わせ</h2>
            <form>
                <label for="name">名前:</label><br>
                <input type="text" id="name" name="name"><br>
                <label for="email">メール:</label><br>
                <input type="email" id="email" name="email"><br>
                <label for="message">メッセージ:</label><br>
                <textarea id="message" name="message"></textarea><br>
                <input type="submit" value="送信">
            </form>
        </section>
    </main>
    <div class="image-container">
        <!-- リポジトリ内の画像ファイルへの相対パス -->
        <img src="images/X_1111.png" alt="画像">
    </div>
    <footer>
        <p>© 2024 Excel BOATRACE</p>
    </footer>
    <script>
        const fileData = {
            "nige": [
                {
                    "name": "2024年08月07日 BOATRACE桐生_11R",
                    "path": "2024年08月07日_桐生/2024年08月07日  BOATRACE桐生_11R  締切時刻 20_04  『 逃げ重視型 』.html"
                },
                {
                    "name": "2024年08月07日 BOATRACE桐生_12R",
                    "path": "2024年08月07日_桐生/2024年08月07日  BOATRACE桐生_12R  締切時刻 20_35  『 逃げ重視型 』.html"
                }
            ],
            "sogo": [
                {
                    "name": "2024年08月07日 BOATRACE桐生_11R",
                    "path": "2024年08月07日_桐生/2024年08月07日  BOATRACE桐生_11R  締切時刻 20_04  『 総合評価型 』.html"
                },
                {
                    "name": "2024年08月07日 BOATRACE桐生_12R",
                    "path": "2024年08月07日_桐生/2024年08月07日  BOATRACE桐生_12R  締切時刻 20_35  『 総合評価型 』.html"
                }
            ]
        };

        let purchaseConfirmed = false; // 購入確認フラグ

        function updateFileOptions() {
            const folder = document.getElementById('folderSelect').value;
            const fileSelect = document.getElementById('fileSelect');
            fileSelect.innerHTML = ''; // ファイル選択肢をクリア

            if (folder && fileData[folder]) {
                document.getElementById('fileSelection').classList.remove('hidden');

                fileData[folder].forEach(file => {
                    const option = document.createElement('option');
                    option.value = file.path;
                    option.textContent = file.name;
                    fileSelect.appendChild(option);
                });
            } else {
                document.getElementById('fileSelection').classList.add('hidden');
            }
        }

        function simulatePayment() {
            // 支払い完了のシミュレーション
            purchaseConfirmed = true;
            alert('支払いが確認されました。ファイルを表示できます。');
        }

        function checkPurchaseAndOpenFile() {
            if (!purchaseConfirmed) {
                alert('購入が確認されていません。購入を完了してください。');
                return;
            }

            const folder = document.getElementById('folderSelect').value;
            const filePath = document.getElementById('fileSelect').value;
            const folderPaths = {
                'nige': 'predictions/nige/', // GitHub Pages用の相対パス
                'sogo': 'predictions/sogo/'  // GitHub Pages用の相対パス
            };

            if (filePath) {
                window.open(folderPaths[folder] + filePath, '_blank');
            } else {
                alert('ファイルを選択してください。');
            }
        }
    </script>
</body>
</html>
