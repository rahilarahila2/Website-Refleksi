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
  <div class="progress">
    <div id="progress-bar"></div>
  </div>

  <form id="form">

    <!-- PAGE 1 -->
    <div class="page active">
      <h2>Identitas</h2>

      <label>Nama</label>
      <input type="text" name="nama" required>

      <label>No Absen</label>
      <input type="number" name="absen" required>

      <button type="button" onclick="nextPage()">Lanjut</button>
    </div>

    <!-- PAGE 2 -->
    <div class="page">
      <h2>Pemahaman</h2>

      <label>
        Jika saya diminta untuk mempresentasikan materi hari ini kepada teman saya, bagian materi apa yang akan saya jelaskan?
      </label>
      <textarea name="bagian_dipahami" required></textarea>

      <div class="nav">
        <button type="button" onclick="prevPage()">Kembali</button>
        <button type="button" onclick="nextPage()">Lanjut</button>
      </div>
    </div>

    <!-- PAGE 3 -->
    <div class="page">
      <h2>Kesulitan</h2>

      <label>
        Bagian mana yang masih sulit saya pahami?
      </label>
      <textarea name="kesulitan" required></textarea>

      <div class="nav">
        <button type="button" onclick="prevPage()">Kembali</button>
        <button type="button" onclick="nextPage()">Lanjut</button>
      </div>
    </div>

    <!-- PAGE 4 -->
    <div class="page">
      <h2>Komitmen</h2>

      <label>
        Komitmen apa yang akan saya lakukan untuk mengatasi hal tersebut?
      </label>
      <textarea name="komitmen" required></textarea>

      <div class="nav">
        <button type="button" onclick="prevPage()">Kembali</button>
        <button type="submit">Kirim</button>
      </div>
    </div>

  </form>

  <p id="status"></p>
</div>

<script>
  let currentPage = 0;
  const pages = document.querySelectorAll(".page");
  const progressBar = document.getElementById("progress-bar");

  function showPage(index) {
    pages.forEach(p => p.classList.remove("active"));
    pages[index].classList.add("active");

    let progress = ((index + 1) / pages.length) * 100;
    progressBar.style.width = progress + "%";
  }

  function nextPage() {
    if (currentPage < pages.length - 1) {
      currentPage++;
      showPage(currentPage);
    }
  }

  function prevPage() {
    if (currentPage > 0) {
      currentPage--;
      showPage(currentPage);
    }
  }

  // Submit ke Google Sheets
  const form = document.getElementById("form");
  const status = document.getElementById("status");

  form.addEventListener("submit", function(e) {
    e.preventDefault();

    const data = new FormData(form);

    fetch("YOUR_GOOGLE_SCRIPT_URL", {
      method: "POST",
      body: data
    })
    .then(() => {
      status.innerText = "Jawaban berhasil dikirim!";
      form.reset();
      currentPage = 0;
      showPage(currentPage);
    })
    .catch(() => {
      status.innerText = "Terjadi kesalahan.";
    });
  });

  showPage(currentPage);
</script>

</body>
</html>

body {
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg, #6dd5ed, #2193b0);
  margin: 0;
}

.container {
  max-width: 600px;
  background: white;
  margin: 50px auto;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.2);
}

h1 {
  text-align: center;
}

h2 {
  margin-bottom: 10px;
}

.page {
  display: none;
}

.page.active {
  display: block;
}

label {
  display: block;
  margin-top: 15px;
  font-weight: 600;
}

input, textarea {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

textarea {
  height: 120px;
}

button {
  margin-top: 20px;
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  background: #2193b0;
  color: white;
  cursor: pointer;
}

button:hover {
  background: #176d82;
}

.nav {
  display: flex;
  justify-content: space-between;
}

.progress {
  width: 100%;
  height: 8px;
  background: #eee;
  border-radius: 10px;
  margin: 20px 0;
}

#progress-bar {
  height: 100%;
  width: 0%;
  background: #2193b0;
  border-radius: 10px;
  transition: 0.3s;
}

#status {
  text-align: center;
  margin-top: 15px;
  font-weight: bold;
}
