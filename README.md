<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Roda Keberuntungan</title>
  <style>
    body {
      text-align: center;
      font-family: sans-serif;
    }

    #wheel {
      width: 400px;
      height: 400px;
      border-radius: 50%;
      border: 8px solid #333;
      position: relative;
      margin: 30px auto;
      overflow: hidden;
      transform: rotate(0deg);
      transition: transform 4s ease-out;
    }

    .sector {
      position: absolute;
      width: 50%;
      height: 50%;
      top: 50%;
      left: 50%;
      transform-origin: 0% 0%;
      background: #ccc;
      clip-path: polygon(0% 0%, 100% 0%, 0% 100%);
      display: flex;
      justify-content: center;
      align-items: center;
      font-weight: bold;
      color: white;
    }

    #pointer {
      width: 0; 
      height: 0; 
      border-left: 20px solid transparent;
      border-right: 20px solid transparent;
      border-bottom: 30px solid red;
      margin: auto;
      position: relative;
      top: -10px;
    }

    #result {
      font-size: 24px;
      margin-top: 20px;
    }

    #spinBtn {
      padding: 10px 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <div id="pointer"></div>
  <div id="wheel"></div>
  <button id="spinBtn">Putar!</button>
  <div id="result"></div>

  <script>
    const sectors = [
      "Hadiah A",
      "Hadiah B",
      "Hadiah Super Langka", // Tidak bisa dimenangkan
      "Hadiah C",
      "Hadiah D",
      "Hadiah E"
    ];

    const colors = ["#e74c3c", "#3498db", "#9b59b6", "#2ecc71", "#f1c40f", "#1abc9c"];
    const fakePrizeIndex = sectors.indexOf("Hadiah Super Langka");
    const wheel = document.getElementById("wheel");
    const result = document.getElementById("result");
    const spinBtn = document.getElementById("spinBtn");
    let deg = 0;

    // Buat sektor roda
    const angle = 360 / sectors.length;
    sectors.forEach((label, i) => {
      const sector = document.createElement("div");
      sector.className = "sector";
      sector.style.background = colors[i % colors.length];
      sector.style.transform = `rotate(${i * angle}deg) skewY(-${90 - angle}deg)`;
      sector.innerHTML = `<div style="transform: skewY(${90 - angle}deg) rotate(${angle / 2}deg);">${label}</div>`;
      wheel.appendChild(sector);
    });

    spinBtn.addEventListener("click", () => {
      spinBtn.disabled = true;
      result.textContent = "Memutar...";
      const spins = 5;
      const extraDeg = Math.floor(Math.random() * 360);
      deg += spins * 360 + extraDeg;
      wheel.style.transform = `rotate(${deg}deg)`;

      setTimeout(() => {
        const actualDeg = deg % 360;
        let selected = Math.floor((sectors.length - (actualDeg / angle)) % sectors.length);

        // Hindari hadiah super langka
        if (selected === fakePrizeIndex) {
          selected = (selected + 1) % sectors.length;
        }

        result.textContent = `Selamat! Kamu mendapatkan: ${sectors[selected]}`;
        spinBtn.disabled = false;
      }, 4500);
    });
  </script>

</body>
</html>
