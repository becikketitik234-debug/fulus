# fulus
Website simulasi target tabungan
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fulus - Target Tabungan</title>
  <meta name="description" content="Hitung target tabungan harian, mingguan, dan bulanan dengan Fulus.">
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f3f7f5;
      color: #18352b;
    }

    header {
      background: #087f5b;
      color: white;
      padding: 25px 20px;
      text-align: center;
    }

    header h1 {
      margin: 0;
      font-size: 32px;
    }

    header p {
      margin-bottom: 0;
      color: #e1fff2;
    }

    main {
      max-width: 650px;
      margin: 25px auto;
      padding: 0 15px;
    }

    .card {
      background: white;
      padding: 22px;
      border-radius: 16px;
      margin-bottom: 18px;
      box-shadow: 0 4px 15px #0000000d;
    }

    h2 {
      margin-top: 0;
      font-size: 21px;
    }

    label {
      display: block;
      margin: 15px 0 7px;
      font-weight: bold;
    }

    input {
      width: 100%;
      padding: 13px;
      border: 1px solid #ccd8d2;
      border-radius: 9px;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 14px;
      margin-top: 18px;
      border: none;
      border-radius: 9px;
      background: #087f5b;
      color: white;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover {
      background: #066747;
    }

    .reset {
      background: #e8f2ed;
      color: #087f5b;
    }

    .result {
      background: #e7f8ef;
      padding: 15px;
      margin-top: 12px;
      border-radius: 10px;
    }

    .result strong {
      display: block;
      font-size: 23px;
      color: #087f5b;
      margin-top: 5px;
    }

    .progress-bg {
      background: #dce9e1;
      height: 13px;
      border-radius: 20px;
      overflow: hidden;
      margin: 12px 0;
    }

    .progress-bar {
      height: 100%;
      width: 0%;
      background: #20a66a;
      transition: width 0.3s;
    }

    .note {
      font-size: 13px;
      color: #60736a;
      line-height: 1.6;
    }

    footer {
      text-align: center;
      padding: 20px;
      color: #60736a;
      font-size: 13px;
    }
  </style>
</head>

<body>
  <header>
    <h1>🌿 Fulus</h1>
    <p>Rencanakan tabunganmu, capai tujuanmu.</p>
  </header>

  <main>
    <section class="card">
      <h2>🎯 Target Tabungan</h2>

      <label for="goal">Target uang (Rp)</label>
      <input id="goal" type="number" min="1"
        placeholder="Contoh: 5000000" value="5000000">

      <label for="saved">Tabungan saat ini (Rp)</label>
      <input id="saved" type="number" min="0"
        placeholder="Contoh: 500000" value="500000">

      <label for="deadline">Target tercapai pada tanggal</label>
      <input id="deadline" type="date">

      <button onclick="calculate()">Hitung Target Tabungan</button>
      <button class="reset" onclick="resetForm()">Atur Ulang</button>
    </section>

    <section class="card">
      <h2>📊 Progres Tabungan</h2>

      <div id="progressText">Masukkan target tabunganmu.</div>
      <div class="progress-bg">
        <div class="progress-bar" id="progressBar"></div>
      </div>

      <div class="result">
        Sisa uang yang perlu dikumpulkan
        <strong id="remaining">Rp0</strong>
      </div>

      <div class="result">
        Target menabung per hari
        <strong id="daily">Rp0</strong>
      </div>

      <div class="result">
        Target menabung per minggu
        <strong id="weekly">Rp0</strong>
      </div>

      <div class="result">
        Target menabung per bulan
        <strong id="monthly">Rp0</strong>
      </div>

      <p class="note" id="message">
        Hasil perhitungan akan muncul di sini.
      </p>
    </section>

    <section class="card">
      <h2>💡 Tips Menabung</h2>
      <ul>
        <li>Tentukan tujuan tabungan yang jelas.</li>
        <li>Sisihkan uang secara rutin.</li>
        <li>Catat setiap uang yang berhasil ditabung.</li>
        <li>Hindari pengeluaran yang tidak diperlukan.</li>
      </ul>
    </section>

    <section class="card">
      <h2>Tentang Fulus</h2>
      <p>
        Fulus adalah alat bantu sederhana untuk merencanakan
        target tabungan. Perhitungan ini merupakan perkiraan,
        bukan jaminan hasil keuangan.
      </p>
    </section>
  </main>

  <footer>
    © 2026 Fulus · Alat bantu perencanaan tabungan
  </footer>

  <script>
    const rupiah = angka =>
      new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
        maximumFractionDigits: 0
      }).format(angka);

    function calculate() {
      const goal = Number(document.getElementById("goal").value);
      const saved = Number(document.getElementById("saved").value);
      const deadlineValue = document.getElementById("deadline").value;

      if (!Number.isFinite(goal) || goal <= 0 ||
          !Number.isFinite(saved) || saved < 0 ||
          !deadlineValue) {
        alert("Isi target, tabungan saat ini, dan tanggal target dengan benar.");
        return;
      }

      const today = new Date();
      today.setHours(0, 0, 0, 0);

      const deadline = new Date(deadlineValue + "T00:00:00");
      const remainingDays = Math.ceil(
        (deadline - today) / (1000 * 60 * 60 * 24)
      );

      const remaining = Math.max(0, goal - saved);
      const progress = Math.min(100, (saved / goal) * 100);

      document.getElementById("progressText").textContent =
        "Progres: " + progress.toFixed(1) + "%";

      document.getElementById("progressBar").style.width =
        progress + "%";

      document.getElementById("remaining").textContent =
        rupiah(remaining);

      if (remaining === 0) {
        document.getElementById("daily").textContent = rupiah(0);
        document.getElementById("weekly").textContent = rupiah(0);
        document.getElementById("monthly").textContent = rupiah(0);
        document.getElementById("message").textContent =
          "Selamat! Target tabunganmu sudah tercapai.";
        return;
      }

      if (remainingDays <= 0) {
        document.getElementById("daily").textContent = "—";
        document.getElementById("weekly").textContent = "—";
        document.getElementById("monthly").textContent = "—";
        document.getElementById("message").textContent =
          "Tanggal target sudah tiba atau terlewat. Pilih tanggal mendatang.";
        return;
      }

      const daily = Math.ceil(remaining / remainingDays);
      const weekly = Math.ceil(daily * 7);
      const monthly = Math.ceil(daily * 30);

      document.getElementById("daily").textContent = rupiah(daily);
      document.getElementById("weekly").textContent = rupiah(weekly);
      document.getElementById("monthly").textContent = rupiah(monthly);

      document.getElementById("message").textContent =
        "Kamu memiliki " + remainingDays +
        " hari untuk mengumpulkan sisa tabungan. Angka mingguan " +
        "dan bulanan merupakan perkiraan berdasarkan target harian.";
    }

    function resetForm() {
      document.getElementById("goal").value = "";
      document.getElementById("saved").value = "";
      document.getElementById("deadline").value = "";
      document.getElementById("progressText").textContent =
        "Masukkan target tabunganmu.";
      document.getElementById("progressBar").style.width = "0%";
      document.getElementById("remaining").textContent = rupiah(0);
      document.getElementById("daily").textContent = rupiah(0);
      document.getElementById("weekly").textContent = rupiah(0);
      document.getElementById("monthly").textContent = rupiah(0);
      document.getElementById("message").textContent =
        "Hasil perhitungan akan muncul di sini.";
    }

    const today = new Date();
    today.setMinutes(today.getMinutes() - today.getTimezoneOffset());
    document.getElementById("deadline").min =
      today.toISOString().slice(0, 10);
  </script>
</body>
</html>
```
