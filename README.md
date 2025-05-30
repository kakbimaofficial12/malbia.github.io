<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>MALBIA.Id</title>
  <link href="https://fonts.googleapis.com/css2?family=Caitlin&display=swap" rel="stylesheet" />
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRk4gtQRKkjZ7HvSStjbRowGTrQJowRNGxjpg&usqp=CAU') no-repeat center center fixed;
      background-size: cover;
      color: #000000;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      min-height: 100vh;
      padding: 20px;
      position: relative;
      z-index: 1;
    }
    body::before {
      content: "";
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      z-index: -1;
    }

    /* Header & Tanggal */
    #datetime {
      font-size: 18px;
      margin-bottom: 10px;
      font-weight: 600;
      color: white;
      user-select: none;
    }

    h1 {
      font-family: 'Caitlin', cursive;
      font-size: 50px;
      margin-top: 0;
      margin-bottom: 0;
      color: white;
    }

    h4 {
      color: #dddddd;
      margin-top: 0;
      margin-bottom: 20px;
    }

    /* Form Pencarian */
    form {
      width: 90%;
      max-width: 600px;
      margin-bottom: 20px;
    }

    .search-box {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    input[type="text"] {
      padding: 10px 16px;
      border: 1px solid #ccc;
      border-radius: 20px;
      font-size: 16px;
      color: #000;
    }

    input[type="text"]::placeholder {
      color: #999;
    }

    .button-group {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    button {
      padding: 10px 20px;
      border: none;
      border-radius: 20px;
      background-color: #1a73e8;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }

    button:hover {
      background-color: #155ab6;
    }

    #loading {
      font-size: 16px;
      color: #fff;
      display: none;
      margin-top: 10px;
    }

    /* Jawaban Ringkas */
    #answerBox {
      margin-top: 20px;
      width: 90%;
      max-width: 800px;
      text-align: left;
    }

    .quick-answer {
      background: #e8f0fe;
      border-left: 4px solid #1a73e8;
      padding: 15px 20px;
      margin-bottom: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.08);
    }

    .quick-answer p {
      font-size: 16px;
      margin: 8px 0;
      color: #222;
    }

    .quick-answer small a {
      color: #1a0dab;
      text-decoration: none;
    }

    .quick-answer small a:hover {
      text-decoration: underline;
    }

    /* Hasil Pencarian */
    #results {
      width: 90%;
      max-width: 800px;
      margin-bottom: 100px;
      text-align: left;
    }

    .result-item {
      margin-bottom: 20px;
      border-bottom: 1px solid #ddd;
      padding-bottom: 15px;
      background-color: rgba(255, 255, 255, 0.85);
      border-radius: 12px;
      padding: 10px;
    }

    .result-item img {
      max-width: 100%;
      height: auto;
      border-radius: 12px;
      margin-bottom: 10px;
    }

    .result-item a {
      font-size: 20px;
      color: #1a0dab;
      text-decoration: none;
      display: block;
      margin-bottom: 6px;
    }

    .result-item a:hover {
      text-decoration: underline;
    }

    .result-item p {
      color: #444444;
      font-size: 15px;
      margin: 0;
    }

    .result-item small {
      color: #888;
    }
  </style>
</head>
<body>

  <div id="datetime"></div>

  <h1>MALBIA.Id</h1>
  <h4>Program Bima Technology Indonesia</h4>

  <form id="searchForm">
    <div class="search-box">
      <input type="text" id="searchInput" placeholder="Telusuri di MALBIA.Id" autocomplete="off" />
      <div class="button-group">
        <button type="submit">Telusuri</button>
        <button type="button" id="imageSearchBtn">Gambar</button>
      </div>
    </div>
  </form>

  <div id="loading">Sedang mencari hasil...</div>
  <div id="answerBox"></div>
  <div id="results"></div>

  <script>
    function updateDateTime() {
      const dtElem = document.getElementById('datetime');
      const now = new Date();
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
      const dateStr = now.toLocaleDateString('id-ID', options);
      const timeStr = now.toLocaleTimeString('id-ID');
      dtElem.textContent = `${dateStr} - ${timeStr}`;
    }
    setInterval(updateDateTime, 1000);
    updateDateTime();

    const apiKey = 'AIzaSyA_QPYOcw-jdXzxNSmuCUaYHy6vA_jQQvE'; // Ganti jika perlu
    const cx = 'b68dbe815b66048af';

    const searchForm = document.getElementById('searchForm');
    const searchInput = document.getElementById('searchInput');
    const loading = document.getElementById('loading');
    const answerBox = document.getElementById('answerBox');
    const resultsDiv = document.getElementById('results');

    const formulaAnswers = {
      "rumus frekuensi": "f = λ × v (frekuensi = panjang gelombang × kecepatan)",
      "rumus kecepatan": "v = s / t (kecepatan = jarak / waktu)",
      "rumus gaya": "F = m × a (gaya = massa × percepatan)",
      "rumus tekanan": "P = F / A (tekanan = gaya / luas)",
      "rumus massa jenis": "ρ = m / V (massa jenis = massa / volume)",
      "bapak proklamator": "Ir. Soekarno adalah presiden pertama Indonesia sekaligus dikenal sebagai bapak proklamator kemerdekaan Indonesia.",
      "penemu malbia": "Penemu Malbia.Id adalah Bima Wahyu Pratama, pelajar asal Indonesia yang menciptakan search engine ini."
    };

    function checkFormulaAnswer(query) {
      const lowerQuery = query.toLowerCase();
      for (const key in formulaAnswers) {
        if (lowerQuery.includes(key)) {
          return formulaAnswers[key];
        }
      }
      return null;
    }

    function performSearch(query, type = 'web') {
      if (!query) return;

      loading.style.display = 'block';
      resultsDiv.innerHTML = '';
      answerBox.innerHTML = '';

      const formula = checkFormulaAnswer(query);
      if (formula) {
        answerBox.innerHTML = `
          <div class="quick-answer">
            <strong>📌 Jawaban Ringkas</strong>
            <p>${formula}</p>
            <small>Sumber: MALBIA.Id</small>
          </div>
        `;
      }

      let searchUrl = `https://www.googleapis.com/customsearch/v1?key=${apiKey}&cx=${cx}&q=${encodeURIComponent(query)}`;
      if (type === 'image') {
        searchUrl += `&searchType=image`;
      }

      fetch(searchUrl)
        .then(res => res.json())
        .then(data => {
          loading.style.display = 'none';

          if (data.items && data.items.length > 0) {
            if (!formula && type === 'web') {
              const topThree = data.items.slice(0, 3);
              answerBox.innerHTML = `
                ${topThree.map(item =>
                  `<div class="quick-answer">
                    <p>${item.snippet}</p>
                    <small>Sumber: <a href="${item.link}" target="_blank">${item.displayLink}</a></small>
                  </div>`).join('')}
              `;
            }

            data.items.forEach(item => {
              if (type === 'image') {
                resultsDiv.innerHTML += `
                  <div class="result-item">
                    <img src="${item.link}" alt="Gambar">
                    <a href="${item.image?.contextLink || item.link}" target="_blank">${item.title}</a>
                  </div>
                `;
              } else {
                resultsDiv.innerHTML += `
                  <div class="result-item">
                    <a href="${item.link}" target="_blank">${item.title}</a>
                    <p>${item.snippet || ''}</p>
                    <small>Sumber: ${item.displayLink}</small>
                  </div>
                `;
              }
            });

          } else {
            resultsDiv.innerHTML = '<p>Tidak ada hasil ditemukan.</p>';
          }
        })
        .catch(err => {
          loading.style.display = 'none';
          resultsDiv.innerHTML = '<p>Terjadi kesalahan saat memuat hasil.</p>';
          console.error(err);
        });
    }

    searchForm.addEventListener('submit', e => {
      e.preventDefault();
      const query = searchInput.value.trim();
      performSearch(query, 'web');
    });

    document.getElementById('imageSearchBtn').addEventListener('click', () => {
      const query = searchInput.value.trim();
      performSearch(query, 'image');
    });
  </script>
</body>
</html>
