# Website-Refleksi
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Refleksi Pembelajaran</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <div class="container">
    <h1>Refleksi Pembelajaran</h1>
    <p class="subtitle">Isi dengan jujur ya 😊</p>

    <form id="refleksiForm">

      <label>Nama</label>
      <input type="text" name="nama" required>

      <label>No Absen</label>
      <input type="number" name="absen" required>

      <label>
        Jika saya diminta untuk mempresentasikan materi hari ini kepada teman saya, bagian materi apa yang akan saya jelaskan?
      </label>
      <textarea name="bagian_dipahami" rows="4" required></textarea>

      <label>
        Bagian mana yang masih sulit saya pahami?
      </label>
      <textarea name="kesulitan" rows="4" required></textarea>

      <label>
        Komitmen apa yang akan saya lakukan untuk mengatasi hal tersebut?
      </label>
      <textarea name="komitmen" rows="4" required></textarea>

      <button type="submit">Kirim Jawaban</button>

      <p id="status"></p>

    </form>
  </div>

  <script>
    const form = document.getElementById('refleksiForm');
    const status = document.getElementById('status');

    form.addEventListener('submit', function(e) {
      e.preventDefault();

      const data = new FormData(form);

      fetch("YOUR_GOOGLE_SCRIPT_URL", {
        method: "POST",
        body: data
      })
      .then(response => {
        status.innerText = "Jawaban berhasil dikirim!";
        form.reset();
      })
      .catch(error => {
        status.innerText = "Terjadi kesalahan, coba lagi.";
      });
    });
  </script>

</body>
</html>
