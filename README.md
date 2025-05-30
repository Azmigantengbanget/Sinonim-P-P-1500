<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Arduino Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [

  { "en": "Panel?", "id": "Papan." },
  { "en": "Panelis?", "id": "Anggota panel." },
  { "en": "Panembahan?", "id": "Gelar bangsawan Jawa." },
  { "en": "Panembrama?", "id": "Lagu pujian." },
  { "en": "Panen?", "id": "Petik hasil." },
  { "en": "Pang?", "id": "Suara (arkais)." },
  { "en": "Pangah?", "id": "Heran (arkais)." },
  { "en": "Pangan?", "id": "Makanan." },
  { "en": "Pangeran?", "id": "Putra raja." },
  { "en": "Pangestu?", "id": "Restu (Jawa)." },
  { "en": "Panggang?", "id": "Bakar." },
  { "en": "Panggar?", "id": "Para-para." },
  { "en": "Panggau?", "id": "Panggung." },
  { "en": "Pangggil?", "id": "Panggil." },
  { "en": "Panggul?", "id": "Pinggul." },
  { "en": "Panggung?", "id": "Pentas." },
  { "en": "Pangkah?", "id": "Sanggah (arkais)." },
  { "en": "Pangkai?", "id": "Ujung (arkais)." },
  { "en": "Pangkal?", "id": "Dasar." },
  { "en": "Pangkalan?", "id": "Basis." },
  { "en": "Pangkar?", "id": "Akar." },
  { "en": "Pangkas?", "id": "Potong." },
  { "en": "Pangkat?", "id": "Kedudukan." },
  { "en": "Pangking?", "id": "Kamar tidur (Tionghoa)." },
  { "en": "Pangkon?", "id": "Pangkuan (Jawa)." },
  { "en": "Pangku?", "id": "Dukung." },
  { "en": "Pangkung?", "id": "Pukul." },
  { "en": "Pangkur?", "id": "Tembang Jawa." },
  { "en": "Panglima?", "id": "Komandan." },
  { "en": "Pangling?", "id": "Tidak kenal lagi." },
  { "en": "Panglong?", "id": "Tempat penebangan kayu." },
  { "en": "Pangonan?", "id": "Padang rumput (Jawa)." },
  { "en": "Pangreh praja?", "id": "Pamong praja." },
  { "en": "Pangsa?", "id": "Bagian." },
  { "en": "Pangsit?", "id": "Makanan." },
  { "en": "Pangsi?", "id": "Pakaian." },
  { "en": "Pangu?", "id": "Dewa pencipta (Tionghoa)." },
  { "en": "Pangur?", "id": "Kikir." },
  { "en": "Pangus?", "id": "Cekatan." },
  { "en": "Panik?", "id": "Gugup." },
  { "en": "Paniki?", "id": "Kelelawar." },
  { "en": "Panil?", "id": "Panel." },
  { "en": "Panir?", "id": "Tepung roti." },
  { "en": "Paniradia?", "id": "Bagian keraton." },
  { "en": "Panitera?", "id": "Juru tulis pengadilan." },
  { "en": "Panitia?", "id": "Penyelenggara." },
  { "en": "Panjang?", "id": "Lama." },
  { "en": "Panjar?", "id": "Uang muka." },
  { "en": "Panjat?", "id": "Naik." },
  { "en": "Panji?", "id": "Bendera." },
  { "en": "Panjing?", "id": "Masuk (arkais)." },
  { "en": "Panjul?", "id": "Tonjol (arkais)." },
  { "en": "Panorama?", "id": "Pemandangan." },
  { "en": "Panri?", "id": "Sakit (Bugis)." },
  { "en": "Panser?", "id": "Kendaraan lapis baja." },
  { "en": "Pantak?", "id": "Pasti (arkais)." },
  { "en": "Pantalon?", "id": "Celana." },
  { "en": "Pantang?", "id": "Larangan." },
  { "en": "Pantar?", "id": "Sebaya." },
  { "en": "Pantas?", "id": "Layak." },
  { "en": "Pantat?", "id": "Bokong." },
  { "en": "Pantau?", "id": "Awasi." },
  { "en": "Panteis?", "id": "Penganut panteisme." },
  { "en": "Panteisme?", "id": "Paham serba Tuhan." },
  { "en": "Pantek?", "id": "Pasak." },
  { "en": "Pantekosta?", "id": "Hari raya Kristen." },
  { "en": "Panten?", "id": "Pantun (arkais)." },
  { "en": "Panter?", "id": "Macan tutul." },
  { "en": "Panti?", "id": "Rumah." },
  { "en": "Pantik?", "id": "Nyalakan api." },
  { "en": "Panting?", "id": "Penting (arkais)." },
  { "en": "Pantis?", "id": "Celak alis." },
  { "en": "Panto?", "id": "Mimpi (arkais)." },
  { "en": "Pantofel?", "id": "Sepatu." },
  { "en": "Pantograf?", "id": "Alat jiplak peta." },
  { "en": "Pantomim?", "id": "Drama isyarat." },
  { "en": "Pantri?", "id": "Ruang simpan makanan." },
  { "en": "Pantul?", "id": "Memantul." },
  { "en": "Pantun?", "id": "Puisi lama." },
  { "en": "Panu?", "id": "Penyakit kulit." },
  { "en": "Panus?", "id": "Lapisan mata." },
  { "en": "Panuti?", "id": "Contoh (Jawa)." },
  { "en": "Papa?", "id": "Ayah." },
  { "en": "Papah?", "id": "Pimpin." },
  { "en": "Papak?", "id": "Datar." },
  { "en": "Papan?", "id": "Lembaran kayu." },
  { "en": "Papar?", "id": "Urai." },
  { "en": "Paparazi?", "id": "Fotografer pemburu berita." },
  { "en": "Papas?", "id": "Buang." },
  { "en": "Papi?", "id": "Ayah." },
  { "en": "Papil?", "id": "Tonjolan kecil." },
  { "en": "Papila?", "id": "Papil." },
  { "en": "Papiloma?", "id": "Tumor." },
  { "en": "Papirus?", "id": "Kertas kuno." },
  { "en": "Papras?", "id": "Cukur." },
  { "en": "Paprika?", "id": "Cabai." },
  { "en": "Papui?", "id": "Penyakit." },
  { "en": "Para?", "id": "Awalan jamak." },
  { "en": "Para-para?", "id": "Rak." },
  { "en": "Parabola?", "id": "Antena." },
  { "en": "Parade?", "id": "Pawai." },
  { "en": "Paradigma?", "id": "Kerangka pikir." },
  { "en": "Paradoks?", "id": "Pertentangan." },
  { "en": "Paradoksal?", "id": "Bersifat paradoks." },
  { "en": "Paraf?", "id": "Tanda tangan singkat." },
  { "en": "Parafemia?", "id": "Salah ucap." },
  { "en": "Parafin?", "id": "Lilin." },
  { "en": "Parafrasa?", "id": "Uraian kembali." },
  { "en": "Paragog?", "id": "Penambahan bunyi akhir." },
  { "en": "Paragon?", "id": "Model sempurna." },
  { "en": "Paragraf?", "id": "Alinea." },
  { "en": "Parah?", "id": "Berat." },
  { "en": "Parak?", "id": "Beda." },
  { "en": "Parakansalak?", "id": "Daerah." },
  { "en": "Paralaks?", "id": "Pergeseran semu." },
  { "en": "Paraldehida?", "id": "Obat tidur." },
  { "en": "Paralel?", "id": "Sejajar." },
  { "en": "Paralelisme?", "id": "Kesejajaran." },
  { "en": "Paralelogram?", "id": "Jajargenjang." },
  { "en": "Paralinguistik?", "id": "Unsur bahasa nonverbal." },
  { "en": "Paralipsis?", "id": "Gaya bahasa." },
  { "en": "Paralisis?", "id": "Kelumpuhan." },
  { "en": "Paralitik?", "id": "Lumpuh." },
  { "en": "Param?", "id": "Bedak obat." },
  { "en": "Paramaarta?", "id": "Kebijaksanaan tertinggi." },
  { "en": "Paramasastra?", "id": "Tata bahasa." },
  { "en": "Paramedis?", "id": "Tenaga medis." },
  { "en": "Parameter?", "id": "Ukuran." },
  { "en": "Paramita?", "id": "Kesempurnaan (Sanskerta)." },
  { "en": "Paran?", "id": "Arah." },
  { "en": "Parang?", "id": "Golok." },
  { "en": "Paranormal?", "id": "Gaib." },
  { "en": "Paranoya?", "id": "Gangguan jiwa." },
  { "en": "Paras?", "id": "Wajah." },
  { "en": "Parasit?", "id": "Benalu." },
  { "en": "Parasitisme?", "id": "Hubungan parasit." },
  { "en": "Parasitoid?", "id": "Serangga." },
  { "en": "Parasitologi?", "id": "Ilmu parasit." },
  { "en": "Parasut?", "id": "Payung udara." },
  { "en": "Parataksis?", "id": "Hubungan kalimat setara." },
  { "en": "Parataktis?", "id": "Bersifat parataksis." },
  { "en": "Paratiroid?", "id": "Kelenjar." },
  { "en": "Parau?", "id": "Serak." },
  { "en": "Pare?", "id": "Sayuran." },
  { "en": "Parenial?", "id": "Tahunan." },
  { "en": "Parenkim?", "id": "Jaringan dasar." },
  { "en": "Parentes?", "id": "Tanda kurung." },
  { "en": "Parentesis?", "id": "Parentes." },
  { "en": "Parestesia?", "id": "Kesemutan." },
  { "en": "Parfe?", "id": "Es krim (Prancis)." },
  { "en": "Parfum?", "id": "Minyak wangi." },
  { "en": "Pari?", "id": "Ikan." },
  { "en": "Paria?", "id": "Golongan rendah (India)." },
  { "en": "Paripurna?", "id": "Sempurna." },
  { "en": "Paris?", "id": "Kota." },
  { "en": "Parit?", "id": "Selokan." },
  { "en": "Paritas?", "id": "Persamaan." },
  { "en": "Pariwara?", "id": "Iklan." },
  { "en": "Pariwisata?", "id": "Wisata." },
  { "en": "Parka?", "id": "Jaket." },
  { "en": "Parket?", "id": "Lantai kayu." },
  { "en": "Parkinsionisme?", "id": "Penyakit saraf." },
  { "en": "Parkir?", "id": "Tempat henti kendaraan." },
  { "en": "Parlemen?", "id": "Dewan rakyat." },
  { "en": "Parlementaria?", "id": "Urusan parlemen." },
  { "en": "Parlementarisme?", "id": "Sistem parlemen." },
  { "en": "Parlemeter?", "id": "Parlemen." },
  { "en": "Parmen?", "id": "Kain (arkais)." },
  { "en": "Parmeswari?", "id": "Permaisuri (arkais)." },
  { "en": "Parnasi?", "id": "Gunung mitologi." },
  { "en": "Parodi?", "id": "Tiruan lucu." },
  { "en": "Parokial?", "id": "Sempit." },
  { "en": "Parokialisme?", "id": "Sikap sempit." },
  { "en": "Parolfaktori?", "id": "Area otak." },
  { "en": "Paron?", "id": "Landasan tempa." },
  { "en": "Paronim?", "id": "Kata mirip." },
  { "en": "Paronisia?", "id": "Radang kuku." },
  { "en": "Parotitis?", "id": "Gondongan." },
  { "en": "Parse?", "id": "Urai kalimat." },
  { "en": "Parsek?", "id": "Satuan jarak." },
  { "en": "Parsel?", "id": "Paket." },
  { "en": "Part?", "id": "Bagian (Inggris)." },
  { "en": "Partai?", "id": "Kelompok politik." },
  { "en": "Partenogenesis?", "id": "Reproduksi tanpa kawin." },
  { "en": "Partikel?", "id": "Butir." },
  { "en": "Partikelir?", "id": "Swasta." },
  { "en": "Partisan?", "id": "Pengikut." },
  { "en": "Partisi?", "id": "Sekat." },
  { "en": "Partisipan?", "id": "Peserta." },
  { "en": "Partisipasi?", "id": "Keikutsertaan." },
  { "en": "Partitif?", "id": "Menyatakan bagian." },
  { "en": "Partner?", "id": "Rekan." },
  { "en": "Partus?", "id": "Kelahiran." },
  { "en": "Paru?", "id": "Organ napas." },
  { "en": "Paruh?", "id": "Mulut burung." },
  { "en": "Parun?", "id": "Bakar (arkais)." },
  { "en": "Parut?", "id": "Kikis." },
  { "en": "Parvenu?", "id": "Orang kaya baru." },
  { "en": "Parwa?", "id": "Bagian Mahabharata." },
  { "en": "Pas?", "id": "Cocok." },
  { "en": "Pasa?", "id": "Pasar (arkais)." },
  { "en": "Pasah?", "id": "Alat ketam." },
  { "en": "Pasai?", "id": "Bosan." },
  { "en": "Pasak?", "id": "Paku kayu." },
  { "en": "Pasan?", "id": "Surat (arkais)." },
  { "en": "Pasang?", "id": "Naik (air laut)." },
  { "en": "Pasanggiri?", "id": "Perlombaan (Sunda)." },
  { "en": "Pasar?", "id": "Tempat jual beli." },
  { "en": "Pasara?", "id": "Makam." },
  { "en": "Pasaraya?", "id": "Toko besar." },
  { "en": "Pasase?", "id": "Lorong." },
  { "en": "Pasasir?", "id": "Penumpang (Belanda)." },
  { "en": "Pasat?", "id": "Angin." },
  { "en": "Pasek?", "id": "Paku (arkais)." },
  { "en": "Paseban?", "id": "Balai." },
  { "en": "Paser?", "id": "Anak panah." },
  { "en": "Paset?", "id": "Faset." },
  { "en": "Pasfoto?", "id": "Foto kecil." },
  { "en": "Pasi?", "id": "Pasir (arkais)." },
  { "en": "Pasien?", "id": "Orang sakit." },
  { "en": "Pasif?", "id": "Tidak aktif." },
  { "en": "Pasifikasi?", "id": "Pendamaian." },
  { "en": "Pasifis?", "id": "Pencinta damai." },
  { "en": "Pasifisme?", "id": "Paham damai." },
  { "en": "Pasigrafi?", "id": "Tulisan universal." },
  { "en": "Pasilan?", "id": "Pajak (arkais)." },
  { "en": "Pasim?", "id": "Pucat (arkais)." },
  { "en": "Pasir?", "id": "Butiran batu." },
  { "en": "Paska?", "id": "Sesudah." },
  { "en": "Paskah?", "id": "Hari raya Kristen." },
  { "en": "Paskin?", "id": "Surat tempel." },
  { "en": "Pasmat?", "id": "Sakit (arkais)." },
  { "en": "Pasmen?", "id": "Renda." },
  { "en": "Paso?", "id": "Tempayan." },
  { "en": "Pasok?", "id": "Suplai." },
  { "en": "Pasowan?", "id": "Tempat menghadap." },
  { "en": "Paspor?", "id": "Surat izin bepergian." },
  { "en": "Pasrah?", "id": "Menyerah." },
  { "en": "Pasta?", "id": "Makanan." },
  { "en": "Pastel?", "id": "Kue." },
  { "en": "Pasteurisasi?", "id": "Pemanasan." },
  { "en": "Pasti?", "id": "Tentu." },
  { "en": "Pastiles?", "id": "Permen." },
  { "en": "Pastis?", "id": "Minuman keras." },
  { "en": "Pastor?", "id": "Pendeta Katolik." },
  { "en": "Pastoral?", "id": "Pedesaan." },
  { "en": "Pastorale?", "id": "Musik pedesaan." },
  { "en": "Pastoran?", "id": "Rumah pastor." },
  { "en": "Pastri?", "id": "Kue kering." },
  { "en": "Pasu?", "id": "Wadah." },
  { "en": "Pasuel?", "id": "Tidak enak badan." },
  { "en": "Pasuk?", "id": "Pakai (arkais)." },
  { "en": "Pasukan?", "id": "Kelompok tentara." },
  { "en": "Pasumandan?", "id": "Pengiring pengantin." },
  { "en": "Patah?", "id": "Putus." },
  { "en": "Pataka?", "id": "Bendera." },
  { "en": "Patam?", "id": "Ikat kepala." },
  { "en": "Patang?", "id": "Cari (arkais)." },
  { "en": "Patar?", "id": "Kikir." },
  { "en": "Patas?", "id": "Cepat terbatas." },
  { "en": "Patek?", "id": "Penyakit kulit." },
  { "en": "Patel?", "id": "Tulang lutut." },
  { "en": "Patela?", "id": "Patel." },
  { "en": "Paten?", "id": "Hak cipta." },
  { "en": "Patera?", "id": "Daun." },
  { "en": "Paternal?", "id": "Kebapakan." },
  { "en": "Paternalis?", "id": "Bersifat kebapakan." },
  { "en": "Paternalisme?", "id": "Sistem kebapakan." },
  { "en": "Patet?", "id": "Tangga nada gamelan." },
  { "en": "Patetis?", "id": "Mengharukan." },
  { "en": "Pati?", "id": "Sari." },
  { "en": "Patih?", "id": "Perdana menteri." },
  { "en": "Patik?", "id": "Saya (hormat)." },
  { "en": "Patil?", "id": "Alat pancing." },
  { "en": "Patina?", "id": "Lapisan karat." },
  { "en": "Patin?", "id": "Ikan." },
  { "en": "Patir?", "id": "Tidak beragi." },
  { "en": "Patiseri?", "id": "Toko kue." },
  { "en": "Patlas?", "id": "Atlas (arkais)." },
  { "en": "Patogen?", "id": "Penyebab penyakit." },
  { "en": "Patogenesis?", "id": "Perkembangan penyakit." },
  { "en": "Patogenik?", "id": "Menyebabkan penyakit." },
  { "en": "Patok?", "id": "Pancang." },
  { "en": "Patola?", "id": "Kain sutra." },
  { "en": "Patolog?", "id": "Ahli penyakit." },
  { "en": "Patologi?", "id": "Ilmu penyakit." },
  { "en": "Patologis?", "id": "Berkaitan penyakit." },
  { "en": "Patri?", "id": "Solder." },
  { "en": "Patriark?", "id": "Kepala keluarga." },
  { "en": "Patriarkal?", "id": "Kekuasaan ayah." },
  { "en": "Patriarkat?", "id": "Sistem patriarkal." },
  { "en": "Patrimonium?", "id": "Warisan ayah." },
  { "en": "Patriot?", "id": "Pencinta tanah air." },
  { "en": "Patriotik?", "id": "Cinta tanah air." },
  { "en": "Patriotisme?", "id": "Paham cinta tanah air." },
  { "en": "Patroli?", "id": "Ronda." },
  { "en": "Patron?", "id": "Pelindung." },
  { "en": "Patronasi?", "id": "Perlindungan." },
  { "en": "Patrun?", "id": "Pola." },
  { "en": "Patuh?", "id": "Taat." },
  { "en": "Patuk?", "id": "Gigit (burung)." },
  { "en": "Patung?", "id": "Arca." },
  { "en": "Patut?", "id": "Layak." },
  { "en": "Pauh?", "id": "Mangga." },
  { "en": "Pauhi?", "id": "Ukuran (arkais)." },
  { "en": "Paul?", "id": "Ukuran (Belanda)." },
  { "en": "Paun?", "id": "Mata uang." },
  { "en": "Paus?", "id": "Pemimpin Katolik." },
  { "en": "Pause?", "id": "Jeda (Inggris)." },
  { "en": "Paut?", "id": "Kait." },
  { "en": "Paviliun?", "id": "Bangunan tambahan." },
  { "en": "Pawai?", "id": "Arak-arakan." },
  { "en": "Pawaka?", "id": "Api (Sanskerta)." },
  { "en": "Pawana?", "id": "Angin (Sanskerta)." },
  { "en": "Pawang?", "id": "Ahli." },
  { "en": "Pawestri?", "id": "Wanita (Jawa)." },
  { "en": "Pawit?", "id": "Mulai (arkais)." },
  { "en": "Pawiyatan?", "id": "Sekolah (Jawa)." },
  { "en": "Payah?", "id": "Sulit." },
  { "en": "Payang?", "id": "Jaring." },
  { "en": "Payar?", "id": "Kapal patroli." },
  { "en": "Payau?", "id": "Campuran asin tawar." },
  { "en": "Payudara?", "id": "Buah dada." },
  { "en": "Payung?", "id": "Alat pelindung." },
  { "en": "Peang?", "id": "Pipih (kepala)." },
  { "en": "Pecah?", "id": "Retak." },
  { "en": "Pecah?", "id": "Belah." },
  { "en": "Pecai?", "id": "Permainan." },
  { "en": "Pecak?", "id": "Makanan." },
  { "en": "Pecara?", "id": "Ular." },
  { "en": "Pecat?", "id": "Buang." },
  { "en": "Pecel?", "id": "Makanan." },
  { "en": "Peci?", "id": "Kopiah." },
  { "en": "Pecicilan?", "id": "Tidak bisa diam." },
  { "en": "Pecinan?", "id": "Kawasan Tionghoa." },
  { "en": "Pecok?", "id": "Luka (arkais)." },
  { "en": "Pecuk?", "id": "Burung." },
  { "en": "Pecun?", "id": "Pelacur (kasar)." },
  { "en": "Pecut?", "id": "Cambuk." },
  { "en": "Peda?", "id": "Ikan asin." },
  { "en": "Pedada?", "id": "Pohon." },
  { "en": "Pedadah?", "id": "Bidak." },
  { "en": "Pedadali?", "id": "Burung." },
  { "en": "Pedagog?", "id": "Ahli pendidikan." },
  { "en": "Pedagogi?", "id": "Ilmu pendidikan." },
  { "en": "Pedagogis?", "id": "Bersifat mendidik." },
  { "en": "Pedak?", "id": "Bedak (arkais)." },
  { "en": "Pedal?", "id": "Pijakan." },
  { "en": "Pedanda?", "id": "Pendeta Hindu Bali." },
  { "en": "Pedang?", "id": "Senjata tajam." },
  { "en": "Pedar?", "id": "Getir." },
  { "en": "Pedas?", "id": "Rasa." },
  { "en": "Pedati?", "id": "Gerobak." },
  { "en": "Pedel?", "id": "Pegawai universitas." },
  { "en": "Pedena?", "id": "Lembu (Sanskerta)." },
  { "en": "Pedendang?", "id": "Penyanyi." },
  { "en": "Pedepokan?", "id": "Padepokan." },
  { "en": "Pedesaan?", "id": "Desa." },
  { "en": "Pedestal?", "id": "Alas patung." },
  { "en": "Pedet?", "id": "Anak sapi." },
  { "en": "Pedia?", "id": "Anak (Yunani)." },
  { "en": "Pediatri?", "id": "Ilmu penyakit anak." },
  { "en": "Pedih?", "id": "Sakit." },
  { "en": "Pedikel?", "id": "Tangkai." },
  { "en": "Pedikur?", "id": "Perawatan kuku kaki." },
  { "en": "Pedis?", "id": "Kaki (Latin)." },
  { "en": "Pedodon?", "id": "Tanah." },
  { "en": "Pedofilia?", "id": "Kelainan seksual." },
  { "en": "Pedogenesis?", "id": "Pembentukan tanah." },
  { "en": "Pedok?", "id": "Kandang kuda." },
  { "en": "Pedologi?", "id": "Ilmu tanah." },
  { "en": "Pedoman?", "id": "Panduan." },
  { "en": "Peduli?", "id": "Acuh." },
  { "en": "Pedunkel?", "id": "Tangkai bunga." },
  { "en": "Pedusi?", "id": "Wanita (arkais)." },
  { "en": "Pedut?", "id": "Kabut." },
  { "en": "Pegah?", "id": "Buka (arkais)." },
  { "en": "Pegal?", "id": "Linu." },
  { "en": "Pegan?", "id": "Pegang (arkais)." },
  { "en": "Pegang?", "id": "Genggam." },
  { "en": "Pegar?", "id": "Burung." },
  { "en": "Pegas?", "id": "Per." },
  { "en": "Pegat?", "id": "Putus (Jawa)." },
  { "en": "Pegawai?", "id": "Karyawan." },
  { "en": "Pegel?", "id": "Pegal (Jawa)." },
  { "en": "Pego?", "id": "Bodoh (kasar)." },
  { "en": "Pegoh?", "id": "Keras (arkais)." },
  { "en": "Pegon?", "id": "Aksara." },
  { "en": "Peguam?", "id": "Pengacara (Malaysia)." },
  { "en": "Pegun?", "id": "Diam." },
  { "en": "Peh?", "id": "Paha (arkais)." },
  { "en": "Pehong?", "id": "Sifilis (arkais)." },
  { "en": "Pejal?", "id": "Padat." },
  { "en": "Pejam?", "id": "Tutup mata." },
  { "en": "Pejara?", "id": "Penjara (arkais)." },
  { "en": "Pek?", "id": "Damar." },
  { "en": "Peka?", "id": "Sensitif." },
  { "en": "Pekak?", "id": "Tuli." },
  { "en": "Pekam?", "id": "Cekam." },
  { "en": "Pekan?", "id": "Minggu." },
  { "en": "Pekara?", "id": "Perkara." },
  { "en": "Pekasam?", "id": "Makanan." },
  { "en": "Pekat?", "id": "Kental." },
  { "en": "Pekatu?", "id": "Pakan (arkais)." },
  { "en": "Pekau?", "id": "Teriak." },
  { "en": "Pekerti?", "id": "Akhlak." },
  { "en": "Pekik?", "id": "Teriak." },
  { "en": "Pekin?", "id": "Beijing (ejaan lama)." },
  { "en": "Pekir?", "id": "Fakir (arkais)." },
  { "en": "Pekok?", "id": "Bodoh." },
  { "en": "Pektorat?", "id": "Otot dada." },
  { "en": "Peku?", "id": "Ribuan (arkais)." },
  { "en": "Pekuk?", "id": "Lekuk." },
  { "en": "Pelabi?", "id": "Bibir (arkais)." },
  { "en": "Pelabur?", "id": "Investor." },
  { "en": "Pelagak?", "id": "Sombong." },
  { "en": "Pelagas?", "id": "Sombong (arkais)." },
  { "en": "Pelagra?", "id": "Penyakit." },
  { "en": "Pelah?", "id": "Makan." },
  { "en": "Pelajar?", "id": "Siswa." },
  { "en": "Pelajaran?", "id": "Materi ajar." },
  { "en": "Pelak?", "id": "Pelaku." },
  { "en": "Pelakat?", "id": "Plakat." },
  { "en": "Pelakon?", "id": "Aktor." },
  { "en": "Pelalah?", "id": "Rakus." },
  { "en": "Pelamin?", "id": "Pelaminan." },
  { "en": "Pelaminan?", "id": "Tempat pengantin." },
  { "en": "Pelampang?", "id": "Ukuran (arkais)." },
  { "en": "Pelan?", "id": "Lambat." },
  { "en": "Pelana?", "id": "Tempat duduk kuda." },
  { "en": "Pelancar?", "id": "Alat pelancar." },
  { "en": "Pelancong?", "id": "Turis." },
  { "en": "Pelanduk?", "id": "Kancil." },
  { "en": "Pelang?", "id": "Perahu." },
  { "en": "Pelangai?", "id": "Perilaku." },
  { "en": "Pelanggan?", "id": "Klien." },
  { "en": "Pelanggaran?", "id": "Pelanggaran." },
  { "en": "Pelangi?", "id": "Bianglala." },
  { "en": "Pelangkin?", "id": "Cat." },
  { "en": "Pelantar?", "id": "Panggung." },
  { "en": "Pelantikan?", "id": "Inaugurasi." },
  { "en": "Pelantur?", "id": "Racau." },
  { "en": "Pelapah?", "id": "Pelepah." },
  { "en": "Pelaporan?", "id": "Pemberitahuan." },
  { "en": "Pelarian?", "id": "Pengungsian." },
  { "en": "Pelaris?", "id": "Penglaris." },
  { "en": "Pelarut?", "id": "Solven." },
  { "en": "Pelas?", "id": "Ikan (arkais)." },
  { "en": "Pelat?", "id": "Lempengan." },
  { "en": "Pelata?", "id": "Ikan." },
  { "en": "Pelatuk?", "id": "Burung." },
  { "en": "Pelaut?", "id": "Nahkoda." },
  { "en": "Pelawa?", "id": "Undangan (arkais)." },
  { "en": "Pelawak?", "id": "Komedian." },
  { "en": "Pelayan?", "id": "Pramusaji." },
  { "en": "Pelayanan?", "id": "Servis." },
  { "en": "Pelayon?", "id": "Pelangi (arkais)." },
  { "en": "Pelecehan?", "id": "Penghinaan." },
  { "en": "Pelecok?", "id": "Terilir." },
  { "en": "Pelek?", "id": "Lingkaran roda." },
  { "en": "Pelekat?", "id": "Perekat." },
  { "en": "Pelelangan?", "id": "Lelang." },
  { "en": "Pelepah?", "id": "Tangkai daun palma." },
  { "en": "Peles?", "id": "Botol." },
  { "en": "Pelesat?", "id": "Cepat." },
  { "en": "Peleset?", "id": "Tidak tepat." },
  { "en": "Pelesir?", "id": "Piknik." },
  { "en": "Pelet?", "id": "Makanan ikan." },
  { "en": "Peleton?", "id": "Satuan tentara." },
  { "en": "Pelepasan?", "id": "Pembebasan." },
  { "en": "Pelias?", "id": "Jimat kebal." },
  { "en": "Pelihara?", "id": "Rawat." },
  { "en": "Pelik?", "id": "Sulit." },
  { "en": "Pelikan?", "id": "Burung." },
  { "en": "Pelipir?", "id": "Pinggir." },
  { "en": "Pelipis?", "id": "Bagian kepala." },
  { "en": "Pelir?", "id": "Penis." },
  { "en": "Pelisir?", "id": "Pita hiasan." },
  { "en": "Pelita?", "id": "Lampu." },
  { "en": "Pelitur?", "id": "Pernis." },
  { "en": "Pelo?", "id": "Cadel." },
  { "en": "Pelog?", "id": "Tangga nada gamelan." },
  { "en": "Peloh?", "id": "Lemah (arkais)." },
  { "en": "Pelojok?", "id": "Sudut (arkais)." },
  { "en": "Pelonco?", "id": "Ospek." },
  { "en": "Pelopor?", "id": "Perintis." },
  { "en": "Pelor?", "id": "Peluru." },
  { "en": "Pelosok?", "id": "Pojok." },
  { "en": "Pelosot?", "id": "Turun." },
  { "en": "Pelota?", "id": "Permainan." },
  { "en": "Pelotot?", "id": "Membelalak." },
  { "en": "Peluang?", "id": "Kesempatan." },
  { "en": "Peluh?", "id": "Keringat." },
  { "en": "Peluit?", "id": "Alat tiup." },
  { "en": "Peluk?", "id": "Dekap." },
  { "en": "Pelukis?", "id": "Seniman gambar." },
  { "en": "Peluluk?", "id": "Kurma muda." },
  { "en": "Pelulut?", "id": "Perekat." },
  { "en": "Pelumas?", "id": "Oli." },
  { "en": "Pelung?", "id": "Ayam." },
  { "en": "Peluntang?", "id": "Kayu." },
  { "en": "Pelupuk?", "id": "Kelopak mata." },
  { "en": "Peluru?", "id": "Amunisi." },
  { "en": "Peluruh?", "id": "Penggugur." },
  { "en": "Pelvis?", "id": "Panggul." },
  { "en": "Pemain?", "id": "Aktor." },
  { "en": "Pemakaman?", "id": "Kuburan." },
  { "en": "Pemali?", "id": "Pantangan." },
  { "en": "Pemalsuan?", "id": "Peniruan." },
  { "en": "Pemangkas?", "id": "Pemotong." },
  { "en": "Pemandangan?", "id": "Panorama." },
  { "en": "Pemandu?", "id": "Penunjuk jalan." },
  { "en": "Pemanfaatan?", "id": "Penggunaan." },
  { "en": "Pemasaran?", "id": "Marketing." },
  { "en": "Pematang?", "id": "Gundukan tanah." },
  { "en": "Pemayang?", "id": "Nelayan." },
  { "en": "Pembabakan?", "id": "Periodisasi." },
  { "en": "Pembaca?", "id": "Penikmat tulisan." },
  { "en": "Pembagian?", "id": "Distribusi." },
  { "en": "Pembahasan?", "id": "Diskusi." },
  { "en": "Pembajakan?", "id": "Perompakan." },
  { "en": "Pembalap?", "id": "Pesaing." },
  { "en": "Pembalasan?", "id": "Pembalasan." },
  { "en": "Pembanding?", "id": "Komparator." },
  { "en": "Pembangkit?", "id": "Generator." },
  { "en": "Pembangkangan?", "id": "Insubordinasi." },
  { "en": "Pembantaian?", "id": "Pembunuhan massal." },
  { "en": "Pembantu?", "id": "Asisten." },
  { "en": "Pembasmian?", "id": "Pemberantasan." },
  { "en": "Pembatas?", "id": "Sekat." },
  { "en": "Pembawa?", "id": "Pengantar." },
  { "en": "Pembayaran?", "id": "Pelunasan." },
  { "en": "Pembebasan?", "id": "Liberasi." },
  { "en": "Pemberdayaan?", "id": "Penguatan." },
  { "en": "Pemberhentian?", "id": "Penghentian." },
  { "en": "Pemberian?", "id": "Hadiah." },
  { "en": "Pemberitahuan?", "id": "Notifikasi." },
  { "en": "Pemberontakan?", "id": "Rebeli." },
  { "en": "Pembesar?", "id": "Pejabat." },
  { "en": "Pembiayaan?", "id": "Pendanaan." },
  { "en": "Pembicaraan?", "id": "Percakapan." },
  { "en": "Pembidangan?", "id": "Pengelompokan." },
  { "en": "Pembimbing?", "id": "Mentor." },
  { "en": "Pembinaan?", "id": "Pengarahan." },
  { "en": "Pemblokiran?", "id": "Penutupan." },
  { "en": "Pembobolan?", "id": "Perusakan." },
  { "en": "Pemboikotan?", "id": "Boikot." },
  { "en": "Pemborong?", "id": "Kontraktor." },
  { "en": "Pemborosan?", "id": "Penghamburan." },
  { "en": "Pembuangan?", "id": "Pengeluaran." },
  { "en": "Pembuat?", "id": "Produsen." },
  { "en": "Pembukaan?", "id": "Awal." },
  { "en": "Pembuktian?", "id": "Verifikasi." },
  { "en": "Pembuluh?", "id": "Saluran." },
  { "en": "Pembungkus?", "id": "Kemasan." },
  { "en": "Pembunuhan?", "id": "Eliminasi." },
  { "en": "Pemburu?", "id": "Penangkap." },
  { "en": "Pemecahan?", "id": "Solusi." },
  { "en": "Pemeliharaan?", "id": "Perawatan." },
  { "en": "Pemenang?", "id": "Juara." },
  { "en": "Pementasan?", "id": "Pertunjukan." },
  { "en": "Pemeras?", "id": "Penindas." },
  { "en": "Pemerintahan?", "id": "Administrasi." },
  { "en": "Pemeriksaan?", "id": "Inspeksi." },
  { "en": "Pemerkosaan?", "id": "Pelecehan." },
  { "en": "Pemersatu?", "id": "Integrator." },
  { "en": "Pemetaan?", "id": "Kartografi." },
  { "en": "Pemicu?", "id": "Penyebab." },
  { "en": "Pemikiran?", "id": "Gagasan." },
  { "en": "Pemilahan?", "id": "Seleksi." },
  { "en": "Pemilihan?", "id": "Seleksi." },
  { "en": "Pemimpin?", "id": "Ketua." },
  { "en": "Pemindahan?", "id": "Transfer." },
  { "en": "Pemirsa?", "id": "Penonton." },
  { "en": "Pemisahan?", "id": "Separasi." },
  { "en": "Pemogokan?", "id": "Protes." },
  { "en": "Pemohon?", "id": "Pengaju." },
  { "en": "Pemotongan?", "id": "Pengurangan." },
  { "en": "Pemrakarsa?", "id": "Inisiator." },
  { "en": "Pemuda?", "id": "Remaja." },
  { "en": "Pemudi?", "id": "Remaji." },
  { "en": "Pemudik?", "id": "Orang pulang kampung." },
  { "en": "Pemugaran?", "id": "Restorasi." },
  { "en": "Pemuka?", "id": "Tokoh." },
  { "en": "Pemukiman?", "id": "Perkampungan." },
  { "en": "Pemula?", "id": "Amatir." },
  { "en": "Pemulihan?", "id": "Rehabilitasi." },
  { "en": "Pemungutan?", "id": "Pengambilan." },
  { "en": "Pemupukan?", "id": "Fertilisasi." },
  { "en": "Pemurah?", "id": "Dermawan." },
  { "en": "Pemurnian?", "id": "Penyucian." },
  { "en": "Pemutarbalikan?", "id": "Distorsi." },
  { "en": "Pena?", "id": "Alat tulis." },
  { "en": "Penafsiran?", "id": "Interpretasi." },
  { "en": "Penakut?", "id": "Pengecut." },
  { "en": "Penalaran?", "id": "Logika." },
  { "en": "Penampilan?", "id": "Performa." },
  { "en": "Penampungan?", "id": "Wadah." },
  { "en": "Penanaman?", "id": "Investasi." },
  { "en": "Penanda?", "id": "Indikator." },
  { "en": "Penanganan?", "id": "Pengelolaan." },
  { "en": "Penangkapan?", "id": "Penahanan." },
  { "en": "Penantian?", "id": "Penungguan." },
  { "en": "Penari?", "id": "Dancer." },
  { "en": "Penarikan?", "id": "Pengambilan." },
  { "en": "Penasihat?", "id": "Konsultan." },
  { "en": "Penat?", "id": "Lelah." },
  { "en": "Penataan?", "id": "Pengaturan." },
  { "en": "Penataran?", "id": "Pelatihan." },
  { "en": "Penatu?", "id": "Binatu." },
  { "en": "Pencabutan?", "id": "Penarikan." },
  { "en": "Pencacahan?", "id": "Penghitungan." },
  { "en": "Pencaharian?", "id": "Pekerjaan." },
  { "en": "Pencairan?", "id": "Pelarutan." },
  { "en": "Pencampuran?", "id": "Pengadukan." },
  { "en": "Pencapaian?", "id": "Prestasi." },
  { "en": "Pencarian?", "id": "Pencarian." },
  { "en": "Pencatatan?", "id": "Registrasi." },
  { "en": "Pencegahan?", "id": "Prevensi." },
  { "en": "Pencemaran?", "id": "Polusi." },
  { "en": "Pencerahan?", "id": "Iluminasi." },
  { "en": "Pencernaan?", "id": "Digesti." },
  { "en": "Penciptaan?", "id": "Kreasi." },
  { "en": "Penciuman?", "id": "Olfaksi." },
  { "en": "Penculikan?", "id": "Penyanderaan." },
  { "en": "Pencurian?", "id": "Pengambilan ilegal." },
  { "en": "Pendaftar?", "id": "Peserta." },
  { "en": "Pendaftaran?", "id": "Registrasi." },
  { "en": "Pendahuluan?", "id": "Introduksi." },
  { "en": "Pendakian?", "id": "Panjatan." },
  { "en": "Pendalaman?", "id": "Eksplorasi." },
  { "en": "Pendamai?", "id": "Mediator." },
  { "en": "Pendamping?", "id": "Pengiring." },
  { "en": "Pendanaan?", "id": "Pembiayaan." },
  { "en": "Pendapat?", "id": "Opini." },
  { "en": "Pendapatan?", "id": "Penghasilan." },
  { "en": "Pendarahan?", "id": "Hemoragi." },
  { "en": "Pendataan?", "id": "Inventarisasi." },
  { "en": "Pendatang?", "id": "Imigran." },
  { "en": "Pendek?", "id": "Singkat." },
  { "en": "Pendekatan?", "id": "Metode." },
  { "en": "Penderitaan?", "id": "Sengsara." },
  { "en": "Pendeta?", "id": "Rohaniwan." },
  { "en": "Pendiam?", "id": "Introvert." },
  { "en": "Pendidikan?", "id": "Edukasi." },
  { "en": "Pendirian?", "id": "Keyakinan." },
  { "en": "Pendobrak?", "id": "Perusak." },
  { "en": "Pendorong?", "id": "Motivator." },
  { "en": "Pendopo?", "id": "Aula." },
  { "en": "Penduduk?", "id": "Warga." },
  { "en": "Pendukung?", "id": "Suporter." },
  { "en": "Pendusta?", "id": "Pembohong." },
  { "en": "Penegasan?", "id": "Konfirmasi." },
  { "en": "Penekanan?", "id": "Aksen." },
  { "en": "Peneliti?", "id": "Riset." },
  { "en": "Penelitian?", "id": "Riset." },
  { "en": "Penemuan?", "id": "Invensi." },
  { "en": "Penempatan?", "id": "Posisi." },
  { "en": "Penerangan?", "id": "Iluminasi." },
  { "en": "Penerapan?", "id": "Aplikasi." },
  { "en": "Penerbang?", "id": "Pilot." },
  { "en": "Penerbangan?", "id": "Aviasi." },
  { "en": "Penerbit?", "id": "Publikator." },
  { "en": "Penerbitan?", "id": "Publikasi." },
  { "en": "Penerima?", "id": "Reseptor." },
  { "en": "Penerimaan?", "id": "Akseptasi." },
  { "en": "Penerjemah?", "id": "Alih bahasa." },
  { "en": "Penetrasi?", "id": "Penyusupan." },
  { "en": "Penewu?", "id": "Pejabat." },
  { "en": "Pengabadian?", "id": "Pelestarian." },
  { "en": "Pengabdian?", "id": "Dedikasi." },
  { "en": "Pengabsahan?", "id": "Legalisasi." },
  { "en": "Pengacau?", "id": "Perusuh." },
  { "en": "Pengacara?", "id": "Advokat." },
  { "en": "Pengadaan?", "id": "Penyediaan." },
  { "en": "Pengadilan?", "id": "Mahkamah." },
  { "en": "Pengaduan?", "id": "Keluhan." },
  { "en": "Pengagum?", "id": "Fans." },
  { "en": "Pengairan?", "id": "Irigasi." },
  { "en": "Pengajar?", "id": "Guru." },
  { "en": "Pengajuan?", "id": "Permohonan." },
  { "en": "Pengakuan?", "id": "Konfesi." },
  { "en": "Pengalaman?", "id": "Empiri." },
  { "en": "Pengamat?", "id": "Observer." },
  { "en": "Pengambilan?", "id": "Penarikan." },
  { "en": "Pengampunan?", "id": "Amnesti." },
  { "en": "Penganan?", "id": "Kue." },
  { "en": "Penganekaragaman?", "id": "Diversifikasi." },
  { "en": "Penganut?", "id": "Pengikut." },
  { "en": "Pengarang?", "id": "Penulis." },
  { "en": "Pengarahan?", "id": "Bimbingan." },
  { "en": "Pengaruh?", "id": "Imbas." },
  { "en": "Pengasingan?", "id": "Isolasi." },
  { "en": "Pengasuh?", "id": "Perawat." },
  { "en": "Pengatur?", "id": "Regulator." },
  { "en": "Pengaturan?", "id": "Tata." },
  { "en": "Pengawal?", "id": "Penjaga." },
  { "en": "Pengawasan?", "id": "Kontrol." },
  { "en": "Pengawas?", "id": "Inspektur." },
  { "en": "Pengawetan?", "id": "Konservasi." },
  { "en": "Pengebirian?", "id": "Kastrasi." },
  { "en": "Pengebom?", "id": "Pelaku bom." },
  { "en": "Pengeboran?", "id": "Drilling." },
  { "en": "Pengecaman?", "id": "Kritik." },
  { "en": "Pengecualian?", "id": "Eksepsi." },
  { "en": "Pengecut?", "id": "Penakut." },
  { "en": "Pengejaran?", "id": "Perburuan." },
  { "en": "Pengejawantahan?", "id": "Manifestasi." },
  { "en": "Pengekangan?", "id": "Restriksi." },
  { "en": "Pengeksploitasian?", "id": "Eksploitasi." },
  { "en": "Pengelola?", "id": "Manajer." },
  { "en": "Pengelolaan?", "id": "Manajemen." },
  { "en": "Pengeluaran?", "id": "Biaya." },
  { "en": "Pengembangan?", "id": "Ekspansi." },
  { "en": "Pengepungan?", "id": "Blokade." },
  { "en": "Pengerahan?", "id": "Mobilisasi." },
  { "en": "Pengeras?", "id": "Amplifier." },
  { "en": "Pengeroyokan?", "id": "Penyerangan." },
  { "en": "Pengertian?", "id": "Pemahaman." },
  { "en": "Pengesahan?", "id": "Legalisasi." },
  { "en": "Pengetahuan?", "id": "Ilmu." },
  { "en": "Penggabungan?", "id": "Fusi." },
  { "en": "Penggalian?", "id": "Ekskavasi." },
  { "en": "Pengganti?", "id": "Substitusi." },
  { "en": "Penggemar?", "id": "Fans." },
  { "en": "Penggembala?", "id": "Gembala." },
  { "en": "Penggorengan?", "id": "Wajan." },
  { "en": "Penghabisan?", "id": "Akhir." },
  { "en": "Penghadap?", "id": "Orang menghadap." },
  { "en": "Penghalang?", "id": "Rintangan." },
  { "en": "Penghamburan?", "id": "Pemborosan." },
  { "en": "Penghancuran?", "id": "Destruksi." },
  { "en": "Penghargaan?", "id": "Apresiasi." },
  { "en": "Penghasilan?", "id": "Pendapatan." },
  { "en": "Penghasut?", "id": "Provokator." },
  { "en": "Penghematan?", "id": "Efisiensi." },
  { "en": "Penghentian?", "id": "Pemberhentian." },
  { "en": "Penghibur?", "id": "Entertainer." },
  { "en": "Penghidupan?", "id": "Nafkah." },
  { "en": "Penghinaan?", "id": "Pelecehan." },
  { "en": "Penghitungan?", "id": "Kalkulasi." },
  { "en": "Penghormatan?", "id": "Respek." },
  { "en": "Penghubung?", "id": "Konektor." },
  { "en": "Penghukuman?", "id": "Pemberian sanksi." },
  { "en": "Penghuni?", "id": "Penduduk." },
  { "en": "Pengikut?", "id": "Penganut." },
  { "en": "Penginapan?", "id": "Akomodasi." },
  { "en": "Pengingkaran?", "id": "Penolakan." },
  { "en": "Pengintaian?", "id": "Observasi." },
  { "en": "Pengiring?", "id": "Pendamping." },
  { "en": "Pengisian?", "id": "Pemuatan." },
  { "en": "Pengkajian?", "id": "Analisis." },
  { "en": "Pengkhianat?", "id": "Traditor." },
  { "en": "Pengkhianatan?", "id": "Tradisi." },
  { "en": "Penglihatan?", "id": "Visi." },
  { "en": "Pengontrol?", "id": "Regulator." },
  { "en": "Penguapan?", "id": "Evaporasi." },
  { "en": "Penguasa?", "id": "Pemimpin." },
  { "en": "Penguasaan?", "id": "Dominasi." },
  { "en": "Pengubah?", "id": "Transformator." },
  { "en": "Pengubahan?", "id": "Transformasi." },
  { "en": "Pengucapan?", "id": "Artikulasi." },
  { "en": "Pengudusan?", "id": "Konsekrasi." },
  { "en": "Pengukuran?", "id": "Metrik." },
  { "en": "Pengulas?", "id": "Komentator." },
  { "en": "Pengulangan?", "id": "Iterasi." },
  { "en": "Pengumuman?", "id": "Notifikasi." },
  { "en": "Pengunduran?", "id": "Resignasi." },
  { "en": "Pengungsian?", "id": "Evakuasi." },
  { "en": "Pengungkapan?", "id": "Eksposisi." },
  { "en": "Penguraian?", "id": "Analisis." },
  { "en": "Pengurangan?", "id": "Reduksi." },
  { "en": "Pengurus?", "id": "Administrator." },
  { "en": "Pengurusan?", "id": "Administrasi." },
  { "en": "Pengusaha?", "id": "Wiraswasta." },
  { "en": "Pengusiran?", "id": "Deportasi." },
  { "en": "Pengusul?", "id": "Penyaran." },
  { "en": "Pengusutan?", "id": "Investigasi." },
  { "en": "Penilaian?", "id": "Evaluasi." },
  { "en": "Penindasan?", "id": "Represi." },
  { "en": "Pening?", "id": "Pusing." },
  { "en": "Peninggalan?", "id": "Warisan." },
  { "en": "Peningkatan?", "id": "Eskalasi." },
  { "en": "Penipu?", "id": "Pembohong." },
  { "en": "Penipuan?", "id": "Kecurangan." },
  { "en": "Peniruan?", "id": "Imitasi." },
  { "en": "Penjara?", "id": "Bui." },
  { "en": "Penjual?", "id": "Pedagang." },
  { "en": "Penjualan?", "id": "Pemasaran." },
  { "en": "Penjelajahan?", "id": "Eksplorasi." },
  { "en": "Penjelasan?", "id": "Deskripsi." },
  { "en": "Penjemputan?", "id": "Pengambilan." },
  { "en": "Penjor?", "id": "Bambu hias." },
  { "en": "Penonton?", "id": "Pemirsa." },
  { "en": "Pentagonal?", "id": "Segilima." },
  { "en": "Pentagram?", "id": "Bintang lima." },
  { "en": "Pentameter?", "id": "Sajak lima matra." },
  { "en": "Pentan?", "id": "Senyawa." },
  { "en": "Pentana?", "id": "Pentan." },
  { "en": "Pentas?", "id": "Panggung." },
  { "en": "Pentateukh?", "id": "Lima kitab Taurat." },
  { "en": "Pentatonik?", "id": "Tangga nada lima." },
  { "en": "Pentil?", "id": "Katup ban." },
  { "en": "Penting?", "id": "Krusial." },
  { "en": "Pentol?", "id": "Tombol." },
  { "en": "Pentosa?", "id": "Gula." },
  { "en": "Pentotal?", "id": "Obat bius." },
  { "en": "Penuaan?", "id": "Proses tua." },
  { "en": "Penuh?", "id": "Lengkap." },
  { "en": "Penulis?", "id": "Pengarang." },
  { "en": "Penulisan?", "id": "Ortografi." },
  { "en": "Penumpang?", "id": "Penumpang." },
  { "en": "Penumpasan?", "id": "Pemberantasan." },
  { "en": "Penurunan?", "id": "Reduksi." },
  { "en": "Penurut?", "id": "Patuh." },
  { "en": "Penutup?", "id": "Akhir." },
  { "en": "Penyakit?", "id": "Derita." },
  { "en": "Penyaluran?", "id": "Distribusi." },
  { "en": "Penyanyi?", "id": "Biduan." },
  { "en": "Penyelamatan?", "id": "Evakuasi." },
  { "en": "Penyelewengan?", "id": "Korupsi." },
  { "en": "Penyelidikan?", "id": "Investigasi." },
  { "en": "Penyelesaian?", "id": "Solusi." },
  { "en": "Penyembahan?", "id": "Pemujaan." },
  { "en": "Penyembuhan?", "id": "Terapi." },
  { "en": "Penyerta?", "id": "Komplemen." },
  { "en": "Penyiksaan?", "id": "Aniaya." },
  { "en": "Penyimpanan?", "id": "Gudang." },
  { "en": "Penyimpangan?", "id": "Deviasi." },
  { "en": "Penyisihan?", "id": "Eliminasi." },
  { "en": "Penyokong?", "id": "Pendukung." },
  { "en": "Penyuluhan?", "id": "Penerangan." },
  { "en": "Penyusunan?", "id": "Komposisi." },
  { "en": "Penyusutan?", "id": "Depresiasi." },
  { "en": "Pepatah?", "id": "Bidal." },
  { "en": "Pepaya?", "id": "Buah." },
  { "en": "Pepes?", "id": "Makanan." },
  { "en": "Pepet?", "id": "Bunyi vokal." },
  { "en": "Pepsin?", "id": "Enzim." },
  { "en": "Peptidase?", "id": "Enzim." },
  { "en": "Peptida?", "id": "Senyawa." },
  { "en": "Pepton?", "id": "Protein." },
  { "en": "Per?", "id": "Pegas." },
  { "en": "Perabot?", "id": "Mebel." },
  { "en": "Perabuan?", "id": "Kremasi." },
  { "en": "Perada?", "id": "Lapisan emas." },
  { "en": "Peradilan?", "id": "Mahkamah." },
  { "en": "Peragaan?", "id": "Demonstrasi." },
  { "en": "Peragawan?", "id": "Model pria." },
  { "en": "Peragawati?", "id": "Model wanita." },
  { "en": "Perah?", "id": "Ambil susu." },
  { "en": "Perahu?", "id": "Kapal kecil." },
  { "en": "Perai?", "id": "Libur." },
  { "en": "Perairan?", "id": "Wilayah air." },
  { "en": "Perajin?", "id": "Pengrajin." },
  { "en": "Peraji?", "id": "Bidan (arkais)." },
  { "en": "Peraka?", "id": "Perak (arkais)." },
  { "en": "Perakaran?", "id": "Sistem akar." },
  { "en": "Peraktik?", "id": "Praktik (arkais)." },
  { "en": "Peralatan?", "id": "Perkakas." },
  { "en": "Peralihan?", "id": "Transisi." },
  { "en": "Peram?", "id": "Simpan." },
  { "en": "Peramal?", "id": "Penilik." },
  { "en": "Perambahan?", "id": "Penebangan." },
  { "en": "Perampok?", "id": "Penyamun." },
  { "en": "Peran?", "id": "Fungsi." },
  { "en": "Perancah?", "id": "Penyangga bangunan." },
  { "en": "Perancangan?", "id": "Desain." },
  { "en": "Perang?", "id": "Pertempuran." },
  { "en": "Perangai?", "id": "Watak." },
  { "en": "Peranggu?", "id": "Set (alat)." },
  { "en": "Peranginan?", "id": "Tempat istirahat." },
  { "en": "Perangkap?", "id": "Jerat." },
  { "en": "Perangkat?", "id": "Alat." },
  { "en": "Perangko?", "id": "Materai pos." },
  { "en": "Perangsang?", "id": "Stimulan." },
  { "en": "Peranjat?", "id": "Terkejut." },
  { "en": "Peranji?", "id": "Tempat tidur (arkais)." },
  { "en": "Perantara?", "id": "Mediator." },
  { "en": "Peranti?", "id": "Alat." },
  { "en": "Perap?", "id": "Dupa (arkais)." },
  { "en": "Perapatan?", "id": "Persimpangan." },
  { "en": "Peras?", "id": "Ambil sari." },
  { "en": "Perasaan?", "id": "Emosi." },
  { "en": "Perasat?", "id": "Alat." },
  { "en": "Peraturan?", "id": "Regulasi." },
  { "en": "Perawakan?", "id": "Postur." },
  { "en": "Perawan?", "id": "Gadis." },
  { "en": "Perawatan?", "id": "Pemeliharaan." },
  { "en": "Perbawa?", "id": "Pengaruh." },
  { "en": "Perbadaan?", "id": "Perbedaan (arkais)." },
  { "en": "Perbahanan?", "id": "Perbendaharaan." },
  { "en": "Perbal?", "id": "Proses verbal." },
  { "en": "Perban?", "id": "Pembalut luka." },
  { "en": "Perbani?", "id": "Bulan." },
  { "en": "Perbatin?", "id": "Orang bijak." },
  { "en": "Perbawa?", "id": "Kewibawaan." },
  { "en": "Perbedaan?", "id": "Disparitas." },
  { "en": "Perbekalan?", "id": "Logistik." },
  { "en": "Perbendaharaan?", "id": "Kas negara." },
  { "en": "Perbincangan?", "id": "Diskusi." },
  { "en": "Perbuatan?", "id": "Tindakan." },
  { "en": "Perbudakan?", "id": "Perhambaan." },
  { "en": "Perburuan?", "id": "Pengejaran." },
  { "en": "Percakapan?", "id": "Dialog." },
  { "en": "Percaya?", "id": "Yakin." },
  { "en": "Percik?", "id": "Ciprat." },
  { "en": "Percintaan?", "id": "Asmara." },
  { "en": "Percobaan?", "id": "Eksperimen." },
  { "en": "Percuma?", "id": "Sia-sia." },
  { "en": "Perdana?", "id": "Pertama." },
  { "en": "Perdikan?", "id": "Daerah bebas pajak." },
  { "en": "Perdu?", "id": "Tanaman." },
  { "en": "Perekat?", "id": "Lem." },
  { "en": "Perempuan?", "id": "Wanita." },
  { "en": "Perestroika?", "id": "Restrukturisasi (Rusia)." },
  { "en": "Perfek?", "id": "Sempurna." },
  { "en": "Perfeksi?", "id": "Kesempurnaan." },
  { "en": "Perfeksionis?", "id": "Pengejar kesempurnaan." },
  { "en": "Perforasi?", "id": "Perlubangan." },
  { "en": "Performa?", "id": "Penampilan." },
  { "en": "Perforator?", "id": "Pelubang." },
  { "en": "Pergam?", "id": "Burung." },
  { "en": "Pergantian?", "id": "Substitusi." },
  { "en": "Pergaulan?", "id": "Sosialisasi." },
  { "en": "Pergedel?", "id": "Perkedel." },
  { "en": "Pergi?", "id": "Berangkat." },
  { "en": "Pergok?", "id": "Tangkap basah." },
  { "en": "Pergolakan?", "id": "Gejolak." },
  { "en": "Pergantian?", "id": "Rotasi." },
  { "en": "Pergula?", "id": "Gula-gula." },
  { "en": "Perguruan?", "id": "Sekolah." },
  { "en": "Perhatian?", "id": "Atensi." },
  { "en": "Perhentian?", "id": "Halte." },
  { "en": "Perhitungan?", "id": "Kalkulasi." },
  { "en": "Perhiasan?", "id": "Ornamen." },
  { "en": "Peri?", "id": "Makhluk halus." },
  { "en": "Peria?", "id": "Pare." },
  { "en": "Perian?", "id": "Wadah bambu." },
  { "en": "Periander?", "id": "Kelopak tambahan." },
  { "en": "Periapsis?", "id": "Titik terdekat orbit." },
  { "en": "Peribahasa?", "id": "Pepatah." },
  { "en": "Periboga?", "id": "Ahli boga." },
  { "en": "Peridi?", "id": "Subur (hewan)." },
  { "en": "Periferal?", "id": "Pinggiran." },
  { "en": "Periferi?", "id": "Tepi." },
  { "en": "Perifrasa?", "id": "Ungkapan panjang." },
  { "en": "Perifrastis?", "id": "Berbelit-belit." },
  { "en": "Perifiton?", "id": "Organisme menempel." },
  { "en": "Perige?", "id": "Titik terdekat bulan." },
  { "en": "Perigel?", "id": "Cekatan." },
  { "en": "Perigi?", "id": "Sumur." },
  { "en": "Perih?", "id": "Sakit." },
  { "en": "Perihal?", "id": "Mengenai." },
  { "en": "Perihelion?", "id": "Titik terdekat matahari." },
  { "en": "Perikarditis?", "id": "Radang selaput jantung." },
  { "en": "Perikardium?", "id": "Selaput jantung." },
  { "en": "Perikemanusiaan?", "id": "Humanitas." },
  { "en": "Perikondrium?", "id": "Selaput tulang rawan." },
  { "en": "Perikop?", "id": "Bagian kitab suci." },
  { "en": "Perilaku?", "id": "Tingkah laku." },
  { "en": "Perimbas?", "id": "Kapak genggam." },
  { "en": "Perimeter?", "id": "Keliling." },
  { "en": "Perimisium?", "id": "Jaringan ikat otot." },
  { "en": "Perin?", "id": "Simpan (arkais)." },
  { "en": "Perineum?", "id": "Daerah antara anus dan kelamin." },
  { "en": "Peringkat?", "id": "Tingkatan." },
  { "en": "Peringkik?", "id": "Suara kuda." },
  { "en": "Perintah?", "id": "Instruksi." },
  { "en": "Perintis?", "id": "Pelopor." },
  { "en": "Period?", "id": "Masa (Inggris)." },
  { "en": "Periode?", "id": "Masa." },
  { "en": "Periodik?", "id": "Berkala." },
  { "en": "Periodisasi?", "id": "Pembabakan." },
  { "en": "Periodontium?", "id": "Jaringan gigi." },
  { "en": "Periosteum?", "id": "Selaput tulang." },
  { "en": "Peripatetik?", "id": "Berkeliling." },
  { "en": "Peripteral?", "id": "Dikelilingi tiang." },
  { "en": "Perisa?", "id": "Pemberi rasa." },
  { "en": "Perisai?", "id": "Tameng." },
  { "en": "Periskop?", "id": "Alat lihat kapal selam." },
  { "en": "Perisperma?", "id": "Cadangan makanan biji." },
  { "en": "Peristalsis?", "id": "Gerak usus." },
  { "en": "Peristaltik?", "id": "Bersifat peristalsis." },
  { "en": "Peristeron?", "id": "Tanaman." },
  { "en": "Peristil?", "id": "Serambi bertiang." },
  { "en": "Peristiwa?", "id": "Kejadian." },
  { "en": "Perit?", "id": "Kikir (arkais)." },
  { "en": "Peritoneum?", "id": "Selaput perut." },
  { "en": "Peritonitis?", "id": "Radang selaput perut." },
  { "en": "Periuk?", "id": "Panci." },
  { "en": "Perjaka?", "id": "Bujang." },
  { "en": "Perjalanan?", "id": "Wisata." },
  { "en": "Perjanjian?", "id": "Kontrak." },
  { "en": "Perjudian?", "id": "Taruhan." },
  { "en": "Perjuangan?", "id": "Usaha." },
  { "en": "Perka?", "id": "Kain." },
  { "en": "Perkabungan?", "id": "Masa duka." },
  { "en": "Perkak?", "id": "Berisik." },
  { "en": "Perkala?", "id": "Alat (arkais)." },
  { "en": "Perkalehan?", "id": "Perkelahian (arkais)." },
  { "en": "Perkampungan?", "id": "Pemukiman." },
  { "en": "Perkara?", "id": "Masalah." },
  { "en": "Perkas?", "id": "Berani (arkais)." },
  { "en": "Perkasa?", "id": "Kuat." },
  { "en": "Perkawanan?", "id": "Persahabatan." },
  { "en": "Perkawinan?", "id": "Pernikahan." },
  { "en": "Perkedel?", "id": "Makanan." },
  { "en": "Perkelahian?", "id": "Pertengkaran." },
  { "en": "Perkembangan?", "id": "Progres." },
  { "en": "Perkemihan?", "id": "Sistem urin." },
  { "en": "Perkenan?", "id": "Persetujuan." },
  { "en": "Perkenanan?", "id": "Persetujuan." },
  { "en": "Perkerisan?", "id": "Ilmu keris." },
  { "en": "Perkosa?", "id": "Paksa." },
  { "en": "Perkutut?", "id": "Burung." },
  { "en": "Perlahan?", "id": "Lambat." },
  { "en": "Perlambang?", "id": "Simbol." },
  { "en": "Perlengkapan?", "id": "Atribut." },
  { "en": "Perlindungan?", "id": "Proteksi." },
  { "en": "Perling?", "id": "Burung." },
  { "en": "Perlombaan?", "id": "Kompetisi." },
  { "en": "Perlu?", "id": "Butuh." },
  { "en": "Perluasan?", "id": "Ekspansi." },
  { "en": "Permai?", "id": "Indah." },
  { "en": "Permaisuri?", "id": "Ratu." },
  { "en": "Permak?", "id": "Ubah." },
  { "en": "Permainan?", "id": "Game." },
  { "en": "Permalin?", "id": "Karpet (arkais)." },
  { "en": "Permanen?", "id": "Tetap." },
  { "en": "Permadani?", "id": "Karpet." },
  { "en": "Permata?", "id": "Batu mulia." },
  { "en": "Permisif?", "id": "Serba boleh." },
  { "en": "Permisi?", "id": "Izin." },
  { "en": "Permohonan?", "id": "Pengajuan." },
  { "en": "Pernah?", "id": "Sudah." },
  { "en": "Pernik?", "id": "Manik-manik." },
  { "en": "Pernikahan?", "id": "Perkawinan." },
  { "en": "Perobahan?", "id": "Perubahan (arkais)." },
  { "en": "Perompak?", "id": "Bajak laut." },
  { "en": "Peron?", "id": "Panggung stasiun." },
  { "en": "Perosok?", "id": "Jatuh." },
  { "en": "Perosot?", "id": "Turun." },
  { "en": "Perponding?", "id": "Pajak tanah (Belanda)." },
  { "en": "Pers?", "id": "Media massa." },
  { "en": "Persada?", "id": "Tanah air." },
  { "en": "Persalinan?", "id": "Kelahiran." },
  { "en": "Persamaan?", "id": "Ekuivalensi." },
  { "en": "Persekot?", "id": "Uang muka." },
  { "en": "Persekusi?", "id": "Penindasan." },
  { "en": "Persemayaman?", "id": "Tempat istirahat." },
  { "en": "Persembahan?", "id": "Korban." },
  { "en": "Persemendaan?", "id": "Hubungan ipar." },
  { "en": "Persen?", "id": "Perseratus." },
  { "en": "Persentase?", "id": "Perbandingan." },
  { "en": "Persepsi?", "id": "Tanggapan." },
  { "en": "Perseptif?", "id": "Tajam." },
  { "en": "Perseptor?", "id": "Penerima." },
  { "en": "Persero?", "id": "Perusahaan." },
  { "en": "Persetan?", "id": "Masa bodoh." },
  { "en": "Persetujuan?", "id": "Akseptasi." },
  { "en": "Persia?", "id": "Iran kuno." },
  { "en": "Persih?", "id": "Bersih." },
  { "en": "Persik?", "id": "Buah." },
  { "en": "Persil?", "id": "Petak tanah." },
  { "en": "Persis?", "id": "Tepat." },
  { "en": "Persisten?", "id": "Gigih." },
  { "en": "Persistensi?", "id": "Kegigihan." },
  { "en": "Persona?", "id": "Pribadi." },
  { "en": "Personal?", "id": "Pribadi." },
  { "en": "Personalia?", "id": "Kepegawaian." },
  { "en": "Personel?", "id": "Pegawai." },
  { "en": "Personifikasi?", "id": "Penginsanan." },
  { "en": "Perspektif?", "id": "Sudut pandang." },
  { "en": "Persuasi?", "id": "Bujukan." },
  { "en": "Persuasif?", "id": "Membujuk." },
  { "en": "Pertalian?", "id": "Hubungan." },
  { "en": "Pertahanan?", "id": "Defensi." },
  { "en": "Pertanda?", "id": "Isyarat." },
  { "en": "Pertapa?", "id": "Orang bertapa." },
  { "en": "Pertapaan?", "id": "Tempat bertapa." },
  { "en": "Pertarungan?", "id": "Perkelahian." },
  { "en": "Pertaruhan?", "id": "Judi." },
  { "en": "Pertelaan?", "id": "Uraian." },
  { "en": "Pertemuan?", "id": "Rapat." },
  { "en": "Pertentangan?", "id": "Konflik." },
  { "en": "Pertiwi?", "id": "Bumi." },
  { "en": "Pertunjukan?", "id": "Pementasan." },
  { "en": "Pertulangan?", "id": "Tulang." },
  { "en": "Pertumbuhan?", "id": "Perkembangan." },
  { "en": "Pertumpahan?", "id": "Tumpah." },
  { "en": "Pertunangan?", "id": "Ikatan janji nikah." },
  { "en": "Pertusis?", "id": "Batuk rejan." },
  { "en": "Peruan?", "id": "Wadah." },
  { "en": "Peruah?", "id": "Banyak." },
  { "en": "Peruak?", "id": "Terbuka." },
  { "en": "Perual?", "id": "Sombong." },
  { "en": "Peruan?", "id": "Tempat sirih." },
  { "en": "Peruang?", "id": "Lubang." },
  { "en": "Perubalsem?", "id": "Balsam." },
  { "en": "Perubahan?", "id": "Transformasi." },
  { "en": "Perudang?", "id": "Udang." },
  { "en": "Peruk?", "id": "Benam." },
  { "en": "Perum?", "id": "Alat ukur." },
  { "en": "Perumahan?", "id": "Kompleks rumah." },
  { "en": "Perumpamaan?", "id": "Kiasan." },
  { "en": "Perun?", "id": "Bakar sampah." },
  { "en": "Perunggu?", "id": "Logam." },
  { "en": "Perunjung?", "id": "Paling tinggi." },
  { "en": "Perupuk?", "id": "Pohon." },
  { "en": "Perusa?", "id": "Paksa." },
  { "en": "Perusahaan?", "id": "Industri." },
  { "en": "Perusak?", "id": "Destruktor." },
  { "en": "Perusuh?", "id": "Pengacau." },
  { "en": "Perut?", "id": "Lambung." },
  { "en": "Perversi?", "id": "Penyimpangan." },
  { "en": "Perwara?", "id": "Dayang." },
  { "en": "Perwatin?", "id": "Pejabat." },
  { "en": "Perwira?", "id": "Opsir." },
  { "en": "Perwujudan?", "id": "Manifestasi." },
  { "en": "Pes?", "id": "Wabah." },
  { "en": "Pesa?", "id": "Pesan (arkais)." },
  { "en": "Pesai?", "id": "Lepas (arkais)." },
  { "en": "Pesak?", "id": "Kain dalam." },
  { "en": "Pesakin?", "id": "Kain." },
  { "en": "Pesam?", "id": "Hangat." },
  { "en": "Pesan?", "id": "Amanat." },
  { "en": "Pesang?", "id": "Sakit (arkais)." },
  { "en": "Pesanggrahan?", "id": "Vila." },
  { "en": "Pesantren?", "id": "Sekolah agama Islam." },
  { "en": "Pesara?", "id": "Makam." },
  { "en": "Pesat?", "id": "Cepat." },
  { "en": "Pesawat?", "id": "Kapal terbang." },
  { "en": "Pese?", "id": "Penyakit." },
  { "en": "Pesek?", "id": "Tidak mancung." },
  { "en": "Peser?", "id": "Mata uang." },
  { "en": "Pesero?", "id": "Pemegang saham." },
  { "en": "Peset?", "id": "Pijit." },
  { "en": "Pesi?", "id": "Rapuh (arkais)." },
  { "en": "Pesiar?", "id": "Jalan-jalan." },
  { "en": "Pesimis?", "id": "Putus asa." },
  { "en": "Pesimisme?", "id": "Sikap putus asa." },
  { "en": "Pesinden?", "id": "Penyanyi Jawa." },
  { "en": "Pesing?", "id": "Bau air kencing." },
  { "en": "Pesisir?", "id": "Pantai." },
  { "en": "Pesok?", "id": "Lekuk." },
  { "en": "Pesolot?", "id": "Kain." },
  { "en": "Pesona?", "id": "Daya tarik." },
  { "en": "Pesta?", "id": "Perayaan." },
  { "en": "Pestaka?", "id": "Buku ramalan." },
  { "en": "Pestilens?", "id": "Wabah." },
  { "en": "Pestisida?", "id": "Pembasmi hama." },
  { "en": "Pesuk?", "id": "Berlubang." },
  { "en": "Pesut?", "id": "Lumba-lumba air tawar." },
  { "en": "Peta?", "id": "Denah." },
  { "en": "Petah?", "id": "Lancar bicara." },
  { "en": "Petai?", "id": "Buah." },
  { "en": "Petak?", "id": "Ruang." },
  { "en": "Petaka?", "id": "Bencana." },
  { "en": "Petal?", "id": "Kelopak bunga." },
  { "en": "Petala?", "id": "Lapisan." },
  { "en": "Petaling?", "id": "Pohon." },
  { "en": "Petam?", "id": "Ikat kepala." },
  { "en": "Petan?", "id": "Cari kutu." },
  { "en": "Petang?", "id": "Sore." },
  { "en": "Petaram?", "id": "Keris." },
  { "en": "Petarang?", "id": "Tempat bertelur." },
  { "en": "Petarangan?", "id": "Petarang." },
  { "en": "Petas?", "id": "Beras." },
  { "en": "Petatus?", "id": "Kentang (arkais)." },
  { "en": "Peterana?", "id": "Singgasana." },
  { "en": "Petes?", "id": "Pijit." },
  { "en": "Peti?", "id": "Kotak." },
  { "en": "Petik?", "id": "Ambil." },
  { "en": "Petilan?", "id": "Bagian." },
  { "en": "Petiolus?", "id": "Tangkai daun." },
  { "en": "Petir?", "id": "Halilintar." },
  { "en": "Petis?", "id": "Bumbu." },
  { "en": "Petisi?", "id": "Permohonan." },
  { "en": "Petitih?", "id": "Pepatah." },
  { "en": "Petola?", "id": "Kain sutra." },
  { "en": "Petor?", "id": "Inspektur (Belanda)." },
  { "en": "Petra?", "id": "Batu (Yunani)." },
  { "en": "Petrel?", "id": "Burung laut." },
  { "en": "Petri?", "id": "Cawan." },
  { "en": "Petrokimia?", "id": "Kimia minyak bumi." },
  { "en": "Petrol?", "id": "Bensin." },
  { "en": "Petrolatum?", "id": "Vaselin." },
  { "en": "Petroleum?", "id": "Minyak bumi." },
  { "en": "Petrologi?", "id": "Ilmu batuan." },
  { "en": "Petromaks?", "id": "Lampu." },
  { "en": "Petsai?", "id": "Sawi putih." },
  { "en": "Petuah?", "id": "Nasihat." },
  { "en": "Petuding?", "id": "Penunjuk." },
  { "en": "Petuduh?", "id": "Tuduhan (arkais)." },
  { "en": "Petugas?", "id": "Aparat." },
  { "en": "Petuk?", "id": "Surat pajak." },
  { "en": "Petunia?", "id": "Bunga." },
  { "en": "Peturun?", "id": "Turunan (arkais)." },
  { "en": "Petus?", "id": "Pecut." },
  { "en": "Petut?", "id": "Nasihat." },
  { "en": "Pewaka?", "id": "Pewaris (arkais)." },
  { "en": "Pewangi?", "id": "Parfum." },
  { "en": "Pewarna?", "id": "Cat." },
  { "en": "Pewaris?", "id": "Ahli waris." },
  { "en": "Peyang?", "id": "Peang." },
  { "en": "Peyek?", "id": "Rempeyek." },
  { "en": "Peyot?", "id": "Tidak bulat." },
  { "en": "Pi?", "id": "Konstanta matematika." },
  { "en": "Pial?", "id": "Gelambir." },
  { "en": "Piala?", "id": "Gelas." },
  { "en": "Pialang?", "id": "Makelar." },
  { "en": "Pialing?", "id": "Penyakit." },
  { "en": "Piam?", "id": "Cacing." },
  { "en": "Piama?", "id": "Pakaian tidur." },
  { "en": "Pian?", "id": "Kamu (Banjar)." },
  { "en": "Piat?", "id": "Putar." },
  { "en": "Piano?", "id": "Alat musik." },
  { "en": "Pianola?", "id": "Piano otomatis." },
  { "en": "Pias?", "id": "Pucat." },
  { "en": "Piaster?", "id": "Mata uang." },
  { "en": "Piatu?", "id": "Yatim piatu." },
  { "en": "Pica?", "id": "Nafsu makan aneh." },
  { "en": "Picik?", "id": "Sempit (pikiran)." },
  { "en": "Picing?", "id": "Pejam." },
  { "en": "Picis?", "id": "Uang." },
  { "en": "Pico?", "id": "Sangat kecil (awalan)." },
  { "en": "Picu?", "id": "Pemicu." },
  { "en": "Pidana?", "id": "Hukuman." },
  { "en": "Pidato?", "id": "Orasi." },
  { "en": "Pidi?", "id": "Arahkan (arkais)." },
  { "en": "Pie?", "id": "Pai." },
  { "en": "Piel?", "id": "Nama ikan." },
  { "en": "Piezoelektrik?", "id": "Sifat kristal." },
  { "en": "Pigmen?", "id": "Zat warna." },
  { "en": "Pigmentasi?", "id": "Pewarnaan." },
  { "en": "Pigmi?", "id": "Orang kerdil." },
  { "en": "Pihak?", "id": "Sisi." },
  { "en": "Pijah?", "id": "Telur ikan." },
  { "en": "Pijahan?", "id": "Hasil pijah." },
  { "en": "Pijak?", "id": "Injak." },
  { "en": "Pijar?", "id": "Nyala." },
  { "en": "Pijat?", "id": "Urut." },
  { "en": "Pijin?", "id": "Bahasa campuran." },
  { "en": "Pijinasi?", "id": "Proses pijin." },
  { "en": "Pika?", "id": "Pica." },
  { "en": "Pikap?", "id": "Mobil bak terbuka." },
  { "en": "Pikat?", "id": "Daya tarik." },
  { "en": "Pikau?", "id": "Teriak (arkais)." },
  { "en": "Piket?", "id": "Jaga." },
  { "en": "Pikir?", "id": "Nalar." },
  { "en": "Pikiran?", "id": "Gagasan." },
  { "en": "Piknik?", "id": "Tamasya." },
  { "en": "Pikofarad?", "id": "Satuan kapasitansi." },
  { "en": "Pikogram?", "id": "Satuan berat." },
  { "en": "Pikolo?", "id": "Seruling kecil." },
  { "en": "Pikometer?", "id": "Satuan panjang." },
  { "en": "Piksel?", "id": "Titik gambar." },
  { "en": "Piktografi?", "id": "Tulisan gambar." },
  { "en": "Piktogram?", "id": "Simbol gambar." },
  { "en": "Pikul?", "id": "Angkat." },
  { "en": "Pikun?", "id": "Lupa usia." },
  { "en": "Pil?", "id": "Tablet obat." },
  { "en": "Pilah?", "id": "Pilih." },
  { "en": "Pilak?", "id": "Uang (arkais)." },
  { "en": "Pilang?", "id": "Pohon." },
  { "en": "Pilar?", "id": "Tiang." },
  { "en": "Pilas?", "id": "Tiang semu." },
  { "en": "Pilaster?", "id": "Pilas." },
  { "en": "Pilau?", "id": "Nasi." },
  { "en": "Pileh?", "id": "Pilih (arkais)." },
  { "en": "Pilek?", "id": "Selesma." },
  { "en": "Pileren?", "id": "Pilar (arkais)." },
  { "en": "Pilih?", "id": "Seleksi." },
  { "en": "Pilin?", "id": "Putar." },
  { "en": "Pilis?", "id": "Bedak dahi." },
  { "en": "Pilon?", "id": "Tiang besar." },
  { "en": "Pilorus?", "id": "Bagian lambung." },
  { "en": "Pilot?", "id": "Pengemudi pesawat." },
  { "en": "Pilu?", "id": "Sedih." },
  { "en": "Pimpin?", "id": "Pandu." },
  { "en": "Pimpinan?", "id": "Ketua." },
  { "en": "Pin?", "id": "Jarum." },
  { "en": "Pina?", "id": "Sirip (arkais)." },
  { "en": "Pinak?", "id": "Keturunan." },
  { "en": "Pinang?", "id": "Buah." },
  { "en": "Pinar?", "id": "Kilau (arkais)." },
  { "en": "Pincang?", "id": "Tidak seimbang (jalan)." },
  { "en": "Pincuk?", "id": "Wadah daun." },
  { "en": "Pinda?", "id": "Pindah (arkais)." },
  { "en": "Pindah?", "id": "Geser." },
  { "en": "Pindai?", "id": "Periksa." },
  { "en": "Pindang?", "id": "Masakan ikan." },
  { "en": "Pinel?", "id": "Pohon." },
  { "en": "Pinga?", "id": "Bingung (arkais)." },
  { "en": "Pingai?", "id": "Kuning muda." },
  { "en": "Pinggan?", "id": "Piring besar." },
  { "en": "Pinggang?", "id": "Bagian tubuh." },
  { "en": "Pinggir?", "id": "Tepi." },
  { "en": "Pingit?", "id": "Kurung." },
  { "en": "Pingkal?", "id": "Tertawa." },
  { "en": "Pingkel?", "id": "Tertawa (Jawa)." },
  { "en": "Pingpong?", "id": "Tenis meja." },
  { "en": "Pingsan?", "id": "Tidak sadar." },
  { "en": "Pinguin?", "id": "Burung." },
  { "en": "Pinis?", "id": "Kapal." },
  { "en": "Pinisi?", "id": "Kapal layar Bugis." },
  { "en": "Pinjal?", "id": "Kutu." },
  { "en": "Pinjam?", "id": "Utang." },
  { "en": "Pinset?", "id": "Penjepit kecil." },
  { "en": "Pinta?", "id": "Permintaan." },
  { "en": "Pintal?", "id": "Buat benang." },
  { "en": "Pintar?", "id": "Cerdas." },
  { "en": "Pintas?", "id": "Jalan pendek." },
  { "en": "Pintu?", "id": "Gerbang." },
  { "en": "Pinus?", "id": "Pohon." },
  { "en": "Pion?", "id": "Bidak catur." },
  { "en": "Pionir?", "id": "Perintis." },
  { "en": "Pipa?", "id": "Saluran." },
  { "en": "Pipet?", "id": "Alat sedot." },
  { "en": "Pipi?", "id": "Bagian wajah." },
  { "en": "Pipih?", "id": "Datar." },
  { "en": "Pipis?", "id": "Kencing." },
  { "en": "Pipit?", "id": "Burung." },
  { "en": "Pir?", "id": "Buah." },
  { "en": "Pirai?", "id": "Penyakit sendi." },
  { "en": "Piramida?", "id": "Bangunan Mesir." },
  { "en": "Piramidal?", "id": "Berbentuk piramida." },
  { "en": "Piran?", "id": "Mertua (arkais)." },
  { "en": "Piranha?", "id": "Ikan." },
  { "en": "Pirang?", "id": "Warna rambut." },
  { "en": "Pirau?", "id": "Simpang." },
  { "en": "Pireksia?", "id": "Demam." },
  { "en": "Piren?", "id": "Obat." },
  { "en": "Piretrum?", "id": "Tanaman insektisida." },
  { "en": "Pirian?", "id": "Tempat sirih." },
  { "en": "Piridin?", "id": "Senyawa." },
  { "en": "Piriform?", "id": "Berbentuk buah pir." },
  { "en": "Piring?", "id": "Wadah makan." },
  { "en": "Pirit?", "id": "Mineral." },
  { "en": "Pirofan?", "id": "Mineral." },
  { "en": "Pirogen?", "id": "Penyebab demam." },
  { "en": "Piroklastik?", "id": "Batuan vulkanik." },
  { "en": "Pirolisis?", "id": "Dekomposisi panas." },
  { "en": "Piromania?", "id": "Suka membakar." },
  { "en": "Pirometalurgi?", "id": "Pengolahan logam panas." },
  { "en": "Pirometer?", "id": "Pengukur suhu tinggi." },
  { "en": "Pirus?", "id": "Batu permata." },
  { "en": "Piruvat?", "id": "Senyawa." },
  { "en": "Pis?", "id": "Uang (arkais)." },
  { "en": "Pisah?", "id": "Cerai." },
  { "en": "Pisak?", "id": "Kain." },
  { "en": "Pisang?", "id": "Buah." },
  { "en": "Pises?", "id": "Rasi bintang." },
  { "en": "Pisiformis?", "id": "Tulang." },
  { "en": "Pisik?", "id": "Fisik (arkais)." },
  { "en": "Pisin?", "id": "Piring kecil." },
  { "en": "Pisit?", "id": "Padat." },
  { "en": "Pispot?", "id": "Wadah kencing." },
  { "en": "Pistol?", "id": "Senjata api." },
  { "en": "Piston?", "id": "Bagian mesin." },
  { "en": "Pisuh?", "id": "Maki (Jawa)." },
  { "en": "Pita?", "id": "Tali." },
  { "en": "Pitak?", "id": "Botak sebagian." },
  { "en": "Pitaka?", "id": "Keranjang (Sanskerta)." },
  { "en": "Pitakam?", "id": "Peti." },
  { "en": "Pitang?", "id": "Pucat (arkais)." },
  { "en": "Pitarah?", "id": "Leluhur." },
  { "en": "Pitas?", "id": "Cerdas (arkais)." },
  { "en": "Piteketropus?", "id": "Manusia purba." },
  { "en": "Piting?", "id": "Kepiting." },
  { "en": "Pitiriasis?", "id": "Penyakit kulit." },
  { "en": "Pitis?", "id": "Uang." },
  { "en": "Piton?", "id": "Ular." },
  { "en": "Pitoresk?", "id": "Indah." },
  { "en": "Pitrah?", "id": "Fitrah." },
  { "en": "Pitri?", "id": "Leluhur (Hindu)." },
  { "en": "Pituah?", "id": "Petuah." },
  { "en": "Pitut?", "id": "Nasihat." },
  { "en": "Pitung?", "id": "Tujuh (Jawa)." },
  { "en": "Pituus?", "id": "Panjang (Latin)." },
  { "en": "Piyama?", "id": "Piama." },
  { "en": "Piyik?", "id": "Anak burung." },
  { "en": "Plafon?", "id": "Langit-langit." },
  { "en": "Plag?", "id": "Steker." },
  { "en": "Plagiat?", "id": "Jiplakan." },
  { "en": "Plagiator?", "id": "Penjiplak." },
  { "en": "Plagioklas?", "id": "Mineral." },
  { "en": "Plakat?", "id": "Pengumuman tempel." },
  { "en": "Plaket?", "id": "Lempeng." },
  { "en": "Plaksegel?", "id": "Meterai tempel." },
  { "en": "Plan?", "id": "Rencana." },
  { "en": "Planar?", "id": "Datar." },
  { "en": "Plane?", "id": "Pesawat (Inggris)." },
  { "en": "Planet?", "id": "Benda langit." },
  { "en": "Planetarium?", "id": "Gedung simulasi langit." },
  { "en": "Planetesimal?", "id": "Benda cikal planet." },
  { "en": "Planetoid?", "id": "Asteroid." },
  { "en": "Planimeter?", "id": "Pengukur luas." },
  { "en": "Planimetri?", "id": "Pengukuran luas." },
  { "en": "Planing?", "id": "Perencanaan." },
  { "en": "Plankton?", "id": "Organisme hanyut." },
  { "en": "Planoblast?", "id": "Sel." },
  { "en": "Planologi?", "id": "Ilmu tata kota." },
  { "en": "Plantase?", "id": "Perkebunan." },
  { "en": "Plafon?", "id": "Atap." },
  { "en": "Plasma?", "id": "Cairan darah." },
  { "en": "Plasmodium?", "id": "Parasit malaria." },
  { "en": "Plastein?", "id": "Protein." },
  { "en": "Plaster?", "id": "Plester." },
  { "en": "Plastid?", "id": "Bagian sel tumbuhan." },
  { "en": "Plastik?", "id": "Bahan sintetis." },
  { "en": "Plastis?", "id": "Mudah dibentuk." },
  { "en": "Plastisitas?", "id": "Sifat plastis." },
  { "en": "Plat?", "id": "Lempeng logam." },
  { "en": "Platan?", "id": "Pohon." },
  { "en": "Platelet?", "id": "Keping darah." },
  { "en": "Platform?", "id": "Panggung." },
  { "en": "Platina?", "id": "Logam mulia." },
  { "en": "Platinum?", "id": "Platina." },
  { "en": "Platonik?", "id": "Murni (cinta)." },
  { "en": "Platonisme?", "id": "Filsafat Plato." },
  { "en": "Plaza?", "id": "Pusat perbelanjaan." },
  { "en": "Plebisit?", "id": "Pemungutan suara rakyat." },
  { "en": "Pleidoi?", "id": "Pembelaan." },
  { "en": "Pleiomorfisme?", "id": "Banyak bentuk." },
  { "en": "Pleistosen?", "id": "Kala geologi." },
  { "en": "Pleksiglas?", "id": "Kaca akrilik." },
  { "en": "Plena?", "id": "Penuh (arkais)." },
  { "en": "Pleno?", "id": "Lengkap (sidang)." },
  { "en": "Pleonasme?", "id": "Gaya bahasa berlebih." },
  { "en": "Pleonastis?", "id": "Bersifat pleonasme." },
  { "en": "Ples?", "id": "Botol (Belanda)." },
  { "en": "Plester?", "id": "Penutup luka." },
  { "en": "Pleton?", "id": "Peleton." },
  { "en": "Pletora?", "id": "Kelebihan." },
  { "en": "Pleura?", "id": "Selaput paru." },
  { "en": "Pleuritis?", "id": "Radang pleura." },
  { "en": "Plin-plan?", "id": "Tidak tetap." },
  { "en": "Plinteng?", "id": "Ketapel." },
  { "en": "Plintit?", "id": "Pelintir." },
  { "en": "Pliosen?", "id": "Kala geologi." },
  { "en": "Plisir?", "id": "Lipatan." },
  { "en": "Plit?", "id": "Pelit (arkais)." },
  { "en": "Plombir?", "id": "Segel." },
  { "en": "Plong?", "id": "Lega." },
  { "en": "Plonga-plongo?", "id": "Bingung." },
  { "en": "Plonci?", "id": "Botak (arkais)." },
  { "en": "Plonco?", "id": "Calon." },
  { "en": "Plot?", "id": "Alur cerita." },
  { "en": "Ploter?", "id": "Pencetak gambar." },
  { "en": "Plug?", "id": "Steker." },
  { "en": "Plumbago?", "id": "Tanaman." },
  { "en": "Plumbum?", "id": "Timbal." },
  { "en": "Plural?", "id": "Jamak." },
  { "en": "Pluralis?", "id": "Penganut pluralisme." },
  { "en": "Pluralisme?", "id": "Paham kemajemukan." },
  { "en": "Pluralitas?", "id": "Kemajemukan." },
  { "en": "Plus?", "id": "Tambah." },
  { "en": "Plutokrasi?", "id": "Pemerintahan kaya." },
  { "en": "Plutonium?", "id": "Unsur kimia." },
  { "en": "Pluvial?", "id": "Berkaitan hujan." },
  { "en": "Pluviometer?", "id": "Pengukur curah hujan." },
  { "en": "Pneumatika?", "id": "Ilmu udara tekan." },
  { "en": "Pneumatofor?", "id": "Akar napas." },
  { "en": "Pneumokoniosis?", "id": "Penyakit paru." },
  { "en": "Pneumonia?", "id": "Radang paru." },
  { "en": "Poal?", "id": "Kain." },
  { "en": "Poces?", "id": "Periksa (Belanda)." },
  { "en": "Poci?", "id": "Teko." },
  { "en": "Pocok?", "id": "Jimat." },
  { "en": "Podiatri?", "id": "Ilmu kaki." },
  { "en": "Podium?", "id": "Panggung." },
  { "en": "Poe?", "id": "Saga (Polinesia)." },
  { "en": "Poedjangga?", "id": "Pujangga (ejaan lama)." },
  { "en": "Pof?", "id": "Empuk." },
  { "en": "Pogoniasis?", "id": "Janggut lebat." },
  { "en": "Pohaci?", "id": "Dewi padi." },
  { "en": "Pohon?", "id": "Tanaman besar." },
  { "en": "Poikilohalin?", "id": "Air berubah salinitas." },
  { "en": "Poikilotermik?", "id": "Berdarah dingin." },
  { "en": "Poin?", "id": "Nilai." },
  { "en": "Poinsetia?", "id": "Kastuba." },
  { "en": "Pojok?", "id": "Sudut." },
  { "en": "Pok?", "id": "Penyakit cacar." },
  { "en": "Pokal?", "id": "Akal (arkais)." },
  { "en": "Pokcai?", "id": "Sayuran." },
  { "en": "Pokeng?", "id": "Pincang (arkais)." },
  { "en": "Poker?", "id": "Permainan kartu." },
  { "en": "Poket?", "id": "Saku (Inggris)." },
  { "en": "Pokok?", "id": "Utama." },
  { "en": "Pokrol?", "id": "Pengacara." },
  { "en": "Pol?", "id": "Penuh (Belanda)." },
  { "en": "Pola?", "id": "Corak." },
  { "en": "Polah?", "id": "Tingkah." },
  { "en": "Polandia?", "id": "Negara." },
  { "en": "Polang-paling?", "id": "Berganti-ganti." },
  { "en": "Polar?", "id": "Kutub." },
  { "en": "Polaris?", "id": "Bintang kutub." },
  { "en": "Polarisasi?", "id": "Pengutuban." },
  { "en": "Polaritas?", "id": "Sifat kutub." },
  { "en": "Polarimeter?", "id": "Pengukur polarisasi." },
  { "en": "Polarimetri?", "id": "Pengukuran polarisasi." },
  { "en": "Poldan?", "id": "Puas (Belanda)." },
  { "en": "Polder?", "id": "Tanah reklamasi." },
  { "en": "Polemik?", "id": "Perdebatan." },
  { "en": "Polen?", "id": "Serbuk sari." },
  { "en": "Polenter?", "id": "Sukarelawan." },
  { "en": "Polero?", "id": "Baju pendek." },
  { "en": "Poles?", "id": "Kilap." },
  { "en": "Polet?", "id": "Tanda pangkat." },
  { "en": "Poli?", "id": "Banyak (awalan)." },
  { "en": "Poliamori?", "id": "Banyak pasangan." },
  { "en": "Poliandri?", "id": "Banyak suami." },
  { "en": "Poliester?", "id": "Serat sintetis." },
  { "en": "Polifagia?", "id": "Makan banyak." },
  { "en": "Polifase?", "id": "Banyak fase." },
  { "en": "Polifiletik?", "id": "Banyak asal." },
  { "en": "Polifonia?", "id": "Musik banyak suara." },
  { "en": "Poligam?", "id": "Banyak istri/suami." },
  { "en": "Poligami?", "id": "Banyak istri." },
  { "en": "Poligini?", "id": "Poligami." },
  { "en": "Poliglot?", "id": "Menguasai banyak bahasa." },
  { "en": "Poligon?", "id": "Segibanyak." },
  { "en": "Poligraf?", "id": "Detektor kebohongan." },
  { "en": "Polihalin?", "id": "Air asin." },
  { "en": "Polihedrin?", "id": "Protein virus." },
  { "en": "Polihedron?", "id": "Bidang banyak." },
  { "en": "Polikel?", "id": "Bagian (arkais)." },
  { "en": "Poliket?", "id": "Cacing." },
  { "en": "Poliklinik?", "id": "Klinik." },
  { "en": "Polikrom?", "id": "Banyak warna." },
  { "en": "Polikultur?", "id": "Banyak jenis tanaman." },
  { "en": "Polimer?", "id": "Molekul rantai." },
  { "en": "Polimerisasi?", "id": "Proses polimer." },
  { "en": "Polinia?", "id": "Daerah es terbuka." },
  { "en": "Polio?", "id": "Penyakit." },
  { "en": "Poliomielitis?", "id": "Polio." },
  { "en": "Polip?", "id": "Tumor." },
  { "en": "Polipeptida?", "id": "Rantai asam amino." },
  { "en": "Polis?", "id": "Kota (Yunani)." },
  { "en": "Polisakarida?", "id": "Karbohidrat kompleks." },
  { "en": "Polisemi?", "id": "Banyak makna." },
  { "en": "Polisena?", "id": "Kain (arkais)." },
  { "en": "Polisiklik?", "id": "Banyak siklus." },
  { "en": "Polisilogisme?", "id": "Silogisme majemuk." },
  { "en": "Polisi?", "id": "Petugas keamanan." },
  { "en": "Polisindeton?", "id": "Gaya bahasa." },
  { "en": "Polispermi?", "id": "Pembuahan banyak sperma." },
  { "en": "Politeis?", "id": "Penganut banyak dewa." },
  { "en": "Politeisme?", "id": "Paham banyak dewa." },
  { "en": "Politeknik?", "id": "Sekolah tinggi teknik." },
  { "en": "Politik?", "id": "Tata negara." },
  { "en": "Politika?", "id": "Masalah politik." },
  { "en": "Politikus?", "id": "Ahli politik." },
  { "en": "Polok?", "id": "Mirip (arkais)." },
  { "en": "Polones?", "id": "Tarian Polandia." },
  { "en": "Polong?", "id": "Buah kacang." },
  { "en": "Polonium?", "id": "Unsur kimia." },
  { "en": "Polos?", "id": "Sederhana." },
  { "en": "Polusi?", "id": "Pencemaran." },
  { "en": "Polutan?", "id": "Penyebab polusi." },
  { "en": "Polutif?", "id": "Menyebabkan polusi." },
  { "en": "Pomade?", "id": "Minyak rambut." },
  { "en": "Pomologi?", "id": "Ilmu buah." },
  { "en": "Pompa?", "id": "Alat sedot." },
  { "en": "Pompon?", "id": "Hiasan benang." },
  { "en": "Pon?", "id": "Satuan berat." },
  { "en": "Poncang?", "id": "Kusut (arkais)." },
  { "en": "Ponco?", "id": "Jas hujan." },
  { "en": "Pondar?", "id": "Berat (arkais)." },
  { "en": "Pondik?", "id": "Rumah kecil (arkais)." },
  { "en": "Pondoh?", "id": "Kelapa." },
  { "en": "Pondok?", "id": "Gubuk." },
  { "en": "Pone?", "id": "Denda (Belanda)." },
  { "en": "Pongah?", "id": "Sombong." },
  { "en": "Pongsu?", "id": "Gundukan tanah." },
  { "en": "Poni?", "id": "Rambut dahi." },
  { "en": "Ponil?", "id": "Kuda kecil." },
  { "en": "Ponok?", "id": "Tonjol (arkais)." },
  { "en": "Pontang-panting?", "id": "Kalang kabut." },
  { "en": "Ponten?", "id": "Nilai." },
  { "en": "Pontif?", "id": "Paus (arkais)." },
  { "en": "Pontifikal?", "id": "Kepausan." },
  { "en": "Ponton?", "id": "Jembatan apung." },
  { "en": "Pop?", "id": "Populer." },
  { "en": "Popeye?", "id": "Tokoh kartun." },
  { "en": "Popi?", "id": "Bunga." },
  { "en": "Poplin?", "id": "Kain." },
  { "en": "Popok?", "id": "Lampin." },
  { "en": "Populer?", "id": "Terkenal." },
  { "en": "Popularisasi?", "id": "Proses populer." },
  { "en": "Popularitas?", "id": "Ketenaran." },
  { "en": "Populasi?", "id": "Jumlah penduduk." },
  { "en": "Porah?", "id": "Terbuka (arkais)." },
  { "en": "Pori?", "id": "Lubang kecil." },
  { "en": "Porno?", "id": "Cabul." },
  { "en": "Pornografi?", "id": "Gambar cabul." },
  { "en": "Porok?", "id": "Garpu." },
  { "en": "Poros?", "id": "Sumbu." },
  { "en": "Porositas?", "id": "Daya serap." },
  { "en": "Porot?", "id": "Kuras." },
  { "en": "Porphyria?", "id": "Penyakit." },
  { "en": "Porselen?", "id": "Keramik." },
  { "en": "Porsi?", "id": "Bagian." },
  { "en": "Portal?", "id": "Gerbang." },
  { "en": "Porter?", "id": "Pengangkat barang." },
  { "en": "Portir?", "id": "Porter." },
  { "en": "Portofolio?", "id": "Kumpulan karya." },
  { "en": "Portugis?", "id": "Portugal." },
  { "en": "Pos?", "id": "Tempat." },
  { "en": "Pose?", "id": "Gaya." },
  { "en": "Posen?", "id": "Daerah." },
  { "en": "Posisi?", "id": "Letak." },
  { "en": "Positif?", "id": "Baik." },
  { "en": "Positivisme?", "id": "Filsafat." },
  { "en": "Positron?", "id": "Partikel." },
  { "en": "Poskar?", "id": "Kartu pos." },
  { "en": "Pospaid?", "id": "Pascabayar." },
  { "en": "Posterior?", "id": "Bagian belakang." },
  { "en": "Postulat?", "id": "Anggapan dasar." },
  { "en": "Postur?", "id": "Perawakan." },
  { "en": "Pot?", "id": "Wadah tanaman." },
  { "en": "Potas?", "id": "Kalium karbonat." },
  { "en": "Potasium?", "id": "Kalium." },
  { "en": "Potator?", "id": "Peminum (alkohol)." },
  { "en": "Potel?", "id": "Patah." },
  { "en": "Potensi?", "id": "Kemampuan." },
  { "en": "Potensial?", "id": "Mungkin." },
  { "en": "Potensiometer?", "id": "Pengukur tegangan." },
  { "en": "Potia?", "id": "Wadah (arkais)." },
  { "en": "Potlot?", "id": "Pensil." },
  { "en": "Potol?", "id": "Potong (arkais)." },
  { "en": "Potong?", "id": "Iris." },
  { "en": "Potpuri?", "id": "Campuran." },
  { "en": "Potret?", "id": "Gambar." },
  { "en": "Pound?", "id": "Satuan berat/mata uang." },
  { "en": "Prah?", "id": "Kapal." },
  { "en": "Prairi?", "id": "Padang rumput." },
  { "en": "Praja?", "id": "Negeri." },
  { "en": "Prajurit?", "id": "Tentara." },
  { "en": "Prakala?", "id": "Prasejarah." },
  { "en": "Prakarsa?", "id": "Inisiatif." },
  { "en": "Prakarya?", "id": "Kerajinan tangan." },
  { "en": "Prakata?", "id": "Pendahuluan." },
  { "en": "Prakiraan?", "id": "Ramalan." },
  { "en": "Prakondisi?", "id": "Syarat awal." },
  { "en": "Praksis?", "id": "Praktik." },
  { "en": "Praktik?", "id": "Pelaksanaan." },
  { "en": "Praktikan?", "id": "Peserta praktik." },
  { "en": "Praktikum?", "id": "Kerja praktik." },
  { "en": "Praktis?", "id": "Mudah." },
  { "en": "Praktisi?", "id": "Pelaksana." },
  { "en": "Pralambang?", "id": "Simbol." },
  { "en": "Pralaya?", "id": "Kiamat (Sanskerta)." },
  { "en": "Pramacitra?", "id": "Citra awal." },
  { "en": "Prambosen?", "id": "Frambusia." },
  { "en": "Pramewara?", "id": "Peragawati." },
  { "en": "Pramlee?", "id": "Aktor Malaysia." },
  { "en": "Pramugara?", "id": "Pelayan pesawat pria." },
  { "en": "Pramugari?", "id": "Pelayan pesawat wanita." },
  { "en": "Pramuka?", "id": "Pandu." },
  { "en": "Pramuniaga?", "id": "Pelayan toko." },
  { "en": "Pramuria?", "id": "Pelayan bar." },
  { "en": "Pramusaji?", "id": "Pelayan." },
  { "en": "Pramuwisata?", "id": "Pemandu wisata." },
  { "en": "Pranala?", "id": "Tautan." },
  { "en": "Pranata?", "id": "Institusi." },
  { "en": "Pranatal?", "id": "Sebelum lahir." },
  { "en": "Prangas?", "id": "Merah padam." },
  { "en": "Pranji?", "id": "Cap (arkais)." },
  { "en": "Prapalatal?", "id": "Bunyi." },
  { "en": "Prapendapat?", "id": "Prasangka." },
  { "en": "Prapromosi?", "id": "Promosi awal." },
  { "en": "Praremaja?", "id": "Menjelang remaja." },
  { "en": "Prarespons?", "id": "Respon awal." },
  { "en": "Prasaja?", "id": "Sederhana." },
  { "en": "Prasangka?", "id": "Dugaan awal." },
  { "en": "Prasaran?", "id": "Pidato." },
  { "en": "Prasarana?", "id": "Infrastruktur." },
  { "en": "Prasasti?", "id": "Tulisan batu." },
  { "en": "Prasawya?", "id": "Berlawanan arah jarum jam." },
  { "en": "Prasejarah?", "id": "Sebelum sejarah." },
  { "en": "Praseminar?", "id": "Seminar awal." },
  { "en": "Prasmanan?", "id": "Sajian hidangan." },
  { "en": "Prastudi?", "id": "Studi awal." },
  { "en": "Pratersier?", "id": "Sebelum zaman Tersier." },
  { "en": "Pratinjau?", "id": "Lihat sekilas." },
  { "en": "Prawacana?", "id": "Kata pengantar." },
  { "en": "Prawira?", "id": "Perwira." },
  { "en": "Prayitna?", "id": "Waspada (Jawa)." },
  { "en": "Prayojana?", "id": "Tujuan (arkais)." },
  { "en": "Prayuwana?", "id": "Selamat (arkais)." },
  { "en": "Pre?", "id": "Sebelum (awalan)." },
  { "en": "Preamplifier?", "id": "Penguat awal." },
  { "en": "Preambul?", "id": "Pembukaan." },
  { "en": "Predasi?", "id": "Pemangsaan." },
  { "en": "Predator?", "id": "Pemangsa." },
  { "en": "Predestinasi?", "id": "Takdir." },
  { "en": "Predikat?", "id": "Bagian kalimat." },
  { "en": "Predikatif?", "id": "Bersifat predikat." },
  { "en": "Prediksi?", "id": "Ramalan." },
  { "en": "Predisposisi?", "id": "Kecenderungan." },
  { "en": "Prefektur?", "id": "Wilayah." },
  { "en": "Preferen?", "id": "Diutamakan." },
  { "en": "Preferensi?", "id": "Pilihan." },
  { "en": "Prefiks?", "id": "Awalan." },
  { "en": "Prehistori?", "id": "Prasejarah." },
  { "en": "Prei?", "id": "Libur (Belanda)." },
  { "en": "Prekositas?", "id": "Kematangan dini." },
  { "en": "Prekursor?", "id": "Pendahulu." },
  { "en": "Prelatur?", "id": "Wilayah gereja." },
  { "en": "Preliminari?", "id": "Pendahuluan." },
  { "en": "Prelir?", "id": "Lirik (arkais)." },
  { "en": "Prelud?", "id": "Musik pembuka." },
  { "en": "Prem?", "id": "Hadiah (Belanda)." },
  { "en": "Prematur?", "id": "Terlalu dini." },
  { "en": "Premeditasi?", "id": "Perencanaan." },
  { "en": "Premi?", "id": "Iuran asuransi." },
  { "en": "Premis?", "id": "Dasar argumen." },
  { "en": "Prenatal?", "id": "Sebelum lahir." },
  { "en": "Preposisi?", "id": "Kata depan." },
  { "en": "Preputium?", "id": "Kulup." },
  { "en": "Prerogatif?", "id": "Hak istimewa." },
  { "en": "Pres?", "id": "Tekan." },
  { "en": "Presbiter?", "id": "Penatua gereja." },
  { "en": "Presbiterian?", "id": "Aliran Kristen." },
  { "en": "Presbiopia?", "id": "Rabun tua." },
  { "en": "Preseden?", "id": "Contoh sebelumnya." },
  { "en": "Presensi?", "id": "Kehadiran." },
  { "en": "Presentabel?", "id": "Layak tampil." },
  { "en": "Presentasi?", "id": "Penyajian." },
  { "en": "Presentil?", "id": "Ukuran statistik." },
  { "en": "Preservasi?", "id": "Pengawetan." },
  { "en": "Presiden?", "id": "Kepala negara." },
  { "en": "Presidensial?", "id": "Berkaitan presiden." },
  { "en": "Presidium?", "id": "Dewan pimpinan." },
  { "en": "Presipitasi?", "id": "Pengendapan." },
  { "en": "Presisi?", "id": "Ketepatan." },
  { "en": "Prestasi?", "id": "Pencapaian." },
  { "en": "Prestise?", "id": "Gengsi." },
  { "en": "Presto?", "id": "Cepat (musik)." },
  { "en": "Presumsi?", "id": "Anggapan." },
  { "en": "Pretensi?", "id": "Pura-pura." },
  { "en": "Preteritum?", "id": "Masa lampau." },
  { "en": "Prevalensi?", "id": "Keluasan." },
  { "en": "Preventif?", "id": "Mencegah." },
  { "en": "Prevensi?", "id": "Pencegahan." },
  { "en": "Preview?", "id": "Pratinjau (Inggris)." },
  { "en": "Pri?", "id": "Peri (arkais)." },
  { "en": "Pria?", "id": "Laki-laki." },
  { "en": "Priagung?", "id": "Bangsawan." },
  { "en": "Priapisme?", "id": "Ereksi lama." },
  { "en": "Pribadi?", "id": "Personal." },
  { "en": "Pribumi?", "id": "Penduduk asli." },
  { "en": "Prihatin?", "id": "Khawatir." },
  { "en": "Prim?", "id": "Nada pertama." },
  { "en": "Prima?", "id": "Utama." },
  { "en": "Primadona?", "id": "Bintang utama." },
  { "en": "Primata?", "id": "Mamalia." },
  { "en": "Primbon?", "id": "Buku ramalan Jawa." },
  { "en": "Primer?", "id": "Utama." },
  { "en": "Primitif?", "id": "Sederhana." },
  { "en": "Primogenitur?", "id": "Hak anak sulung." },
  { "en": "Primordial?", "id": "Paling awal." },
  { "en": "Primordium?", "id": "Bakal organ." },
  { "en": "Primula?", "id": "Bunga." },
  { "en": "Pring?", "id": "Bambu (Jawa)." },
  { "en": "Prinsip?", "id": "Asas." },
  { "en": "Prinsipal?", "id": "Kepala sekolah." },
  { "en": "Printer?", "id": "Pencetak." },
  { "en": "Prioritas?", "id": "Keutamaan." },
  { "en": "Pris?", "id": "Hadiah (Belanda)." },
  { "en": "Prisma?", "id": "Bentuk geometri." },
  { "en": "Prit?", "id": "Seruan." },
  { "en": "Privat?", "id": "Pribadi." },
  { "en": "Privatisasi?", "id": "Pengalihan ke swasta." },
  { "en": "Privilese?", "id": "Hak istimewa." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 4000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
