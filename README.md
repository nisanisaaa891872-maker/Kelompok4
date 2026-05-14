# Kelompok4<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Enigma: Pesan Rahasia Emoji</title>
    <style>
        :root {
            --primary: #6366f1;
            --dark: #1e293b;
            --light: #f8fafc;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--dark);
            color: var(--light);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        .container {
            background: #2d3748;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            width: 90%;
            max-width: 500px;
            text-align: center;
        }

        h1 { color: var(--primary); margin-bottom: 1.5rem; }

        textarea, input {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border-radius: 8px;
            border: 1px solid #4a5568;
            background: #1a202c;
            color: #fff;
            box-sizing: border-box;
            font-size: 1rem;
        }

        .output-box {
            background: #1a202c;
            min-height: 80px;
            padding: 15px;
            margin-top: 15px;
            border-radius: 8px;
            border: 2px dashed var(--primary);
            word-wrap: break-word;
            font-family: monospace;
            cursor: pointer;
        }

        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.3s;
            width: 100%;
            margin-top: 10px;
        }

        button:hover { opacity: 0.9; transform: translateY(-2px); }
        
        .label { font-size: 0.8rem; text-align: left; display: block; color: #a0aec0; }
    </style>
</head>
<body>

<div class="container">
    <h1>🕵️ Enigma Chat</h1>
    
    <label class="label">Ketik Pesan Asli (atau Kode Rahasia):</label>
    <textarea id="userInput" rows="4" placeholder="Tulis sesuatu..."></textarea>

    <label class="label">Kata Kunci (Kunci Keamanan):</label>
    <input type="password" id="keyInput" placeholder="Masukkan kata kunci...">

    <div style="display: flex; gap: 10px;">
        <button onclick="process(true)">Enkripsi (Sembunyikan)</button>
        <button onclick="process(false)" style="background: #48bb78;">Dekripsi (Buka)</button>
    </div>

    <div class="output-box" id="resultDisplay" title="Klik untuk salin" onclick="copyResult()">
        Hasil akan muncul di sini...
    </div>
    <p style="font-size: 0.7rem; color: #718096; margin-top: 10px;">Tips: Klik pada hasil untuk menyalin kode.</p>
</div>

<script>
    // Kunci tambahan internal agar tidak mudah ditebak
    const salt = "SECRET_SALT";

    function process(isEncrypt) {
        const text = document.getElementById('userInput').value;
        const key = document.getElementById('keyInput').value;
        const display = document.getElementById('resultDisplay');

        if (!text || !key) {
            alert("Harap isi pesan dan kata kunci!");
            return;
        }

        try {
            if (isEncrypt) {
                // Gabungkan teks dengan kunci lalu ubah ke Base64
                const combined = key + salt + text;
                const encoded = btoa(unescape(encodeURIComponent(combined)));
                // Ubah beberapa karakter menjadi emoji agar terlihat unik
                display.innerText = "🔐 " + encoded.replace(/a/g, '💎').replace(/e/g, '🔥').replace(/i/g, '🌪️');
            } else {
                // Kembalikan emoji ke karakter normal
                let cleanedText = text.replace(/🔐 /g, '').replace(/💎/g, 'a').replace(/🔥/g, 'e').replace(/🌪️/g, 'i');
                const decoded = decodeURIComponent(escape(atob(cleanedText)));
                
                // Cek apakah kata kunci benar
                if (decoded.startsWith(key + salt)) {
                    display.innerText = decoded.replace(key + salt, "");
                } else {
                    display.innerText = "❌ Kata kunci salah atau kode rusak!";
                }
            }
        } catch (e) {
            display.innerText = "❌ Error: Format kode tidak valid.";
        }
    }

    function copyResult() {
        const text = document.getElementById('resultDisplay').innerText;
        if (text && !text.includes("Hasil akan muncul") && !text.includes("❌")) {
            navigator.clipboard.writeText(text);
            alert("Kode disalin ke clipboard!");
        }
    }
</script>

</body>
</html>
