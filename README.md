</head>
<body>

  <h1>🚀 Baileys WhatsApp API - By oozvc & AR.code</h1>
  <p><strong>Library WhatsApp Web berbasis Node.js</strong> tanpa websocket tambahan. Modifikasi dari Whiskey Baileys yang lebih stabil dan support lebih banyak fitur.</p>
  <p>Didesain untuk <strong>bot, automation, dan integrasi WhatsApp</strong> dengan performa tinggi.</p>

  <div class="section">
    <h2>📌 Tentang Proyek Ini</h2>
    <p>Dikembangkan oleh <strong>ziole</strong> bersama para kontributor open-source. Komunitas sangat dihargai! 💖</p>
  </div>

  <div class="section">
    <h2>✨ Fitur Utama</h2>
    <ul>
      <li>Autentikasi tanpa QR menggunakan session</li>
      <li>Dukungan WhatsApp Multi-Device terbaru</li>
      <li>Kirim & terima pesan dalam berbagai format</li>
      <li>Kelola grup: tambah/kick anggota, deskripsi, dll.</li>
      <li>Integrasi event: join/left group, message read, dll.</li>
    </ul>
  </div>

  <div class="section">
    <h2>📦 Instalasi</h2>
    <p>Pastikan Node.js v14 atau lebih baru sudah terinstall.</p>
    <pre><code>npm install @oozvc/baileys</code></pre>
    <p>Atau dengan Yarn:</p>
    <pre><code>yarn add @oozvc/baileys</code></pre>
  </div>

  <div class="section">
    <h2>🚀 Penggunaan Dasar</h2>
    <p>Contoh pairing code tanpa scan QR:</p>
    <pre><code>const { useMultiFileAuthState, makeWASocket } = require('@oozvc/baileys');
const readline = require('readline');
const rl = readline.createInterface({ input: process.stdin, output: process.stdout });
const question = (q) => new Promise(res => rl.question(q, res));

async function start() {
  const { state, saveCreds } = await useMultiFileAuthState('./session');
  const usePairingCode = true;

  const conn = makeWASocket({ auth: state, printQRInTerminal: !usePairingCode });

  conn.ev.on('creds.update', saveCreds);

  if (usePairingCode && !conn.authState.creds.registered) {
    const phone = (await question('Enter your phone:\n')).replace(/\D/g, '');
    rl.close();
    const code = await conn.requestPairingCode(phone);
    console.log('Code:', code.match(/.{1,4}/g).join('-'));
  }

  conn.ev.on('connection.update', ({ connection }) => {
    if (connection === 'open') console.log('Connected!');
  });

  conn.ev.on('messages.upsert', async (m) => {
    const msg = m.messages[0];
    if (!msg.key.fromMe && msg.message) {
      await conn.sendMessage(msg.key.remoteJid, { text: 'Hello there!' });
    }
  });
}
start();</code></pre>
  </div>

  <div class="section">
    <h2>📜 Dokumentasi API</h2>
    <table>
      <thead>
        <tr>
          <th>Fitur</th>
          <th>Deskripsi</th>
        </tr>
      </thead>
      <tbody>
        <tr><td><code>sendMessage()</code></td><td>Mengirim teks, gambar, video, dll.</td></tr>
        <tr><td><code>updateProfile()</code></td><td>Update nama dan foto profil</td></tr>
        <tr><td><code>getChats()</code></td><td>Mendapatkan daftar chat</td></tr>
        <tr><td><code>groupParticipantsUpdate()</code></td><td>Tambah/hapus anggota grup</td></tr>
      </tbody>
    </table>
    <p>📖 Dokumentasi lengkap: <a href="#">Baileys API Docs</a></p>
  </div>

  <div class="section">
    <h2>📩 Contoh Kode Lainnya</h2>
    <p><strong>📌 Interaktif Message, Buttons, Card, Status Mention, dsb.</strong> Sudah disediakan di file asli. Silakan cek dokumentasi atau file <code>examples</code>.</p>
  </div>

  <div class="section">
    <h2>🤝 Kontribusi</h2>
    <p>Fork, pull request, dan ide kamu sangat diterima!</p>
    <ul>
      <li>Fork repo ini</li>
      <li>Buat branch baru</li>
      <li>Ajukan Pull Request (PR)</li>
      <li>💡 Punya ide? Buat Issue sekarang!</li>
    </ul>
  </div>

</body>
</html>
