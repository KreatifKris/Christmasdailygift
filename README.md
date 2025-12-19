# Christmasdailygift


<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Christmas Wishes 🎄</title>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Mountains+of+Christmas:wght@700&family=Poppins:wght@300;400;600&display=swap');

        :root {
            --red: #d42426;
            --green: #1a472a;
            --gold: #f8b229;
            --white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background: radial-gradient(circle, var(--green), #0d2b1a);
            color: var(--white);
            font-family: 'Poppins', sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
        }

        h1 {
            font-family: 'Mountains of Christmas', cursive;
            font-size: clamp(2rem, 8vw, 3.5rem);
            color: var(--red);
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.4);
            margin-bottom: 20px;
            text-align: center;
        }

        .countdown-wrapper {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            justify-content: center;
        }

        .time-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(5px);
            border: 1px solid var(--gold);
            border-radius: 10px;
            padding: 10px;
            min-width: 70px;
            text-align: center;
        }

        .time-card .num { display: block; font-size: 1.8rem; font-weight: 600; }
        .time-card .label { font-size: 0.6rem; color: var(--gold); text-transform: uppercase; }

        .wish-container {
            background: rgba(255, 255, 255, 0.95);
            color: #333;
            padding: 25px;
            border-radius: 20px;
            width: 100%;
            max-width: 450px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
        }

        .wish-container h2 {
            font-family: 'Mountains of Christmas';
            color: var(--red);
            text-align: center;
            margin-bottom: 15px;
        }

        label { font-size: 0.8rem; font-weight: 600; color: var(--green); display: block; margin-top: 10px;}

        input, textarea {
            width: 100%;
            padding: 12px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-family: 'Poppins', sans-serif;
        }

        .btn {
            width: 100%;
            padding: 14px;
            border: none;
            border-radius: 10px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.3s;
            font-size: 1rem;
            margin-top: 15px;
            background: #25d366;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn:disabled { background: #ccc; cursor: not-allowed; }
        .btn:hover:not(:disabled) { background: #128c7e; transform: translateY(-2px); }

        .snow {
            position: fixed; top: -10px; color: white;
            pointer-events: none; z-index: 9999;
            animation: fall linear forwards;
        }
        @keyframes fall { to { transform: translateY(105vh); } }
    </style>
</head>
<body>

    <h1>Christmas Countdown</h1>

    <div class="countdown-wrapper">
        <div class="time-card"><span id="days" class="num">00</span><span class="label">Hari</span></div>
        <div class="time-card"><span id="hours" class="num">00</span><span class="label">Jam</span></div>
        <div class="time-card"><span id="mins" class="num">00</span><span class="label">Menit</span></div>
        <div class="time-card"><span id="secs" class="num">00</span><span class="label">Detik</span></div>
    </div>

    <div class="wish-container">
        <h2>Kirim Ucapan Natal 🎄</h2>
        <form id="waForm">
            <label>Nomor WA Tujuan:</label>
            <input type="tel" id="nomorTujuan" placeholder="Contoh: 08123456789" required>
            
            <label>Nama Pengirim:</label>
            <input type="text" id="nama" placeholder="Nama Anda" required>
            
            <label>Pesan Ucapan:</label>
            <textarea id="pesan" rows="3" placeholder="Selamat Natal..." required></textarea>
            
            <button type="submit" id="btnSubmit" class="btn">
                <span>Kirim via WhatsApp ✨</span>
            </button>
        </form>
        <p style="text-align: center; font-size: 0.7rem; color: #888; margin-top: 15px;">Data akan tersimpan secara otomatis.</p>
    </div>

    <script>
        // 1. PENGATURAN (GANTI URL DI SINI)
        const URL_GAS = "https://docs.google.com/spreadsheets/d/103iHl5YDNddyCnXAe-xeU0jN1dZmzGRMfCM0MhMR38w/edit"; 

        // 2. COUNTDOWN TIMER
        function updateTimer() {
            const sekarang = new Date();
            let natal = new Date(`December 25, ${sekarang.getFullYear()} 00:00:00`);
            if (sekarang > natal) natal.setFullYear(natal.getFullYear() + 1);
            const selisih = natal - sekarang;
            document.getElementById('days').innerText = Math.floor(selisih / 86400000).toString().padStart(2, '0');
            document.getElementById('hours').innerText = Math.floor((selisih / 3600000) % 24).toString().padStart(2, '0');
            document.getElementById('mins').innerText = Math.floor((selisih / 60000) % 60).toString().padStart(2, '0');
            document.getElementById('secs').innerText = Math.floor((selisih / 1000) % 60).toString().padStart(2, '0');
        }
        setInterval(updateTimer, 1000);
        updateTimer();

        // 3. LOGIKA KIRIM DATA & REDIRECT WA
        const form = document.getElementById('waForm');
        const btnSubmit = document.getElementById('btnSubmit');

        form.addEventListener('submit', function(e) {
            e.preventDefault();
            
            btnSubmit.innerHTML = "Sedang Memproses...";
            btnSubmit.disabled = true;

            let nomor = document.getElementById('nomorTujuan').value.replace(/\D/g, '');
            const nama = document.getElementById('nama').value;
            const pesan = document.getElementById('pesan').value;

            if (nomor.startsWith('0')) { nomor = '62' + nomor.slice(1); }

            const dataPayload = {
                nomor: nomor,
                nama: nama,
                ucapan: pesan
            };

            // Menyiapkan Link WA
            const teks = `Halo! 🎄%0A%0A*${pesan}*%0A%0A- Dari: *${nama}*%0A%0A✨ Selamat Natal ✨`;
            const waUrl = `https://api.whatsapp.com/send?phone=${nomor}&text=${teks}`;

            // Kirim ke Sheets secara "fire and forget" (tidak menunggu respon penuh)
            fetch(URL_GAS, {
                method: 'POST',
                mode: 'no-cors',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(dataPayload)
            });

            // Langsung buka WhatsApp setelah jeda sangat singkat (agar fetch sempat terkirim)
            setTimeout(() => {
                window.location.href = waUrl;
                // Mengembalikan tombol jika user kembali ke halaman ini
                btnSubmit.innerHTML = "Kirim via WhatsApp ✨";
                btnSubmit.disabled = false;
            }, 800); 
        });

        // 4. EFEK SALJU
        setInterval(() => {
            const snow = document.createElement('div');
            snow.className = 'snow';
            snow.innerHTML = '❄';
            snow.style.left = Math.random() * 100 + 'vw';
            snow.style.fontSize = (Math.random() * 10 + 10) + 'px';
            snow.style.opacity = Math.random();
            snow.style.animationDuration = (Math.random() * 3 + 2) + 's';
            document.body.appendChild(snow);
            setTimeout(() => snow.remove(), 5000);
        }, 200);
    </script>
</body>

