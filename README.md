# Christmasdailygift


<div class="wish-container" id="mainContainer">
    <div id="formSection">
        <h2>Kirim Kartu Ucapan 🎄</h2>
        <p style="margin-bottom: 15px; font-size: 0.9rem;">Isi formulir di bawah untuk membuat ucapan Natal.</p>
        
        <form id="waForm">
            <input type="text" id="nama" placeholder="Nama Anda" required>
            <textarea id="pesan" rows="4" placeholder="Tulis ucapan selamat Natal..." required></textarea>
            <button type="submit">Buat Ucapan ✨</button>
        </form>
    </div>

    <div id="confirmSection" style="display: none; padding: 10px;">
        <div style="font-size: 3rem; margin-bottom: 10px;">🎁</div>
        <h3 style="color: var(--red); margin-bottom: 10px;">Ucapan Siap Dikirim!</h3>
        <p style="font-size: 0.9rem; margin-bottom: 20px;">Klik tombol di bawah untuk mengirimkannya ke WhatsApp.</p>
        
        <button id="btnKirimWA" style="background: #25d366;">
            <span>Kirim ke WhatsApp Sekarang</span>
        </button>
        <p onclick="location.reload()" style="margin-top: 15px; font-size: 0.8rem; color: #888; cursor: pointer; text-decoration: underline;">Batal / Tulis Ulang</p>
    </div>
</div>

<script>
    const NOMOR_WA = "6285785218016"; 

    // 1. COUNTDOWN (Tetap sama)
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

    // 2. WHATSAPP LOGIC (Dimodifikasi)
    let urlWhatsApp = "";

    document.getElementById('waForm').addEventListener('submit', function(e) {
        e.preventDefault();
        
        const nama = document.getElementById('nama').value;
        const pesan = document.getElementById('pesan').value;
        const teks = `Halo! 🎄%0A%0ANama: *${nama}*%0AUcapan: _${pesan}_%0A%0ASelamat Natal! ✨`;
        
        // Simpan URL ke variabel global
        urlWhatsApp = `https://api.whatsapp.com/send?phone=${NOMOR_WA}&text=${teks}`;
        
        // Sembunyikan form, tampilkan konfirmasi
        document.getElementById('formSection').style.display = 'none';
        document.getElementById('confirmSection').style.display = 'block';
    });

    // Eksekusi buka WhatsApp saat tombol konfirmasi diklik
    document.getElementById('btnKirimWA').addEventListener('click', function() {
        window.open(urlWhatsApp, '_blank');
    });

    // 3. SNOW EFFECT (Tetap sama)
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
