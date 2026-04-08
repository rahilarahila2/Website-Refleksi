<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Refleksi Pembelajaran</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="wrapper">

  <div class="progress">
    <div id="bar"></div>
  </div>

  <form id="form">

    <!-- PAGE 1 -->
    <section class="step active">
      <h2>Siapa namamu?</h2>
      <input type="text" name="nama" placeholder="Ketik di sini..." required autofocus>

      <h2>Nomor absen?</h2>
      <input type="number" name="absen" placeholder="Contoh: 12" required>

      <button type="button" onclick="next()">Lanjut →</button>
    </section>

    <!-- PAGE 2 -->
    <section class="step">
      <h2>
        Jika kamu harus menjelaskan ke temanmu, bagian apa yang akan kamu jelaskan?
      </h2>
      <textarea name="bagian_dipahami" placeholder="Tulis jawabanmu..." required></textarea>

      <div class="nav">
        <button type="button" onclick="prev()">←</button>
        <button type="button" onclick="next()">→</button>
      </div>
    </section>

    <!-- PAGE 3 -->
    <section class="step">
      <h2>
        Bagian mana yang masih sulit kamu pahami?
      </h2>
      <textarea name="kesulitan" placeholder="Tulis di sini..." required></textarea>

      <div class="nav">
        <button type="button" onclick="prev()">←</button>
        <button type="button" onclick="next()">→</button>
      </div>
    </section>

    <!-- PAGE 4 -->
    <section class="step">
      <h2>
        Apa komitmenmu untuk mengatasinya?
      </h2>
      <textarea name="komitmen" placeholder="Contoh: saya akan latihan soal..." required></textarea>

      <div class="nav">
        <button type="button" onclick="prev()">←</button>
        <button type="submit">Kirim ✔</button>
      </div>
    </section>

  </form>

  <p id="status"></p>

</div>

<script>
let step = 0;
const steps = document.querySelectorAll(".step");
const bar = document.getElementById("bar");

function showStep(i) {
  steps.forEach(s => s.classList.remove("active"));
  steps[i].classList.add("active");

  let progress = ((i + 1) / steps.length) * 100;
  bar.style.width = progress + "%";

  // autofocus otomatis
  const input = steps[i].querySelector("input, textarea");
  if (input) input.focus();
}

function validateStep() {
  const inputs = steps[step].querySelectorAll("input, textarea");
  for (let input of inputs) {
    if (!input.value.trim()) {
      input.style.border = "2px solid red";
      return false;
    } else {
      input.style.border = "1px solid #ccc";
    }
  }
  return true;
}

function next() {
  if (!validateStep()) return;

  if (step < steps.length - 1) {
    step++;
    showStep(step);
  }
}

function prev() {
  if (step > 0) {
    step--;
    showStep(step);
  }
}

// Enter untuk lanjut
document.addEventListener("keydown", function(e) {
  if (e.key === "Enter" && step < steps.length - 1) {
    e.preventDefault();
    next();
  }
});

// submit
const form = document.getElementById("form");
const status = document.getElementById("status");

form.addEventListener("submit", function(e) {
  e.preventDefault();

  if (!validateStep()) return;

  const data = new FormData(form);

  fetch("YOUR_GOOGLE_SCRIPT_URL", {
    method: "POST",
    body: data
  })
  .then(() => {
    status.innerText = "Jawaban terkirim 🎉";
    form.reset();
    step = 0;
    showStep(step);
  })
  .catch(() => {
    status.innerText = "Gagal mengirim.";
  });
});

showStep(step);
</script>

</body>
</html>
