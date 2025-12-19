# Christmasdailygift



<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kirim Ucapan Natal ke Teman 🎄</title>
    
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
            font-size: clamp(2.5rem, 8vw, 3.5rem);
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

        label { font-size: 0.85rem; font-weight: 600; color: var(--green); }

        input, textarea {
            width: 100%;
            padding: 12px;
            margin: 5px 0 15px 0;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-family: 'Poppins', sans-serif;
            font-size: 0.9rem;
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
        }

        .btn-primary { background: var(--red); color: white; }
        .btn-whatsapp { background: #25d366; color: white; margin-top: 10px; }
        .btn:hover { opacity: 0.9; transform: translateY(-2px); }

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
        <div id="formSection">
            <h2>Kirim Kartu Ucapan 🎄</h2>
            <form id="waForm">
                <label>Nomor WhatsApp Tujuan:</label>
                <input type="tel" id="nomorTujuan" placeholder="Contoh: 08123456789" required>
                
                <label>Nama Pengirim (Anda):</label>
                <input type="text" id="nama" placeholder="Masukkan nama Anda" required>
                
                <label>Isi Ucapan:</label>
                <textarea id="pesan" rows="3" placeholder="Tulis ucapan selamat Natal..." required></textarea>
                
                <button type="submit" class="btn btn-primary">Siapkan Pesan ✨</button>
            </form>
        </div>

        <div id="confirmSection" style="display: none; text-align: center;">
            <div style="font-size: 3rem;">🎁</div>
            <h3 style="color: var(--red); margin-bottom: 5px;">Pesan Siap!</h3>
            <p style="font-size: 0.9rem; margin-bottom: 20px;">Klik tombol di bawah untuk mengirim ke nomor tersebut.</p>
            
            <button id="btnKirimSekarang" class="btn btn-whatsapp">Buka WhatsApp & Kirim</button>
            
            <p onclick="window.location.reload()" style="margin-top: 15px; font-size: 0.8rem; color: #888; cursor: pointer; text-decoration: underline;">Edit Pesan / Batal</p>
        </div>
    </div>

    <script>
        // 1. TIMER
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

        // 2. LOGIKA KIRIM
        let finalUrl = "";

        document.getElementById('waForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            let nomor = document.getElementById('nomorTujuan').value.replace(/\D/g, ''); // Ambil angka saja
            const nama = document.getElementById('nama').value;
            const pesan = document.getElementById('pesan').value;

            // Validasi & Format Nomor (Ubah 08 jadi 628)
            if (nomor.startsWith('0')) {
                nomor = '62' + nomor.slice(1);
            }

            const teks = `Halo! 🎄%0A%0A*${pesan}*%0A%0A- Dari: *${nama}*%0A%0A✨ Selamat Natal ✨`;
            finalUrl = `https://api.whatsapp.com/send?phone=${nomor}&text=${teks}`;

            // Sembunyikan form, tampilkan konfirmasi
            document.getElementById('formSection').style.display = 'none';
            document.getElementById('confirmSection').style.display = 'block';
        });

        document.getElementById('btnKirimSekarang').addEventListener('click', function() {
            window.open(finalUrl, '_blank');
        });

        // 3. SNOW
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
