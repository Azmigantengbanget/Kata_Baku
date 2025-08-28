<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
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
        <h1>Pengetahuan Umum Dasar</h1>
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


{ "en": "Bahasa Baku dari 'Abjat'?", "id": "Abjad." },
{ "en": "Bahasa Baku dari 'Apotik'?", "id": "Apotek." },
{ "en": "Bahasa Baku dari 'Aktip'?", "id": "Aktif." },
{ "en": "Bahasa Baku dari 'Nasehat'?", "id": "Nasihat." },
{ "en": "Bahasa Baku dari 'Resiko'?", "id": "Risiko." },
{ "en": "Bahasa Baku dari 'Jaman'?", "id": "Zaman." },
{ "en": "Bahasa Baku dari 'Praktek'?", "id": "Praktik." },
{ "en": "Bahasa Baku dari 'Ijin'?", "id": "Izin." },
{ "en": "Bahasa Baku dari 'Karir'?", "id": "Karier." },
{ "en": "Bahasa Baku dari 'Pebruari'?", "id": "Februari." },
{ "en": "Bahasa Baku dari 'Nopember'?", "id": "November." },
{ "en": "Bahasa Baku dari 'Atlit'?", "id": "Atlet." },
{ "en": "Bahasa Baku dari 'Coklat'?", "id": "Cokelat." },
{ "en": "Bahasa Baku dari 'Bis'?", "id": "Bus." },
{ "en": "Bahasa Baku dari 'Jadual'?", "id": "Jadwal." },
{ "en": "Bahasa Baku dari 'Kwalitas'?", "id": "Kualitas." },
{ "en": "Bahasa Baku dari 'Obyek'?", "id": "Objek." },
{ "en": "Bahasa Baku dari 'Subyek'?", "id": "Subjek." },
{ "en": "Bahasa Baku dari 'Fikir'?", "id": "Pikir." },
{ "en": "Bahasa Baku dari 'Propinsi'?", "id": "Provinsi." },
{ "en": "Bahasa Baku dari 'Tehnik'?", "id": "Teknik." },
{ "en": "Bahasa Baku dari 'Nafas'?", "id": "Napas." },
{ "en": "Bahasa Baku dari 'Antri'?", "id": "Antre." },
{ "en": "Bahasa Baku dari 'Cabe'?", "id": "Cabai." },
{ "en": "Bahasa Baku dari 'Detil'?", "id": "Detail." },
{ "en": "Bahasa Baku dari 'Doa'?", "id": "Doa." },
{ "en": "Bahasa Baku dari 'Ekstrim'?", "id": "Ekstrem." },
{ "en": "Bahasa Baku dari 'Frasa'?", "id": "Frase." },
{ "en": "Bahasa Baku dari 'Gubug'?", "id": "Gubuk." },
{ "en": "Bahasa Baku dari 'Hadist'?", "id": "Hadis." },
{ "en": "Bahasa Baku dari 'Hapal'?", "id": "Hafal." },
{ "en": "Bahasa Baku dari 'Hirarki'?", "id": "Hierarki." },
{ "en": "Bahasa Baku dari 'Himbau'?", "id": "Imbau." },
{ "en": "Bahasa Baku dari 'Hutang'?", "id": "Utang." },
{ "en": "Bahasa Baku dari 'Ijasah'?", "id": "Ijazah." },
{ "en": "Bahasa Baku dari 'Insyaf'?", "id": "Insaf." },
{ "en": "Bahasa Baku dari 'Isteri'?", "id": "Istri." },
{ "en": "Bahasa Baku dari 'Jendral'?", "id": "Jenderal." },
{ "en": "Bahasa Baku dari 'Jenius'?", "id": "Genius." },
{ "en": "Bahasa Baku dari 'Kaos'?", "id": "Kaus." },
{ "en": "Bahasa Baku dari 'Katagori'?", "id": "Kategori." },
{ "en": "Bahasa Baku dari 'Kaedah'?", "id": "Kaidah." },
{ "en": "Bahasa Baku dari 'Legalisir'?", "id": "Legalisasi." },
{ "en": "Bahasa Baku dari 'Lembab'?", "id": "Lembap." },
{ "en": "Bahasa Baku dari 'Maaf'?", "id": "Maaf." },
{ "en": "Bahasa Baku dari 'Milyar'?", "id": "Miliar." },
{ "en": "Bahasa Baku dari 'Milyuner'?", "id": "Miliuner." },
{ "en": "Bahasa Baku dari 'Nampak'?", "id": "Tampak." },
{ "en": "Bahasa Baku dari 'Obyektif'?", "id": "Objektif." },
{ "en": "Bahasa Baku dari 'Faham'?", "id": "Paham." },
{ "en": "Bahasa Baku dari 'Pasip'?", "id": "Pasif." },
{ "en": "Bahasa Baku dari 'Pondasi'?", "id": "Fondasi." },
{ "en": "Bahasa Baku dari 'Projek'?", "id": "Proyek." },
{ "en": "Bahasa Baku dari 'Putera'?", "id": "Putra." },
{ "en": "Bahasa Baku dari 'Puteri'?", "id": "Putri." },
{ "en": "Bahasa Baku dari 'Ramadhan'?", "id": "Ramadan." },
{ "en": "Bahasa Baku dari 'Rapih'?", "id": "Rapi." },
{ "en": "Bahasa Baku dari 'Respon'?", "id": "Respons." },
{ "en": "Bahasa Baku dari 'Rejeki'?", "id": "Rezeki." },
{ "en": "Bahasa Baku dari 'Rubah'?", "id": "Ubah." },
{ "en": "Bahasa Baku dari 'Saklar'?", "id": "Sakelar." },
{ "en": "Bahasa Baku dari 'Sambel'?", "id": "Sambal." },
{ "en": "Bahasa Baku dari 'Sekedar'?", "id": "Sekadar." },
{ "en": "Bahasa Baku dari 'Silahkan'?", "id": "Silakan." },
{ "en": "Bahasa Baku dari 'Sistim'?", "id": "Sistem." },
{ "en": "Bahasa Baku dari 'Supir'?", "id": "Sopir." },
{ "en": "Bahasa Baku dari 'Standarisasi'?", "id": "Standardisasi." },
{ "en": "Bahasa Baku dari 'Trampil'?", "id": "Terampil." },
{ "en": "Bahasa Baku dari 'Terimakasih'?", "id": "Terima kasih." },
{ "en": "Bahasa Baku dari 'Trotowar'?", "id": "Trotoar." },
{ "en": "Bahasa Baku dari 'Ustad'?", "id": "Ustaz." },
{ "en": "Bahasa Baku dari 'Varitas'?", "id": "Varietas." },
{ "en": "Bahasa Baku dari 'Walikota'?", "id": "Wali kota." },
{ "en": "Bahasa Baku dari 'Akte'?", "id": "Akta." },
{ "en": "Bahasa Baku dari 'Azas'?", "id": "Asas." },
{ "en": "Bahasa Baku dari 'Bangalow'?", "id": "Bungalow." },
{ "en": "Bahasa Baku dari 'Balsem'?", "id": "Balsam." },
{ "en": "Bahasa Baku dari 'Batere'?", "id": "Baterai." },
{ "en": "Bahasa Baku dari 'Cidera'?", "id": "Cedera." },
{ "en": "Bahasa Baku dari 'Defenisi'?", "id": "Definisi." },
{ "en": "Bahasa Baku dari 'Diagnosa'?", "id": "Diagnosis." },
{ "en": "Bahasa Baku dari 'Disain'?", "id": "Desain." },
{ "en": "Bahasa Baku dari 'Eskrim'?", "id": "Es krim." },
{ "en": "Bahasa Baku dari 'Ekwivalen'?", "id": "Ekuivalen." },
{ "en": "Bahasa Baku dari 'Elit'?", "id": "Elite." },
{ "en": "Bahasa Baku dari 'Enerji'?", "id": "Energi." },
{ "en": "Bahasa Baku dari 'Faksimili'?", "id": "Faksimile." },
{ "en": "Bahasa Baku dari 'Formil'?", "id": "Formal." },
{ "en": "Bahasa Baku dari 'Photo'?", "id": "Foto." },
{ "en": "Bahasa Baku dari 'Frekwensi'?", "id": "Frekuensi." },
{ "en": "Bahasa Baku dari 'Ghaib'?", "id": "Gaib." },
{ "en": "Bahasa Baku dari 'Gisi'?", "id": "Gizi." },
{ "en": "Bahasa Baku dari 'Geladi'?", "id": "Gladi." },
{ "en": "Bahasa Baku dari 'Goa'?", "id": "Gua." },
{ "en": "Bahasa Baku dari 'Hakekat'?", "id": "Hakikat." },
{ "en": "Bahasa Baku dari 'Hembus'?", "id": "Embus." },
{ "en": "Bahasa Baku dari 'Hipotesa'?", "id": "Hipotesis." },
{ "en": "Bahasa Baku dari 'Aktifitas'?", "id": "Aktivitas." },
{ "en": "Bahasa Baku dari 'Ambein'?", "id": "Ambeien." },
{ "en": "Bahasa Baku dari 'Ambulan'?", "id": "Ambulans." },
{ "en": "Bahasa Baku dari 'Analisa'?", "id": "Analisis." },
{ "en": "Bahasa Baku dari 'Handal'?", "id": "Andal." },
{ "en": "Bahasa Baku dari 'Antene'?", "id": "Antena." },
{ "en": "Bahasa Baku dari 'Adzan'?", "id": "Azan." },
{ "en": "Bahasa Baku dari 'Belum'?", "id": "Belum." },
{ "en": "Bahasa Baku dari 'Berfikir'?", "id": "Berpikir." },
{ "en": "Bahasa Baku dari 'Blanko'?", "id": "Blangko." },
{ "en": "Bahasa Baku dari 'Blesteran'?", "id": "Blasteran." },
{ "en": "Bahasa Baku dari 'Bok'?", "id": "Boks." },
{ "en": "Bahasa Baku dari 'Bosen'?", "id": "Bosan." },
{ "en": "Bahasa Baku dari 'Brandal'?", "id": "Berandal." },
{ "en": "Bahasa Baku dari 'Cengkeh'?", "id": "Cengkih." },
{ "en": "Bahasa Baku dari 'Cendikiawan'?", "id": "Cendekiawan." },
{ "en": "Bahasa Baku dari 'Daptar'?", "id": "Daftar." },
{ "en": "Bahasa Baku dari 'Debet'?", "id": "Debit." },
{ "en": "Bahasa Baku dari 'Depot'?", "id": "Depo." },
{ "en": "Bahasa Baku dari 'Dollar'?", "id": "Dolar." },
{ "en": "Bahasa Baku dari 'Duren'?", "id": "Durian." },
{ "en": "Bahasa Baku dari 'Efektip'?", "id": "Efektif." },
{ "en": "Bahasa Baku dari 'Epesien'?", "id": "Efisien." },
{ "en": "Bahasa Baku dari 'Eskadron'?", "id": "Skuadron." },
{ "en": "Bahasa Baku dari 'Expor'?", "id": "Ekspor." },
{ "en": "Bahasa Baku dari 'Extra'?", "id": "Ekstra." },
{ "en": "Bahasa Baku dari 'Faksin'?", "id": "Vaksin." },
{ "en": "Bahasa Baku dari 'Pavorit'?", "id": "Favorit." },
{ "en": "Bahasa Baku dari 'Pilem'?", "id": "Film." },
{ "en": "Bahasa Baku dari 'Phisik'?", "id": "Fisik." },
{ "en": "Bahasa Baku dari 'Flatform'?", "id": "Platform." },
{ "en": "Bahasa Baku dari 'Ghoib'?", "id": "Gaib." },
{ "en": "Bahasa Baku dari 'Glonggong'?", "id": "Gelonggong." },
{ "en": "Bahasa Baku dari 'Grebek'?", "id": "Gerebek." },
{ "en": "Bahasa Baku dari 'Group'?", "id": "Grup." },
{ "en": "Bahasa Baku dari 'Gudheg'?", "id": "Gudeg." },
{ "en": "Bahasa Baku dari 'Hektar'?", "id": "Hektare." },
{ "en": "Bahasa Baku dari 'Hena'?", "id": "Inai." },
{ "en": "Bahasa Baku dari 'Hentak'?", "id": "Entak." },
{ "en": "Bahasa Baku dari 'Hihienis'?", "id": "Higienis." },
{ "en": "Bahasa Baku dari 'Idiologi'?", "id": "Ideologi." },
{ "en": "Bahasa Baku dari 'Iklas'?", "id": "Ikhlas." },
{ "en": "Bahasa Baku dari 'Indera'?", "id": "Indra." },
{ "en": "Bahasa Baku dari 'Influensa'?", "id": "Influenza." },
{ "en": "Bahasa Baku dari 'Instink'?", "id": "Insting." },
{ "en": "Bahasa Baku dari 'Interpertasi'?", "id": "Interpretasi." },
{ "en": "Bahasa Baku dari 'Introgasi'?", "id": "Interogasi." },
{ "en": "Bahasa Baku dari 'Hisap'?", "id": "Isap." },
{ "en": "Bahasa Baku dari 'Isya'?", "id": "Isya." },
{ "en": "Bahasa Baku dari 'Jagad'?", "id": "Jagat." },
{ "en": "Bahasa Baku dari 'Jamaah'?", "id": "Jemaah." },
{ "en": "Bahasa Baku dari 'Jenasah'?", "id": "Jenazah." },
{ "en": "Bahasa Baku dari 'Jeep'?", "id": "Jip." },
{ "en": "Bahasa Baku dari 'Jumat'?", "id": "Jumat." },
{ "en": "Bahasa Baku dari 'Junior'?", "id": "Yunior." },
{ "en": "Bahasa Baku dari 'Kaca mata'?", "id": "Kacamata." },
{ "en": "Bahasa Baku dari 'Kangker'?", "id": "Kanker." },
{ "en": "Bahasa Baku dari 'Kharisma'?", "id": "Karisma." },
{ "en": "Bahasa Baku dari 'Katagoris'?", "id": "Kategoris." },
{ "en": "Bahasa Baku dari 'Kendor'?", "id": "Kendur." },
{ "en": "Bahasa Baku dari 'Kuatir'?", "id": "Khawatir." },
{ "en": "Bahasa Baku dari 'Kholifah'?", "id": "Khalifah." },
{ "en": "Bahasa Baku dari 'Kelakson'?", "id": "Klakson." },
{ "en": "Bahasa Baku dari 'Koboy'?", "id": "Koboi." },
{ "en": "Bahasa Baku dari 'Komersil'?", "id": "Komersial." },
{ "en": "Bahasa Baku dari 'Komfirmasi'?", "id": "Konfirmasi." },
{ "en": "Bahasa Baku dari 'Konkrit'?", "id": "Konkret." },
{ "en": "Bahasa Baku dari 'Konsekwen'?", "id": "Konsekuen." },
{ "en": "Bahasa Baku dari 'Kordinasi'?", "id": "Koordinasi." },
{ "en": "Bahasa Baku dari 'Kosa kata'?", "id": "Kosakata." },
{ "en": "Bahasa Baku dari 'Kwantitas'?", "id": "Kuantitas." },
{ "en": "Bahasa Baku dari 'Kwitansi'?", "id": "Kuitansi." },
{ "en": "Bahasa Baku dari 'Laik'?", "id": "Layak." },
{ "en": "Bahasa Baku dari 'Liat'?", "id": "Lihat." },
{ "en": "Bahasa Baku dari 'Lobang'?", "id": "Lubang." },
{ "en": "Bahasa Baku dari 'Losen'?", "id": "Losion." },
{ "en": "Bahasa Baku dari 'Maag'?", "id": "Mag." },
{ "en": "Bahasa Baku dari 'Managemen'?", "id": "Manajemen." },
{ "en": "Bahasa Baku dari 'Mangkok'?", "id": "Mangkuk." },
{ "en": "Bahasa Baku dari 'Marjin'?", "id": "Margin." },
{ "en": "Bahasa Baku dari 'Masal'?", "id": "Massal." },
{ "en": "Bahasa Baku dari 'Mastuli'?", "id": "Mestuli." },
{ "en": "Bahasa Baku dari 'Mateng'?", "id": "Matang." },
{ "en": "Bahasa Baku dari 'Motifasi'?", "id": "Motivasi." },
{ "en": "Bahasa Baku dari 'Mahluk'?", "id": "Makhluk." },
{ "en": "Bahasa Baku dari 'Manager'?", "id": "Manajer." },
{ "en": "Bahasa Baku dari 'Mantera'?", "id": "Mantra." },
{ "en": "Bahasa Baku dari 'Mesjid'?", "id": "Masjid." },
{ "en": "Bahasa Baku dari 'Merk'?", "id": "Merek." },
{ "en": "Bahasa Baku dari 'Methode'?", "id": "Metode." },
{ "en": "Bahasa Baku dari 'Nafas'?", "id": "Napas." },
{ "en": "Bahasa Baku dari 'Negri'?", "id": "Negeri." },
{ "en": "Bahasa Baku dari 'Nomer'?", "id": "Nomor." },
{ "en": "Bahasa Baku dari 'Obyektifitas'?", "id": "Objektivitas." },
{ "en": "Bahasa Baku dari 'Automatis'?", "id": "Otomatis." },
{ "en": "Bahasa Baku dari 'Paragrap'?", "id": "Paragraf." },
{ "en": "Bahasa Baku dari 'Pasport'?", "id": "Paspor." },
{ "en": "Bahasa Baku dari 'Capek'?", "id": "Capai." },
{ "en": "Bahasa Baku dari 'Fihak'?", "id": "Pihak." },
{ "en": "Bahasa Baku dari 'Psikotest'?", "id": "Psikotes." },
{ "en": "Bahasa Baku dari 'Rebo'?", "id": "Rabu." },
{ "en": "Bahasa Baku dari 'Rido'?", "id": "Rida." },
{ "en": "Bahasa Baku dari 'Ristleting'?", "id": "Ritsleting." },
{ "en": "Bahasa Baku dari 'Retail'?", "id": "Ritel." },
{ "en": "Bahasa Baku dari 'Ruh'?", "id": "Roh." },
{ "en": "Bahasa Baku dari 'Sajen'?", "id": "Sesajen." },
{ "en": "Bahasa Baku dari 'Saksama'?", "id": "Seksama." },
{ "en": "Bahasa Baku dari 'Samasekali'?", "id": "Sama sekali." },
{ "en": "Bahasa Baku dari 'Sanksekerta'?", "id": "Sanskerta." },
{ "en": "Bahasa Baku dari 'Saos'?", "id": "Saus." },
{ "en": "Bahasa Baku dari 'Sapu tangan'?", "id": "Saputangan." },
{ "en": "Bahasa Baku dari 'Sariawan'?", "id": "Seriawan." },
{ "en": "Bahasa Baku dari 'Senen'?", "id": "Senin." },
{ "en": "Bahasa Baku dari 'Sentimentil'?", "id": "Sentimental." },
{ "en": "Bahasa Baku dari 'Sirial'?", "id": "Sereal." },
{ "en": "Bahasa Baku dari 'Srigala'?", "id": "Serigala." },
{ "en": "Bahasa Baku dari 'Syetan'?", "id": "Setan." },
{ "en": "Bahasa Baku dari 'Setempel'?", "id": "Stempel." },
{ "en": "Bahasa Baku dari 'Shampo'?", "id": "Sampo." },
{ "en": "Bahasa Baku dari 'Sirop'?", "id": "Sirup." },
{ "en": "Bahasa Baku dari 'Speker'?", "id": "Speaker." },
{ "en": "Bahasa Baku dari 'Sticker'?", "id": "Stiker." },
{ "en": "Bahasa Baku dari 'Stop kontak'?", "id": "Stopkontak." },
{ "en": "Bahasa Baku dari 'Sumatra'?", "id": "Sumatera." },
{ "en": "Bahasa Baku dari 'Sorga'?", "id": "Surga." },
{ "en": "Bahasa Baku dari 'Survey'?", "id": "Survei." },
{ "en": "Bahasa Baku dari 'Takhta'?", "id": "Tahta." },
{ "en": "Bahasa Baku dari 'Tau'?", "id": "Tahu." },
{ "en": "Bahasa Baku dari 'Taoco'?", "id": "Tauco." },
{ "en": "Bahasa Baku dari 'Tape'?", "id": "Tapai." },
{ "en": "Bahasa Baku dari 'Tauladan'?", "id": "Teladan." },
{ "en": "Bahasa Baku dari 'Telpon'?", "id": "Telepon." },
{ "en": "Bahasa Baku dari 'Telor'?", "id": "Telur." },
{ "en": "Bahasa Baku dari 'Tembako'?", "id": "Tembakau." },
{ "en": "Bahasa Baku dari 'Tentram'?", "id": "Tenteram." },
{ "en": "Bahasa Baku dari 'Teoritis'?", "id": "Teoretis." },
{ "en": "Bahasa Baku dari 'Tepok'?", "id": "Tepuk." },
{ "en": "Bahasa Baku dari 'Terong'?", "id": "Terung." },
{ "en": "Bahasa Baku dari 'Team'?", "id": "Tim." },
{ "en": "Bahasa Baku dari 'Taubat'?", "id": "Tobat." },
{ "en": "Bahasa Baku dari 'Toge'?", "id": "Tauge." },
{ "en": "Bahasa Baku dari 'Tuna netra'?", "id": "Tunanetra." },
{ "en": "Bahasa Baku dari 'Udzur'?", "id": "Uzur." },
{ "en": "Bahasa Baku dari 'Ultah'?", "id": "Ulang tahun." },
{ "en": "Bahasa Baku dari 'Onta'?", "id": "Unta." },
{ "en": "Bahasa Baku dari 'Urine'?", "id": "Urin." },
{ "en": "Bahasa Baku dari 'Pidio'?", "id": "Video." },
{ "en": "Bahasa Baku dari 'Villa'?", "id": "Vila." },
{ "en": "Bahasa Baku dari 'Wira swasta'?", "id": "Wiraswasta." },
{ "en": "Bahasa Baku dari 'Yagud'?", "id": "Yahudi." },
{ "en": "Bahasa Baku dari 'Yoghurt'?", "id": "Yogurt." },
{ "en": "Bahasa Baku dari 'Jogyakarta'?", "id": "Yogyakarta." },
{ "en": "Bahasa Baku dari 'Zig-zag'?", "id": "Zigzag." },
{ "en": "Bahasa Baku dari 'Zohor'?", "id": "Zuhur." },
{ "en": "Bahasa Baku dari 'Baligh'?", "id": "Balig." },
{ "en": "Bahasa Baku dari 'Bangker'?", "id": "Bungker." },
{ "en": "Bahasa Baku dari 'Barzah'?", "id": "Barzakh." },
{ "en": "Bahasa Baku dari 'Bolkam'?", "id": "Bohlam." },
{ "en": "Bahasa Baku dari 'Biosfir'?", "id": "Biosfer." },
{ "en": "Bahasa Baku dari 'Da'i'?", "id": "Dai." },
{ "en": "Bahasa Baku dari 'Dashboard'?", "id": "Dasbor." },
{ "en": "Bahasa Baku dari 'Dekrit'?", "id": "Dekret." },
{ "en": "Bahasa Baku dari 'Deodorant'?", "id": "Deodoran." },
{ "en": "Bahasa Baku dari 'Desertasi'?", "id": "Disertasi." },
{ "en": "Bahasa Baku dari 'Diesel'?", "id": "Disel." },
{ "en": "Bahasa Baku dari 'Diskripsi'?", "id": "Deskripsi." },
{ "en": "Bahasa Baku dari 'Distilasi'?", "id": "Destilasi." },
{ "en": "Bahasa Baku dari 'Dite'?", "id": "Dikte." },
{ "en": "Bahasa Baku dari 'Devisi'?", "id": "Divisi." },
{ "en": "Bahasa Baku dari 'Donkrak'?", "id": "Dongkrak." },
{ "en": "Bahasa Baku dari 'Fakultatip'?", "id": "Fakultatif." },
{ "en": "Bahasa Baku dari 'Februari'?", "id": "Februari." },
{ "en": "Bahasa Baku dari 'Finis'?", "id": "Finish." },
{ "en": "Bahasa Baku dari 'Genteng'?", "id": "Genting." },
{ "en": "Bahasa Baku dari 'Gip'?", "id": "Gips." },
{ "en": "Bahasa Baku dari 'Gres'?", "id": "Gres." },
{ "en": "Bahasa Baku dari 'Got'?", "id": "Selokan." },
{ "en": "Bahasa Baku dari 'Hempas'?", "id": "Empas." },
{ "en": "Bahasa Baku dari 'Histeris'?", "id": "Histeris." },
{ "en": "Bahasa Baku dari 'Homogen'?", "id": "Homogen." },
{ "en": "Bahasa Baku dari 'Imaginasi'?", "id": "Imajinasi." },
{ "en": "Bahasa Baku dari 'Incognito'?", "id": "Inkonito." },
{ "en": "Bahasa Baku dari 'Individuil'?", "id": "Individual." },
{ "en": "Bahasa Baku dari 'Inteligen'?", "id": "Intelijen." },
{ "en": "Bahasa Baku dari 'Intens'?", "id": "Inten." },
{ "en": "Bahasa Baku dari 'Itensif'?", "id": "Intensif." },
{ "en": "Bahasa Baku dari 'Jadul'?", "id": "Zaman dahulu." },
{ "en": "Bahasa Baku dari 'Jelek'?", "id": "Jelek." },
{ "en": "Bahasa Baku dari 'Jip'?", "id": "Jip." },
{ "en": "Bahasa Baku dari 'Justeru'?", "id": "Justru." },
{ "en": "Bahasa Baku dari 'Kafeteria'?", "id": "Kafetaria." },
{ "en": "Bahasa Baku dari 'Kangguru'?", "id": "Kanguru." },
{ "en": "Bahasa Baku dari 'Kantong'?", "id": "Kantung." },
{ "en": "Bahasa Baku dari 'Katholik'?", "id": "Katolik." },
{ "en": "Bahasa Baku dari 'Kesatria'?", "id": "Ksatria." },
{ "en": "Bahasa Baku dari 'Khatulistiwa'?", "id": "Katulistiwa." },
{ "en": "Bahasa Baku dari 'Komplek'?", "id": "Kompleks." },
{ "en": "Bahasa Baku dari 'Kleb'?", "id": "Klub." },
{ "en": "Bahasa Baku dari 'Kolumnis'?", "id": "Kolomnis." },
{ "en": "Bahasa Baku dari 'Kondite'?", "id": "Konduite." },
{ "en": "Bahasa Baku dari 'Konggres'?", "id": "Kongres." },
{ "en": "Bahasa Baku dari 'Korna'?", "id": "Kornea." },
{ "en": "Bahasa Baku dari 'Korp'?", "id": "Korps." },
{ "en": "Bahasa Baku dari 'Krem'?", "id": "Krim." },
{ "en": "Bahasa Baku dari 'Kreatifitas'?", "id": "Kreativitas." },
{ "en": "Bahasa Baku dari 'Kriminil'?", "id": "Kriminal." },
{ "en": "Bahasa Baku dari 'Kropos'?", "id": "Keropos." },
{ "en": "Bahasa Baku dari 'Kwadran'?", "id": "Kuadran." },
{ "en": "Bahasa Baku dari 'Kwalifikasi'?", "id": "Kualifikasi." },
{ "en": "Bahasa Baku dari 'Qubur'?", "id": "Kubur." },
{ "en": "Bahasa Baku dari 'Kuno'?", "id": "Kuna." },
{ "en": "Bahasa Baku dari 'Lansekap'?", "id": "Lanskap." },
{ "en": "Bahasa Baku dari 'Lagenda'?", "id": "Legenda." },
{ "en": "Bahasa Baku dari 'Limpa'?", "id": "Limfa." },
{ "en": "Bahasa Baku dari 'Lini masa'?", "id": "Linimasa." },
{ "en": "Bahasa Baku dari 'Lokalisir'?", "id": "Lokalisasi." },
{ "en": "Bahasa Baku dari 'Longmarch'?", "id": "Longmars." },
{ "en": "Bahasa Baku dari 'Lotere'?", "id": "Lotre." },
{ "en": "Bahasa Baku dari 'Maghnet'?", "id": "Magnet." },
{ "en": "Bahasa Baku dari 'Makcomblang'?", "id": "Mak comblang." },
{ "en": "Bahasa Baku dari 'Makdum'?", "id": "Makhdum." },
{ "en": "Bahasa Baku dari 'Maximal'?", "id": "Maksimal." },
{ "en": "Bahasa Baku dari 'Malapraktek'?", "id": "Malapraktik." },
{ "en": "Bahasa Baku dari 'Mandeg'?", "id": "Mandek." },
{ "en": "Bahasa Baku dari 'Mahzab'?", "id": "Mazhab." },
{ "en": "Bahasa Baku dari 'Menyolok'?", "id": "Mencolok." },
{ "en": "Bahasa Baku dari 'Merubah'?", "id": "Mengubah." },
{ "en": "Bahasa Baku dari 'Mupakat'?", "id": "Mufakat." },
{ "en": "Bahasa Baku dari 'Munkar'?", "id": "Mungkar." },
{ "en": "Bahasa Baku dari 'Musium'?", "id": "Museum." },
{ "en": "Bahasa Baku dari 'Napsu'?", "id": "Nafsu." },
{ "en": "Bahasa Baku dari 'Nenas'?", "id": "Nanas." },
{ "en": "Bahasa Baku dari 'Nara sumber'?", "id": "Narasumber." },
{ "en": "Bahasa Baku dari 'Netralisir'?", "id": "Netralisasi." },
{ "en": "Bahasa Baku dari 'Netto'?", "id": "Neto." },
{ "en": "Bahasa Baku dari 'Ngeliat'?", "id": "Melihat." },
{ "en": "Bahasa Baku dari 'Nggak'?", "id": "Tidak." },
{ "en": "Bahasa Baku dari 'Ni'mat'?", "id": "Nikmat." },
{ "en": "Bahasa Baku dari 'Oktav'?", "id": "Oktaf." },
{ "en": "Bahasa Baku dari 'Olah raga'?", "id": "Olahraga." },
{ "en": "Bahasa Baku dari 'Omset'?", "id": "Omzet." },
{ "en": "Bahasa Baku dari 'Original'?", "id": "Orisinal." },
{ "en": "Bahasa Baku dari 'Ortu'?", "id": "Orang tua." },
{ "en": "Bahasa Baku dari 'Otopsi'?", "id": "Autopsi." },
{ "en": "Bahasa Baku dari 'Pacet'?", "id": "Pacat." },
{ "en": "Bahasa Baku dari 'Palm'?", "id": "Palem." },
{ "en": "Bahasa Baku dari 'Pano'?", "id": "Panu." },
{ "en": "Bahasa Baku dari 'Pararel'?", "id": "Paralel." },
{ "en": "Bahasa Baku dari 'Paramedis'?", "id": "Paramedik." },
{ "en": "Bahasa Baku dari 'Partikelir'?", "id": "Partikulir." },
{ "en": "Bahasa Baku dari 'Paspoto'?", "id": "Pasfoto." },
{ "en": "Bahasa Baku dari 'Pavilyun'?", "id": "Paviliun." },
{ "en": "Bahasa Baku dari 'Peliara'?", "id": "Pelihara." },
{ "en": "Bahasa Baku dari 'Pemukiman'?", "id": "Permukiman." },
{ "en": "Bahasa Baku dari 'Pengelihatan'?", "id": "Penglihatan." },
{ "en": "Bahasa Baku dari 'Bregedel'?", "id": "Perkedel." },
{ "en": "Bahasa Baku dari 'Prosen'?", "id": "Persen." },
{ "en": "Bahasa Baku dari 'Piramid'?", "id": "Piramida." },
{ "en": "Bahasa Baku dari 'Plapon'?", "id": "Plafon." },
{ "en": "Bahasa Baku dari 'Plaster'?", "id": "Plester." },
{ "en": "Bahasa Baku dari 'Pro aktif'?", "id": "Proaktif." },
{ "en": "Bahasa Baku dari 'Proklamir'?", "id": "Proklamasi." },
{ "en": "Bahasa Baku dari 'Podeng'?", "id": "Puding." },
{ "en": "Bahasa Baku dari 'Rame'?", "id": "Ramai." },
{ "en": "Bahasa Baku dari 'Rasionil'?", "id": "Rasional." },
{ "en": "Bahasa Baku dari 'Rehap'?", "id": "Rehabilitasi." },
{ "en": "Bahasa Baku dari 'Riel'?", "id": "Riil." },
{ "en": "Bahasa Baku dari 'Rubuh'?", "id": "Roboh." },
{ "en": "Bahasa Baku dari 'Salfok'?", "id": "Salah fokus." },
{ "en": "Bahasa Baku dari 'Sampek'?", "id": "Sampai." },
{ "en": "Bahasa Baku dari 'Selep'?", "id": "Selip." },
{ "en": "Bahasa Baku dari 'Sembrawut'?", "id": "Semrawut." },
{ "en": "Bahasa Baku dari 'Sendal'?", "id": "Sandal." },
{ "en": "Bahasa Baku dari 'Sengkata'?", "id": "Sengketa." },
{ "en": "Bahasa Baku dari 'Sepray'?", "id": "Seprai." },
{ "en": "Bahasa Baku dari 'Suling'?", "id": "Seruling." },
{ "en": "Bahasa Baku dari 'Setres'?", "id": "Stres." },
{ "en": "Bahasa Baku dari 'Sholat'?", "id": "Salat." },
{ "en": "Bahasa Baku dari 'Sepur'?", "id": "Spor." },
{ "en": "Bahasa Baku dari 'Standart'?", "id": "Standar." },
{ "en": "Bahasa Baku dari 'Stelan'?", "id": "Setelan." },
{ "en": "Bahasa Baku dari 'Sutera'?", "id": "Sutra." },
{ "en": "Bahasa Baku dari 'Syahdu'?", "id": "Sahdu." },
{ "en": "Bahasa Baku dari 'Tandatangan'?", "id": "Tanda tangan." },
{ "en": "Bahasa Baku dari 'Tapsir'?", "id": "Tafsir." },
{ "en": "Bahasa Baku dari 'Tekat'?", "id": "Tekad." },
{ "en": "Bahasa Baku dari 'Terlanjur'?", "id": "Telanjur." },
{ "en": "Bahasa Baku dari 'Trilyun'?", "id": "Triliun." },
{ "en": "Bahasa Baku dari 'Trophi'?", "id": "Trofi." },
{ "en": "Bahasa Baku dari 'Ubah'?", "id": "Ubah." },
{ "en": "Bahasa Baku dari 'Ustadzah'?", "id": "Ustazah." },
{ "en": "Bahasa Baku dari 'Vakum'?", "id": "Vakum." },
{ "en": "Bahasa Baku dari 'Valuta'?", "id": "Valuta." },
{ "en": "Bahasa Baku dari 'Vermak'?", "id": "Permak." },
{ "en": "Bahasa Baku dari 'Volum'?", "id": "Volume." },
{ "en": "Bahasa Baku dari 'Waskom'?", "id": "Baskom." },
{ "en": "Bahasa Baku dari 'Wujud'?", "id": "Wujud." },
{ "en": "Bahasa Baku dari 'Yudikatip'?", "id": "Yudikatif." },
{ "en": "Bahasa Baku dari 'Zuhud'?", "id": "Zuhud." },
{ "en": "Bahasa Baku dari 'Al Quran'?", "id": "Al-Qur'an." },
{ "en": "Bahasa Baku dari 'Amphibi'?", "id": "Amfibi." },
{ "en": "Bahasa Baku dari 'Baut'?", "id": "Baud." },
{ "en": "Bahasa Baku dari 'Becermin'?", "id": "Bercermin." },
{ "en": "Bahasa Baku dari 'Besok'?", "id": "Esok." },
{ "en": "Bahasa Baku dari 'Beternak'?", "id": "Berternak." },
{ "en": "Bahasa Baku dari 'Bhineka'?", "id": "Bhinneka." },
{ "en": "Bahasa Baku dari 'Cedera'?", "id": "Cedera." },
{ "en": "Bahasa Baku dari 'Cengengesan'?", "id": "Cengengesan." },
{ "en": "Bahasa Baku dari 'Cumicumi'?", "id": "Cumi-cumi." },
{ "en": "Bahasa Baku dari 'Dahsyat'?", "id": "Dahsyat." },
{ "en": "Bahasa Baku dari 'Derajad'?", "id": "Derajat." },
{ "en": "Bahasa Baku dari 'Design'?", "id": "Desain." },
{ "en": "Bahasa Baku dari 'Destilasi'?", "id": "Distilasi." },
{ "en": "Bahasa Baku dari 'Detasemen'?", "id": "Detasemen." },
{ "en": "Bahasa Baku dari 'Dingdong'?", "id": "Ding dong." },
{ "en": "Bahasa Baku dari 'Ditemukan'?", "id": "Ditemukan." },
{ "en": "Bahasa Baku dari 'Do'a'?", "id": "Doa." },
{ "en": "Bahasa Baku dari 'Dramatisir'?", "id": "Dramatisasi." },
{ "en": "Bahasa Baku dari 'Duku'?", "id": "Duku." },
{ "en": "Bahasa Baku dari 'Eksim'?", "id": "Eksem." },
{ "en": "Bahasa Baku dari 'Eksport'?", "id": "Ekspor." },
{ "en": "Bahasa Baku dari 'Elips'?", "id": "Elips." },
{ "en": "Bahasa Baku dari 'Empek-empek'?", "id": "Pempek." },
{ "en": "Bahasa Baku dari 'Entri'?", "id": "Entri." },
{ "en": "Bahasa Baku dari 'Faedah'?", "id": "Faedah." },
{ "en": "Bahasa Baku dari 'Fantasi'?", "id": "Fantasi." },
{ "en": "Bahasa Baku dari 'Farmakop'?", "id": "Farmakope." },
{ "en": "Bahasa Baku dari 'Fiasko'?", "id": "Fiasco." },
{ "en": "Bahasa Baku dari 'Flanel'?", "id": "Fanel." },
{ "en": "Bahasa Baku dari 'Folio'?", "id": "Folio." },
{ "en": "Bahasa Baku dari 'Formalin'?", "id": "Formalin." },
{ "en": "Bahasa Baku dari 'Fosil'?", "id": "Fosil." },
{ "en": "Bahasa Baku dari 'Frustasi'?", "id": "Frustrasi." },
{ "en": "Bahasa Baku dari 'Gado-gado'?", "id": "Gado-gado." },
{ "en": "Bahasa Baku dari 'Galer'?", "id": "Galeri." },
{ "en": "Bahasa Baku dari 'Gamma'?", "id": "Gama." },
{ "en": "Bahasa Baku dari 'Gampang'?", "id": "Mudah." },
{ "en": "Bahasa Baku dari 'Gemetar'?", "id": "Gemeter." },
{ "en": "Bahasa Baku dari 'Gono-gini'?", "id": "Gana-gini." },
{ "en": "Bahasa Baku dari 'Geraham'?", "id": "Graham." },
{ "en": "Bahasa Baku dari 'Griya'?", "id": "Gria." },
{ "en": "Bahasa Baku dari 'Gua'?", "id": "Gua." },
{ "en": "Bahasa Baku dari 'Gudang'?", "id": "Gudang." },
{ "en": "Bahasa Baku dari 'Gulai'?", "id": "Gule." },
{ "en": "Bahasa Baku dari 'Hempas'?", "id": "Empas." },
{ "en": "Bahasa Baku dari 'Henset'?", "id": "Peranti telinga." },
{ "en": "Bahasa Baku dari 'Herpes'?", "id": "Herpes." },
{ "en": "Bahasa Baku dari 'Hestek'?", "id": "Tagar." },
{ "en": "Bahasa Baku dari 'Hyper'?", "id": "Hiper." },
{ "en": "Bahasa Baku dari 'Imbau'?", "id": "Imbau." },
{ "en": "Bahasa Baku dari 'Impor'?", "id": "Impor." },
{ "en": "Bahasa Baku dari 'Indeks'?", "id": "Indeks." },
{ "en": "Bahasa Baku dari 'Innalillahi'?", "id": "Inalilahi." },
{ "en": "Bahasa Baku dari 'Inovasi'?", "id": "Inovasi." },
{ "en": "Bahasa Baku dari 'Intel'?", "id": "Intel." },
{ "en": "Bahasa Baku dari 'Interogasi'?", "id": "Interogasi." },
{ "en": "Bahasa Baku dari 'Introspeksi'?", "id": "Introspeksi." },
{ "en": "Bahasa Baku dari 'Jagung'?", "id": "Jagung." },
{ "en": "Bahasa Baku dari 'Jahiliyah'?", "id": "Jahiliah." },
{ "en": "Bahasa Baku dari 'Jaring'?", "id": "Jaring." },
{ "en": "Bahasa Baku dari 'Jas'?", "id": "Jas." },
{ "en": "Bahasa Baku dari 'Jemaat'?", "id": "Jemaah." },
{ "en": "Bahasa Baku dari 'Jip'?", "id": "Jip." },
{ "en": "Bahasa Baku dari 'Jujur'?", "id": "Jujur." },
{ "en": "Bahasa Baku dari 'Jurnal'?", "id": "Jurnal." },
{ "en": "Bahasa Baku dari 'Kaidah'?", "id": "Kaidah." },
{ "en": "Bahasa Baku dari 'Kalkulator'?", "id": "Kalkulator." },
{ "en": "Bahasa Baku dari 'Kamboja'?", "id": "Kamboja." },
{ "en": "Bahasa Baku dari 'Kanal'?", "id": "Kanal." },
{ "en": "Bahasa Baku dari 'Kapal'?", "id": "Kapal." },
{ "en": "Bahasa Baku dari 'Kapten'?", "id": "Kapten." },
{ "en": "Bahasa Baku dari 'Karisma'?", "id": "Karisma." },
{ "en": "Bahasa Baku dari 'Karnaval'?", "id": "Karnaval." },
{ "en": "Bahasa Baku dari 'Kasat mata'?", "id": "Kasat mata." },
{ "en": "Bahasa Baku dari 'Kasur'?", "id": "Kasur." },
{ "en": "Bahasa Baku dari 'Katalog'?", "id": "Katalog." },
{ "en": "Bahasa Baku dari 'Kecoa'?", "id": "Kecoa." },
{ "en": "Bahasa Baku dari 'Kedaluwarsa'?", "id": "Kedaluwarsa." },
{ "en": "Bahasa Baku dari 'Keji'?", "id": "Keji." },
{ "en": "Bahasa Baku dari 'Kejutan'?", "id": "Kejutan." },
{ "en": "Bahasa Baku dari 'Kelar'?", "id": "Selesai." },
{ "en": "Bahasa Baku dari 'Kemeja'?", "id": "Kemeja." },
{ "en": "Bahasa Baku dari 'Kendaraan'?", "id": "Kendaraan." },
{ "en": "Bahasa Baku dari 'Kereta'?", "id": "Kereta." },
{ "en": "Bahasa Baku dari 'Keripik'?", "id": "Keripik." },
{ "en": "Bahasa Baku dari 'Kenek'?", "id": "Kernet." },
{ "en": "Bahasa Baku dari 'Kere'?", "id": "Miskin." },
{ "en": "Bahasa Baku dari 'Keroncong'?", "id": "Kroncong." },
{ "en": "Bahasa Baku dari 'Kesamping'?", "id": "Ke samping." },
{ "en": "Bahasa Baku dari 'Kesihatan'?", "id": "Kesehatan." },
{ "en": "Bahasa Baku dari 'Kesturi'?", "id": "Kasturi." },
{ "en": "Bahasa Baku dari 'Ketapel'?", "id": "Katapel." },
{ "en": "Bahasa Baku dari 'Kethoprak'?", "id": "Ketoprak." },
{ "en": "Bahasa Baku dari 'Kyai'?", "id": "Kiai." },
{ "en": "Bahasa Baku dari 'Komoditi'?", "id": "Komoditas." },
{ "en": "Bahasa Baku dari 'Kondusip'?", "id": "Kondusif." },
{ "en": "Bahasa Baku dari 'Konseleting'?", "id": "Korsleting." },
{ "en": "Bahasa Baku dari 'Konserpatip'?", "id": "Konservatif." },
{ "en": "Bahasa Baku dari 'Konslet'?", "id": "Korsleting." },
{ "en": "Bahasa Baku dari 'Kontinyu'?", "id": "Kontinu." },
{ "en": "Bahasa Baku dari 'Kopeling'?", "id": "Kopling." },
{ "en": "Bahasa Baku dari 'Kos'?", "id": "Indekos." },
{ "en": "Bahasa Baku dari 'Kosmonot'?", "id": "Kosmonaut." },
{ "en": "Bahasa Baku dari 'Kran'?", "id": "Keran." },
{ "en": "Bahasa Baku dari 'Kreatip'?", "id": "Kreatif." },
{ "en": "Bahasa Baku dari 'Keredit'?", "id": "Kredit." },
{ "en": "Bahasa Baku dari 'Krew'?", "id": "Kru." },
{ "en": "Bahasa Baku dari 'Krismon'?", "id": "Krisis moneter." },
{ "en": "Bahasa Baku dari 'Kulintang'?", "id": "Kolintang." },
{ "en": "Bahasa Baku dari 'Lafal'?", "id": "Lafaz." },
{ "en": "Bahasa Baku dari 'Lajim'?", "id": "Lazim." },
{ "en": "Bahasa Baku dari 'Laminating'?", "id": "Laminasi." },
{ "en": "Bahasa Baku dari 'Leptop'?", "id": "Laptop." },
{ "en": "Bahasa Baku dari 'Leukimia'?", "id": "Leukemia." },
{ "en": "Bahasa Baku dari 'Lift'?", "id": "Lif." },
{ "en": "Bahasa Baku dari 'Limousine'?", "id": "Limusin." },
{ "en": "Bahasa Baku dari 'Linier'?", "id": "Linear." },
{ "en": "Bahasa Baku dari 'Maha karya'?", "id": "Mahakarya." },
{ "en": "Bahasa Baku dari 'Majlis'?", "id": "Majelis." },
{ "en": "Bahasa Baku dari 'Makam'?", "id": "Makam." },
{ "en": "Bahasa Baku dari 'Maksimum'?", "id": "Maksimum." },
{ "en": "Bahasa Baku dari 'Makro'?", "id": "Makro." },
{ "en": "Bahasa Baku dari 'Malaikat'?", "id": "Malaikat." },
{ "en": "Bahasa Baku dari 'Manekin'?", "id": "Maneken." },
{ "en": "Bahasa Baku dari 'Mandarin'?", "id": "Mandarin." },
{ "en": "Bahasa Baku dari 'Manejer'?", "id": "Manajer." },
{ "en": "Bahasa Baku dari 'Mangkubumi'?", "id": "Mangkubumi." },
{ "en": "Bahasa Baku dari 'Manoever'?", "id": "Manuver." },
{ "en": "Bahasa Baku dari 'Mantab'?", "id": "Mantap." },
{ "en": "Bahasa Baku dari 'Manusiawi'?", "id": "Manusiawi." },
{ "en": "Bahasa Baku dari 'Marathon'?", "id": "Maraton." },
{ "en": "Bahasa Baku dari 'Margarine'?", "id": "Margarin." },
{ "en": "Bahasa Baku dari 'Masa'?", "id": "Massa." },
{ "en": "Bahasa Baku dari 'Masak'?", "id": "Masak." },
{ "en": "Bahasa Baku dari 'Maskot'?", "id": "Maskot." },
{ "en": "Bahasa Baku dari 'Master'?", "id": "Master." },
{ "en": "Bahasa Baku dari 'Megap-megap'?", "id": "Megap-megap." },
{ "en": "Bahasa Baku dari 'Mekanik'?", "id": "Mekanik." },
{ "en": "Bahasa Baku dari 'Mekanisme'?", "id": "Mekanisme." },
{ "en": "Bahasa Baku dari 'Menaati'?", "id": "Menaati." },
{ "en": "Bahasa Baku dari 'Mencolok'?", "id": "Menyolok." },
{ "en": "Bahasa Baku dari 'Mendeskripsikan'?", "id": "Mendeskripsikan." },
{ "en": "Bahasa Baku dari 'Mendiang'?", "id": "Mendiang." },
{ "en": "Bahasa Baku dari 'Mendzolimi'?", "id": "Menzalimi." },
{ "en": "Bahasa Baku dari 'Mensiasati'?", "id": "Menyiasati." },
{ "en": "Bahasa Baku dari 'Mensukseskan'?", "id": "Menyukseskan." },
{ "en": "Bahasa Baku dari 'Mental'?", "id": "Mental." },
{ "en": "Bahasa Baku dari 'Menthol'?", "id": "Mentol." },
{ "en": "Bahasa Baku dari 'Mestika'?", "id": "Mustika." },
{ "en": "Bahasa Baku dari 'Meteor'?", "id": "Meteor." },
{ "en": "Bahasa Baku dari 'Meteri'?", "id": "Materi." },
{ "en": "Bahasa Baku dari 'Mie'?", "id": "Mi." },
{ "en": "Bahasa Baku dari 'Mikro'?", "id": "Mikro." },
{ "en": "Bahasa Baku dari 'Milyader'?", "id": "Miliarder." },
{ "en": "Bahasa Baku dari 'Minder'?", "id": "Minder." },
{ "en": "Bahasa Baku dari 'Missi'?", "id": "Misi." },
{ "en": "Bahasa Baku dari 'Misteri'?", "id": "Misteri." },
{ "en": "Bahasa Baku dari 'Mistik'?", "id": "Mistik." },
{ "en": "Bahasa Baku dari 'Mitos'?", "id": "Mitos." },
{ "en": "Bahasa Baku dari 'Mixer'?", "id": "Mikser." },
{ "en": "Bahasa Baku dari 'Modal'?", "id": "Modal." },
{ "en": "Bahasa Baku dari 'Moderen'?", "id": "Modern." },
{ "en": "Bahasa Baku dari 'Monarki'?", "id": "Monarki." },
{ "en": "Bahasa Baku dari 'Mongslet'?", "id": "Mogok." },
{ "en": "Bahasa Baku dari 'Monogami'?", "id": "Monogami." },
{ "en": "Bahasa Baku dari 'Montage'?", "id": "Montase." },
{ "en": "Bahasa Baku dari 'Morgan'?", "id": "Murgan." },
{ "en": "Bahasa Baku dari 'Mosaik'?", "id": "Mozaik." },
{ "en": "Bahasa Baku dari 'Muadzin'?", "id": "Muazin." },
{ "en": "Bahasa Baku dari 'Mubazir'?", "id": "Mubazir." },
{ "en": "Bahasa Baku dari 'Muin'?", "id": "Muin." },
{ "en": "Bahasa Baku dari 'Multi'?", "id": "Multi." },
{ "en": "Bahasa Baku dari 'Mumi'?", "id": "Mumi." },
{ "en": "Bahasa Baku dari 'Muntaber'?", "id": "Muntah berak." },
{ "en": "Bahasa Baku dari 'Mursid'?", "id": "Mursyid." },
{ "en": "Bahasa Baku dari 'Musafir'?", "id": "Musafir." },
{ "en": "Bahasa Baku dari 'Musholla'?", "id": "Musala." },
{ "en": "Bahasa Baku dari 'Mutakhir'?", "id": "Mutakhir." },
{ "en": "Bahasa Baku dari 'Mutlak'?", "id": "Mutlak." },
{ "en": "Bahasa Baku dari 'Nampak'?", "id": "Tampak." },
{ "en": "Bahasa Baku dari 'Narkoba'?", "id": "Narkotika." },
{ "en": "Bahasa Baku dari 'Narapidana'?", "id": "Narapidana." },
{ "en": "Bahasa Baku dari 'Nazar'?", "id": "Nadar." },
{ "en": "Bahasa Baku dari 'Nego'?", "id": "Negosiasi." },
{ "en": "Bahasa Baku dari 'Nipas'?", "id": "Nifas." },
{ "en": "Bahasa Baku dari 'Non aktif'?", "id": "Nonaktif." },
{ "en": "Bahasa Baku dari 'Notulen'?", "id": "Notula." },
{ "en": "Bahasa Baku dari 'Opname'?", "id": "Rekam medis." },
{ "en": "Bahasa Baku dari 'Odol'?", "id": "Pasta gigi." },
{ "en": "Bahasa Baku dari 'Olah TKP'?", "id": "Olah tempat kejadian perkara." },
{ "en": "Bahasa Baku dari 'Omlet'?", "id": "Omelet." },
{ "en": "Bahasa Baku dari 'Oplosan'?", "id": "Campuran." },
{ "en": "Bahasa Baku dari 'Orangtua'?", "id": "Orang tua." },
{ "en": "Bahasa Baku dari 'Organisir'?", "id": "Mengorganisasi." },
{ "en": "Bahasa Baku dari 'Oper'?", "id": "Over." },
{ "en": "Bahasa Baku dari 'Pedepokan'?", "id": "Padepokan." },
{ "en": "Bahasa Baku dari 'Paderi'?", "id": "Padri." },
{ "en": "Bahasa Baku dari 'Pajama'?", "id": "Piama." },
{ "en": "Bahasa Baku dari 'Palaeolitikum'?", "id": "Paleolitikum." },
{ "en": "Bahasa Baku dari 'Pantopel'?", "id": "Pantofel." },
{ "en": "Bahasa Baku dari 'Parpum'?", "id": "Parfum." },
{ "en": "Bahasa Baku dari 'Paska'?", "id": "Pasca." },
{ "en": "Bahasa Baku dari 'Pasen'?", "id": "Pasien." },
{ "en": "Bahasa Baku dari 'Patri'?", "id": "Pateri." },
{ "en": "Bahasa Baku dari 'Pausa'?", "id": "Pause." },
{ "en": "Bahasa Baku dari 'Perduli'?", "id": "Peduli." },
{ "en": "Bahasa Baku dari 'Pembaharuan'?", "id": "Pembaruan." },
{ "en": "Bahasa Baku dari 'Penasehat'?", "id": "Penasihat." },
{ "en": "Bahasa Baku dari 'Pinsil'?", "id": "Pensil." },
{ "en": "Bahasa Baku dari 'Penterjemah'?", "id": "Penerjemah." },
{ "en": "Bahasa Baku dari 'Pengrajin'?", "id": "Perajin." },
{ "en": "Bahasa Baku dari 'Prangko'?", "id": "Perangko." },
{ "en": "Bahasa Baku dari 'Perseneling'?", "id": "Persneling." },
{ "en": "Bahasa Baku dari 'Pesen'?", "id": "Pesan." },
{ "en": "Bahasa Baku dari 'Piara'?", "id": "Pelihara." },
{ "en": "Bahasa Baku dari 'Plang'?", "id": "Papan nama." },
{ "en": "Bahasa Baku dari 'Plat'?", "id": "Pelat." },
{ "en": "Bahasa Baku dari 'Plinplan'?", "id": "Plin-plan." },
{ "en": "Bahasa Baku dari 'Pelintir'?", "id": "Plintir." },
{ "en": "Bahasa Baku dari 'Pelitur'?", "id": "Plitur." },
{ "en": "Bahasa Baku dari 'Polister'?", "id": "Poliester." },
{ "en": "Bahasa Baku dari 'Puntang-panting'?", "id": "Pontang-panting." },
{ "en": "Bahasa Baku dari 'Posesip'?", "id": "Posesif." },
{ "en": "Bahasa Baku dari 'Positip'?", "id": "Positif." },
{ "en": "Bahasa Baku dari 'Potkasting'?", "id": "Siniar." },
{ "en": "Bahasa Baku dari 'Pre-'?", "id": "Pra-." },
{ "en": "Bahasa Baku dari 'Presdir'?", "id": "Presiden direktur." },
{ "en": "Bahasa Baku dari 'Periper'?", "id": "Periferal." },
{ "en": "Bahasa Baku dari 'Printer'?", "id": "Pencetak." },
{ "en": "Bahasa Baku dari 'Propit'?", "id": "Profit." },
{ "en": "Bahasa Baku dari 'Psycho'?", "id": "Psiko." },
{ "en": "Bahasa Baku dari 'Puber'?", "id": "Akil balig." },
{ "en": "Bahasa Baku dari 'Pundung'?", "id": "Merajuk." },
{ "en": "Bahasa Baku dari 'Qasidah'?", "id": "Kasidah." },
{ "en": "Bahasa Baku dari 'Realisir'?", "id": "Realisasi." },
{ "en": "Bahasa Baku dari 'Resleting'?", "id": "Ritsleting." },
{ "en": "Bahasa Baku dari 'Restouran'?", "id": "Restoran." },
{ "en": "Bahasa Baku dari 'Resume'?", "id": "Ringkasan." },
{ "en": "Bahasa Baku dari 'Robotik'?", "id": "Robotika." },
{ "en": "Bahasa Baku dari 'Ronsen'?", "id": "Rontgen." },
{ "en": "Bahasa Baku dari 'Royalitas'?", "id": "Royalti." },
{ "en": "Bahasa Baku dari 'Salesma'?", "id": "Selesma." },
{ "en": "Bahasa Baku dari 'Sate'?", "id": "Satai." },
{ "en": "Bahasa Baku dari 'Skedul'?", "id": "Jadwal." },
{ "en": "Bahasa Baku dari 'Science'?", "id": "Sains." },
{ "en": "Bahasa Baku dari 'Sebar luaskan'?", "id": "Sebarluaskan." },
{ "en": "Bahasa Baku dari 'Sadakah'?", "id": "Sedekah." },
{ "en": "Bahasa Baku dari 'Segi tiga'?", "id": "Segitiga." },
{ "en": "Bahasa Baku dari 'Sejarahwan'?", "id": "Sejarawan." },
{ "en": "Bahasa Baku dari 'Sekring'?", "id": "Sekering." },
{ "en": "Bahasa Baku dari 'Sekertaris'?", "id": "Sekretaris." },
{ "en": "Bahasa Baku dari 'Selinder'?", "id": "Silinder." },
{ "en": "Bahasa Baku dari 'Selulose'?", "id": "Selulosa." },
{ "en": "Bahasa Baku dari 'Senapati'?", "id": "Senopati." },
{ "en": "Bahasa Baku dari 'Sentra'?", "id": "Sentral." },
{ "en": "Bahasa Baku dari 'Sertipikat'?", "id": "Sertifikat." },
{ "en": "Bahasa Baku dari 'Service'?", "id": "Servis." },
{ "en": "Bahasa Baku dari 'Stir'?", "id": "Setir." },
{ "en": "Bahasa Baku dari 'Setoples'?", "id": "Stoples." },
{ "en": "Bahasa Baku dari 'Sharia'?", "id": "Syariah." },
{ "en": "Bahasa Baku dari 'Sidin'?", "id": "Dia." },
{ "en": "Bahasa Baku dari 'Simple'?", "id": "Simpel." },
{ "en": "Bahasa Baku dari 'Sop'?", "id": "Sup." },
{ "en": "Bahasa Baku dari 'Spirituil'?", "id": "Spiritual." },
{ "en": "Bahasa Baku dari 'Sreg'?", "id": "Sesuai." },
{ "en": "Bahasa Baku dari 'Stock'?", "id": "Stok." },
{ "en": "Bahasa Baku dari 'Stripping'?", "id": "Peretelan." },
{ "en": "Bahasa Baku dari 'Strukturil'?", "id": "Struktural." },
{ "en": "Bahasa Baku dari 'Subsitusi'?", "id": "Substitusi." },
{ "en": "Bahasa Baku dari 'Sumpek'?", "id": "Sesak." },
{ "en": "Bahasa Baku dari 'Super market'?", "id": "Pasar swalayan." },
{ "en": "Bahasa Baku dari 'Sur'?", "id": "Senang." },
{ "en": "Bahasa Baku dari 'Surah'?", "id": "Surat." },
{ "en": "Bahasa Baku dari 'Swanggi'?", "id": "Suangi." },
{ "en": "Bahasa Baku dari 'Sahbandar'?", "id": "Syahbandar." },
{ "en": "Bahasa Baku dari 'Tabligh'?", "id": "Tablig." },
{ "en": "Bahasa Baku dari 'Tachometer'?", "id": "Takometer." },
{ "en": "Bahasa Baku dari 'Tahayul'?", "id": "Takhayul." },
{ "en": "Bahasa Baku dari 'Tandon'?", "id": "Tandon." },
{ "en": "Bahasa Baku dari 'Tangker'?", "id": "Tanker." },
{ "en": "Bahasa Baku dari 'Taplak'?", "id": "Taplak." },
{ "en": "Bahasa Baku dari 'Taqwa'?", "id": "Takwa." },
{ "en": "Bahasa Baku dari 'Taro'?", "id": "Talas." },
{ "en": "Bahasa Baku dari 'Tatto'?", "id": "Tato." },
{ "en": "Bahasa Baku dari 'Tengkurep'?", "id": "Tengkurap." },
{ "en": "Bahasa Baku dari 'Tengsin'?", "id": "Malu." },
{ "en": "Bahasa Baku dari 'Tepekong'?", "id": "Kelenteng." },
{ "en": "Bahasa Baku dari 'Test'?", "id": "Tes." },
{ "en": "Bahasa Baku dari 'Theater'?", "id": "Teater." },
{ "en": "Bahasa Baku dari 'Tipe-X'?", "id": "Pita koreksi." },
{ "en": "Bahasa Baku dari 'Tips'?", "id": "Tip." },
{ "en": "Bahasa Baku dari 'Tivi'?", "id": "Televisi." },
{ "en": "Bahasa Baku dari 'Tolerir'?", "id": "Toleransi." },
{ "en": "Bahasa Baku dari 'Tour'?", "id": "Tur." },
{ "en": "Bahasa Baku dari 'Towing'?", "id": "Derek." },
{ "en": "Bahasa Baku dari 'Tralis'?", "id": "Terali." },
{ "en": "Bahasa Baku dari 'Transpor'?", "id": "Transportasi." },
{ "en": "Bahasa Baku dari 'Tripang'?", "id": "Teripang." },
{ "en": "Bahasa Baku dari 'Type'?", "id": "Tipe." },
{ "en": "Bahasa Baku dari 'Umroh'?", "id": "Umrah." },
{ "en": "Bahasa Baku dari 'Uniporm'?", "id": "Seragam." },
{ "en": "Bahasa Baku dari 'Vandel'?", "id": "Panji." },
{ "en": "Bahasa Baku dari 'Vegitarian'?", "id": "Vegetarian." },
{ "en": "Bahasa Baku dari 'Vetsin'?", "id": "Penyedap rasa." },
{ "en": "Bahasa Baku dari 'Vignet'?", "id": "Vinyet." },
{ "en": "Bahasa Baku dari 'Visuil'?", "id": "Visual." },
{ "en": "Bahasa Baku dari 'Vokabuler'?", "id": "Vokabular." },
{ "en": "Bahasa Baku dari 'Volley'?", "id": "Voli." },
{ "en": "Bahasa Baku dari 'Voucher'?", "id": "Voucer." },
{ "en": "Bahasa Baku dari 'Waffer'?", "id": "Wafer." },
{ "en": "Bahasa Baku dari 'Washtafel'?", "id": "Wastafel." },
{ "en": "Bahasa Baku dari 'Web site'?", "id": "Website." },
{ "en": "Bahasa Baku dari 'Whisky'?", "id": "Wiski." },
{ "en": "Bahasa Baku dari 'Wirid'?", "id": "Wirit." },
{ "en": "Bahasa Baku dari 'Xerofit'?", "id": "Serofit." },
{ "en": "Bahasa Baku dari 'Xilofon'?", "id": "Silofon." },
{ "en": "Bahasa Baku dari 'Dzalim'?", "id": "Zalim." },
{ "en": "Bahasa Baku dari 'Zam-zam'?", "id": "Zamzam." },
{ "en": "Bahasa Baku dari 'Zone'?", "id": "Zona." },
{ "en": "Bahasa Baku dari 'Absen (dalam arti tidak masuk)'?", "id": "Tidak hadir." },
{ "en": "Bahasa Baku dari 'Adi busana'?", "id": "Adibusana." },
{ "en": "Bahasa Baku dari 'Alhamdullilah'?", "id": "Alhamdulillah." },
{ "en": "Bahasa Baku dari 'Al-kitab'?", "id": "Alkitab." },
{ "en": "Bahasa Baku dari 'Alpokat'?", "id": "Alpukat." },
{ "en": "Bahasa Baku dari 'Antar kota'?", "id": "Antarkota." },
{ "en": "Bahasa Baku dari 'Anti biotik'?", "id": "Antibiotik." },
{ "en": "Bahasa Baku dari 'Apalagi'?", "id": "Apa lagi." },
{ "en": "Bahasa Baku dari 'Audio visual'?", "id": "Audiovisual." },
{ "en": "Bahasa Baku dari 'Bagaimana pun'?", "id": "Bagaimanapun." },
{ "en": "Bahasa Baku dari 'Bahu-membahu'?", "id": "Bahu-membahu." },
{ "en": "Bahasa Baku dari 'Bala tentara'?", "id": "Balatentara." },
{ "en": "Bahasa Baku dari 'Balu'?", "id": "Janda." },
{ "en": "Bahasa Baku dari 'Bamsae'?", "id": "Balsem." },
{ "en": "Bahasa Baku dari 'Barangkali'?", "id": "Barangkali." },
{ "en": "Bahasa Baku dari 'Beasiswa'?", "id": "Beasiswa." },
{ "en": "Bahasa Baku dari 'Bela sungkawa'?", "id": "Belasungkawa." },
{ "en": "Bahasa Baku dari 'Bemper'?", "id": "Bumper." },
{ "en": "Bahasa Baku dari 'Bengkoang'?", "id": "Bengkuang." },
{ "en": "Bahasa Baku dari 'Beres'?", "id": "Rapi." },
{ "en": "Bahasa Baku dari 'Bihun'?", "id": "Mihun." },
{ "en": "Bahasa Baku dari 'Bilyar'?", "id": "Biliar." },
{ "en": "Bahasa Baku dari 'Bina raga'?", "id": "Binaraga." },
{ "en": "Bahasa Baku dari 'Bio data'?", "id": "Biodata." },
{ "en": "Bahasa Baku dari 'Blantika'?", "id": "Belantika." },
{ "en": "Bahasa Baku dari 'Bombardir'?", "id": "Bom bardemen." },
{ "en": "Bahasa Baku dari 'Bowling'?", "id": "Bolin." },
{ "en": "Bahasa Baku dari 'Braket'?", "id": "Behel." },
{ "en": "Bahasa Baku dari 'Brokoli'?", "id": "Brokoli." },
{ "en": "Bahasa Baku dari 'Cabo'?", "id": "Cabau." },
{ "en": "Bahasa Baku dari 'Cakra wala'?", "id": "Cakrawala." },
{ "en": "Bahasa Baku dari 'Campur tangan'?", "id": "Campur tangan." },
{ "en": "Bahasa Baku dari 'Cari tahu'?", "id": "Cari tahu." },
{ "en": "Bahasa Baku dari 'Casse'?", "id": "Sasis." },
{ "en": "Bahasa Baku dari 'Catwalk'?", "id": "Peragawan." },
{ "en": "Bahasa Baku dari 'Cekak'?", "id": "Cekak." },
{ "en": "Bahasa Baku dari 'Ceking'?", "id": "Kurus." },
{ "en": "Bahasa Baku dari 'Celoteh'?", "id": "Celoteh." },
{ "en": "Bahasa Baku dari 'Cemilan'?", "id": "Kudapan." },
{ "en": "Bahasa Baku dari 'Cendol'?", "id": "Cendol." },
{ "en": "Bahasa Baku dari 'Centeng'?", "id": "Centeng." },
{ "en": "Bahasa Baku dari 'Ceramah'?", "id": "Ceramah." },
{ "en": "Bahasa Baku dari 'Cipika-cipiki'?", "id": "Cipika-cipiki." },
{ "en": "Bahasa Baku dari 'Clipping'?", "id": "Kliping." },
{ "en": "Bahasa Baku dari 'Closing'?", "id": "Penutupan." },
{ "en": "Bahasa Baku dari 'Cok'?", "id": "Colok." },
{ "en": "Bahasa Baku dari 'Combro'?", "id": "Comro." },
{ "en": "Bahasa Baku dari 'Cool'?", "id": "Keren." },
{ "en": "Bahasa Baku dari 'Copy'?", "id": "Salin." },
{ "en": "Bahasa Baku dari 'Cowok'?", "id": "Laki-laki." },
{ "en": "Bahasa Baku dari 'Cuci mata'?", "id": "Cuci mata." },
{ "en": "Bahasa Baku dari 'Cuek'?", "id": "Tidak peduli." },
{ "en": "Bahasa Baku dari 'Curhat'?", "id": "Curahan hati." },
{ "en": "Bahasa Baku dari 'Dah'?", "id": "Sudah." },
{ "en": "Bahasa Baku dari 'Damprat'?", "id": "Damprat." },
{ "en": "Bahasa Baku dari 'Darma wisata'?", "id": "Darmawisata." },
{ "en": "Bahasa Baku dari 'Darurat'?", "id": "Darurat." },
{ "en": "Bahasa Baku dari 'Debitor'?", "id": "Debitur." },
{ "en": "Bahasa Baku dari 'Degan'?", "id": "Kelapa muda." },
{ "en": "Bahasa Baku dari 'Dekor'?", "id": "Dekorasi." },
{ "en": "Bahasa Baku dari 'Demisionar'?", "id": "Demisioner." },
{ "en": "Bahasa Baku dari 'Denger'?", "id": "Dengar." },
{ "en": "Bahasa Baku dari 'Depkolektor'?", "id": "Penagih utang." },
{ "en": "Bahasa Baku dari 'Detektip'?", "id": "Detektif." },
{ "en": "Bahasa Baku dari 'Deviden'?", "id": "Dividen." },
{ "en": "Bahasa Baku dari 'Dharma'?", "id": "Darma." },
{ "en": "Bahasa Baku dari 'Dimana-mana'?", "id": "Di mana-mana." },
{ "en": "Bahasa Baku dari 'Dipteri'?", "id": "Difteri." },
{ "en": "Bahasa Baku dari 'Dikarenakan'?", "id": "Karena." },
{ "en": "Bahasa Baku dari 'Dim sam'?", "id": "Dimsum." },
{ "en": "Bahasa Baku dari 'Dino saurus'?", "id": "Dinosaurus." },
{ "en": "Bahasa Baku dari 'Diskotik'?", "id": "Diskotek." },
{ "en": "Bahasa Baku dari 'Distiributor'?", "id": "Distributor." },
{ "en": "Bahasa Baku dari 'Dobel'?", "id": "Ganda." },
{ "en": "Bahasa Baku dari 'Doku'?", "id": "Uang." },
{ "en": "Bahasa Baku dari 'Downlod'?", "id": "Unduh." },
{ "en": "Bahasa Baku dari 'Draft'?", "id": "Draf." },
{ "en": "Bahasa Baku dari 'Dram'?", "id": "Drum." },
{ "en": "Bahasa Baku dari 'Duka cita'?", "id": "Dukacita." },
{ "en": "Bahasa Baku dari 'Duit'?", "id": "Uang." },
{ "en": "Bahasa Baku dari 'Dubes'?", "id": "Duta besar." },
{ "en": "Bahasa Baku dari 'Email'?", "id": "Surel." },
{ "en": "Bahasa Baku dari 'Ecek-ecek'?", "id": "Pura-pura." },
{ "en": "Bahasa Baku dari 'Eksklusip'?", "id": "Eksklusif." },
{ "en": "Bahasa Baku dari 'Eksprimen'?", "id": "Eksperimen." },
{ "en": "Bahasa Baku dari 'Extrinsik'?", "id": "Ekstrinsik." },
{ "en": "Bahasa Baku dari 'Embriyo'?", "id": "Embrio." },
{ "en": "Bahasa Baku dari 'Endog'?", "id": "Telur." },
{ "en": "Bahasa Baku dari 'Enjiniring'?", "id": "Rekayasa." },
{ "en": "Bahasa Baku dari 'Ensikopedia'?", "id": "Ensiklopedi." },
{ "en": "Bahasa Baku dari 'Enter'?", "id": "Masuk." },
{ "en": "Bahasa Baku dari 'Episod'?", "id": "Episode." },
{ "en": "Bahasa Baku dari 'Exit'?", "id": "Keluar." },
{ "en": "Bahasa Baku dari 'Fak'?", "id": "Fakultas." },
{ "en": "Bahasa Baku dari 'Fas'?", "id": "Fase." },
{ "en": "Bahasa Baku dari 'Permentasi'?", "id": "Fermentasi." },
{ "en": "Bahasa Baku dari 'Fiks'?", "id": "Pasti." },
{ "en": "Bahasa Baku dari 'Fit'?", "id": "Bugar." },
{ "en": "Bahasa Baku dari 'Flashdisk'?", "id": "Diska lepas." },
{ "en": "Bahasa Baku dari 'Flat'?", "id": "Datar." },
{ "en": "Bahasa Baku dari 'Football'?", "id": "Sepak bola." },
{ "en": "Bahasa Baku dari 'Forcasting'?", "id": "Prakiraan." },
{ "en": "Bahasa Baku dari 'Phosil'?", "id": "Fosil." },
{ "en": "Bahasa Baku dari 'Fundamen'?", "id": "Fondamen." },
{ "en": "Bahasa Baku dari 'Galaktose'?", "id": "Galaktosa." },
{ "en": "Bahasa Baku dari 'Geladi resik'?", "id": "Gladi resik." },
{ "en": "Bahasa Baku dari 'Geleper'?", "id": "Menggelepar." },
{ "en": "Bahasa Baku dari 'Gempor'?", "id": "Lumpuh." },
{ "en": "Bahasa Baku dari 'Geneologi'?", "id": "Genealogi." },
{ "en": "Bahasa Baku dari 'Geographi'?", "id": "Geografi." },
{ "en": "Bahasa Baku dari 'Getok'?", "id": "Mengetuk." },
{ "en": "Bahasa Baku dari 'GICO'?", "id": "Gerakan 30 September." },
{ "en": "Bahasa Baku dari 'Gila'?", "id": "Tidak waras." },
{ "en": "Bahasa Baku dari 'Gim'?", "id": "Permainan." },
{ "en": "Bahasa Baku dari 'Gladi kotor'?", "id": "Gladi kotor." },
{ "en": "Bahasa Baku dari 'Glondong'?", "id": "Gelondong." },
{ "en": "Bahasa Baku dari 'Glosary'?", "id": "Glosarium." },
{ "en": "Bahasa Baku dari 'Gol'?", "id": "Gol." },
{ "en": "Bahasa Baku dari 'Gonta-ganti'?", "id": "Gonta-ganti." },
{ "en": "Bahasa Baku dari 'Gong'?", "id": "Gong." },
{ "en": "Bahasa Baku dari 'Gonggong'?", "id": "Gonggong." },
{ "en": "Bahasa Baku dari 'Gorden'?", "id": "Horden." },
{ "en": "Bahasa Baku dari 'Gowes'?", "id": "Bersepeda." },
{ "en": "Bahasa Baku dari 'Gram'?", "id": "Gram." },
{ "en": "Bahasa Baku dari 'Grebek'?", "id": "Gerebek." },
{ "en": "Bahasa Baku dari 'Griya'?", "id": "Griya." },
{ "en": "Bahasa Baku dari 'Grogi'?", "id": "Gugup." },
{ "en": "Bahasa Baku dari 'Gua'?", "id": "Gua." },
{ "en": "Bahasa Baku dari 'Gudeg'?", "id": "Gudeg." },
{ "en": "Bahasa Baku dari 'Gugling'?", "id": "Mencari di Google." },
{ "en": "Bahasa Baku dari 'Hacker'?", "id": "Peretas." },
{ "en": "Bahasa Baku dari 'Half'?", "id": "Setengah." },
{ "en": "Bahasa Baku dari 'Handsfree'?", "id": "Peranti bebas genggam." },
{ "en": "Bahasa Baku dari 'Handuk'?", "id": "Handuk." },
{ "en": "Bahasa Baku dari 'Hantem'?", "id": "Hantam." },
{ "en": "Bahasa Baku dari 'Happening'?", "id": "Kejadian." },
{ "en": "Bahasa Baku dari 'Happy'?", "id": "Senang." },
{ "en": "Bahasa Baku dari 'Harddisk'?", "id": "Cakram keras." },
{ "en": "Bahasa Baku dari 'Hardware'?", "id": "Perangkat keras." },
{ "en": "Bahasa Baku dari 'Harmonis'?", "id": "Harmonis." },
{ "en": "Bahasa Baku dari 'Harta'?", "id": "Harta." },
{ "en": "Bahasa Baku dari 'HBH'?", "id": "Halalbihalal." },
{ "en": "Bahasa Baku dari 'Hello'?", "id": "Halo." },
{ "en": "Bahasa Baku dari 'Helm'?", "id": "Helm." },
{ "en": "Bahasa Baku dari 'Hemat'?", "id": "Hemat." },
{ "en": "Bahasa Baku dari 'Hentai'?", "id": "Hentai." },
{ "en": "Bahasa Baku dari 'Heran'?", "id": "Heran." },
{ "en": "Bahasa Baku dari 'Heroin'?", "id": "Heroin." },
{ "en": "Bahasa Baku dari 'HET'?", "id": "Harga eceran tertinggi." },
{ "en": "Bahasa Baku dari 'Heteroseksual'?", "id": "Heteroseksual." },
{ "en": "Bahasa Baku dari 'Hinggap'?", "id": "Hinggap." },
{ "en": "Bahasa Baku dari 'Hip-hop'?", "id": "Hip-hop." },
{ "en": "Bahasa Baku dari 'Hipnotis'?", "id": "Hipnosis." },
{ "en": "Bahasa Baku dari 'Hiu'?", "id": "Ikan hiu." },
{ "en": "Bahasa Baku dari 'Hoby'?", "id": "Hobi." },
{ "en": "Bahasa Baku dari 'Holigan'?", "id": "Huligan." },
{ "en": "Bahasa Baku dari 'Homestay'?", "id": "Pondok wisata." },
{ "en": "Bahasa Baku dari 'Hoax'?", "id": "Hoaks." },
{ "en": "Bahasa Baku dari 'Hotline'?", "id": "Lini panas." },
{ "en": "Bahasa Baku dari 'Hunting'?", "id": "Berburu." },
{ "en": "Bahasa Baku dari 'Hybrid'?", "id": "Hibrida." },
{ "en": "Bahasa Baku dari 'Hydro'?", "id": "Hidro." },
{ "en": "Bahasa Baku dari 'Idul Fitri'?", "id": "Idulfitri." },
{ "en": "Bahasa Baku dari 'Ilfeel'?", "id": "Hilang perasaan." },
{ "en": "Bahasa Baku dari 'Impeachment'?", "id": "Pemakzulan." },
{ "en": "Bahasa Baku dari 'Input'?", "id": "Masukan." },
{ "en": "Bahasa Baku dari 'Insha Allah'?", "id": "Insyaallah." },
{ "en": "Bahasa Baku dari 'Instal'?", "id": "Pasang." },
{ "en": "Bahasa Baku dari 'Intermezo'?", "id": "Intermeso." },
{ "en": "Bahasa Baku dari 'Invest'?", "id": "Investasi." },
{ "en": "Bahasa Baku dari 'Isyu'?", "id": "Isu." },
{ "en": "Bahasa Baku dari 'Isolatip'?", "id": "Isolatif." },
{ "en": "Bahasa Baku dari 'Isro' mi'roj'?", "id": "Isra Mikraj." },
{ "en": "Bahasa Baku dari 'Jaim'?", "id": "Jaga citra." },
{ "en": "Bahasa Baku dari 'Jarkom'?", "id": "Jaringan komunikasi." },
{ "en": "Bahasa Baku dari 'Jelous'?", "id": "Cemburu." },
{ "en": "Bahasa Baku dari 'Jerigen'?", "id": "Jeriken." },
{ "en": "Bahasa Baku dari 'Jersei'?", "id": "Jersi." },
{ "en": "Bahasa Baku dari 'Joging'?", "id": "Jalan santai." },
{ "en": "Bahasa Baku dari 'Join'?", "id": "Bergabung." },
{ "en": "Bahasa Baku dari 'Jomblo'?", "id": "Lajang." },
{ "en": "Bahasa Baku dari 'Jubir'?", "id": "Juru bicara." },
{ "en": "Bahasa Baku dari 'Julid'?", "id": "Dengki." },
{ "en": "Bahasa Baku dari 'Jumping'?", "id": "Melompat." },
{ "en": "Bahasa Baku dari 'Juris'?", "id": "Yuris." },
{ "en": "Bahasa Baku dari 'Kaget'?", "id": "Terkejut." },
{ "en": "Bahasa Baku dari 'Kalo'?", "id": "Kalau." },
{ "en": "Bahasa Baku dari 'Kamseupay'?", "id": "Kampungan." },
{ "en": "Bahasa Baku dari 'Kap'?", "id": "Tudung lampu." },
{ "en": "Bahasa Baku dari 'Kare'?", "id": "Kari." },
{ "en": "Bahasa Baku dari 'Karoseri'?", "id": "Karoseri." },
{ "en": "Bahasa Baku dari 'Kasti'?", "id": "Kasti." },
{ "en": "Bahasa Baku dari 'Katalog'?", "id": "Katalog." },
{ "en": "Bahasa Baku dari 'Katekisasi'?", "id": "Katekisasi." },
{ "en": "Bahasa Baku dari 'Kaya'?", "id": "Kaya." },
{ "en": "Bahasa Baku dari 'Kayangan'?", "id": "Kahyangan." },
{ "en": "Bahasa Baku dari 'Kebab'?", "id": "Kebab." },
{ "en": "Bahasa Baku dari 'Kebiri'?", "id": "Kebiri." },
{ "en": "Bahasa Baku dari 'Kecantol'?", "id": "Terkait." },
{ "en": "Bahasa Baku dari 'Kecubung'?", "id": "Kecubung." },
{ "en": "Bahasa Baku dari 'Kedele'?", "id": "Kedelai." },
{ "en": "Bahasa Baku dari 'Kedengeran'?", "id": "Terdengar." },
{ "en": "Bahasa Baku dari 'Kegep'?", "id": "Tertangkap basah." },
{ "en": "Bahasa Baku dari 'Kejedot'?", "id": "Terbentur." },
{ "en": "Bahasa Baku dari 'Kekeh'?", "id": "Kukuh." },
{ "en": "Bahasa Baku dari 'Kelepek'?", "id": "Tergeletak." },
{ "en": "Bahasa Baku dari 'Kelewat'?", "id": "Terlalu." },
{ "en": "Bahasa Baku dari 'Kelinci'?", "id": "Kelinci." },
{ "en": "Bahasa Baku dari 'Kelolosan'?", "id": "Lolos." },
{ "en": "Bahasa Baku dari 'Keluarga'?", "id": "Keluarga." },
{ "en": "Bahasa Baku dari 'Kempot'?", "id": "Kempis." },
{ "en": "Bahasa Baku dari 'Kemping'?", "id": "Berkemah." },
{ "en": "Bahasa Baku dari 'Kemplang'?", "id": "Kemplang." },
{ "en": "Bahasa Baku dari 'Kempes'?", "id": "Kempis." },
{ "en": "Bahasa Baku dari 'Kenalpot'?", "id": "Knalpot." },
{ "en": "Bahasa Baku dari 'Kendi'?", "id": "Kendi." },
{ "en": "Bahasa Baku dari 'Kepikiran'?", "id": "Terpikirkan." },
{ "en": "Bahasa Baku dari 'Kepo'?", "id": "Ingin tahu." },
{ "en": "Bahasa Baku dari 'Kera'?", "id": "Monyet." },
{ "en": "Bahasa Baku dari 'Keramik'?", "id": "Keramik." },
{ "en": "Bahasa Baku dari 'Kere'?", "id": "Miskin." },
{ "en": "Bahasa Baku dari 'Kerkop'?", "id": "Pekuburan." },
{ "en": "Bahasa Baku dari 'Kernet'?", "id": "Kernet." },
{ "en": "Bahasa Baku dari 'Keruan'?", "id": "Tentu." },
{ "en": "Bahasa Baku dari 'Kesengsem'?", "id": "Terpesona." },
{ "en": "Bahasa Baku dari 'Kesleo'?", "id": "Terkilir." },
{ "en": "Bahasa Baku dari 'Ketauan'?", "id": "Ketahuan." },
{ "en": "Bahasa Baku dari 'Ketemu'?", "id": "Bertemu." },
{ "en": "Bahasa Baku dari 'Ketik'?", "id": "Tik." },
{ "en": "Bahasa Baku dari 'Ketok'?", "id": "Ketuk." },
{ "en": "Bahasa Baku dari 'Keval'?", "id": "Kefal." },
{ "en": "Bahasa Baku dari 'Kilo'?", "id": "Kilogram." },
{ "en": "Bahasa Baku dari 'Kimono'?", "id": "Kimono." },
{ "en": "Bahasa Baku dari 'Kirain'?", "id": "Kukira." },
{ "en": "Bahasa Baku dari 'Klab'?", "id": "Klub." },
{ "en": "Bahasa Baku dari 'Klarinet'?", "id": "Klarinet." },
{ "en": "Bahasa Baku dari 'Klausa'?", "id": "Klausa." },
{ "en": "Bahasa Baku dari 'Klik'?", "id": "Klik." },
{ "en": "Bahasa Baku dari 'Klise'?", "id": "Klise." },
{ "en": "Bahasa Baku dari 'Kloset'?", "id": "Kloset." },
{ "en": "Bahasa Baku dari 'Kokpit'?", "id": "Kokpit." },
{ "en": "Bahasa Baku dari 'Koktail'?", "id": "Koktail." },
{ "en": "Bahasa Baku dari 'Kolaborasi'?", "id": "Kolaborasi." },
{ "en": "Bahasa Baku dari 'Kolera'?", "id": "Kolera." },
{ "en": "Bahasa Baku dari 'Kolestrol'?", "id": "Kolesterol." },
{ "en": "Bahasa Baku dari 'Kolonel'?", "id": "Kolonel." },
{ "en": "Bahasa Baku dari 'Komedi'?", "id": "Komedi." },
{ "en": "Bahasa Baku dari 'Komisaris'?", "id": "Komisaris." },
{ "en": "Bahasa Baku dari 'Kompeni'?", "id": "Kompi." },
{ "en": "Bahasa Baku dari 'Komplen'?", "id": "Komplain." },
{ "en": "Bahasa Baku dari 'Komplit'?", "id": "Lengkap." },
{ "en": "Bahasa Baku dari 'Komprehensip'?", "id": "Komprehensif." },
{ "en": "Bahasa Baku dari 'Konbes'?", "id": "Konferensi besar." },
{ "en": "Bahasa Baku dari 'Konfrensi'?", "id": "Konferensi." },
{ "en": "Bahasa Baku dari 'Konfrontir'?", "id": "Konfrontasi." },
{ "en": "Bahasa Baku dari 'Konyungtur'?", "id": "Konjungtur." },
{ "en": "Bahasa Baku dari 'Kontra indikasi'?", "id": "Kontraindikasi." },
{ "en": "Bahasa Baku dari 'Kopeah'?", "id": "Kopiah." },
{ "en": "Bahasa Baku dari 'Kop surat'?", "id": "Kepala surat." },
{ "en": "Bahasa Baku dari 'Kopdar'?", "id": "Kopi darat." },
{ "en": "Bahasa Baku dari 'Kopyor'?", "id": "Kopior." },
{ "en": "Bahasa Baku dari 'Koreograpi'?", "id": "Koreografi." },
{ "en": "Bahasa Baku dari 'Kosekan'?", "id": "Kosinus." },
{ "en": "Bahasa Baku dari 'Kostim'?", "id": "Kostum." },
{ "en": "Bahasa Baku dari 'Kotbah'?", "id": "Khotbah." },
{ "en": "Bahasa Baku dari 'Cover'?", "id": "Sampul." },
{ "en": "Bahasa Baku dari 'Kowad'?", "id": "Korps Wanita Angkatan Darat." },
{ "en": "Bahasa Baku dari 'Kramat'?", "id": "Keramat." },
{ "en": "Bahasa Baku dari 'Kreditor'?", "id": "Kreditur." },
{ "en": "Bahasa Baku dari 'Kroscek'?", "id": "Periksa ulang." },
{ "en": "Bahasa Baku dari 'Krupuk'?", "id": "Kerupuk." },
{ "en": "Bahasa Baku dari 'Kwartal'?", "id": "Kuartal." },
{ "en": "Bahasa Baku dari 'Kudrat'?", "id": "Kodrat." },
{ "en": "Bahasa Baku dari 'Kuesionair'?", "id": "Kuesioner." },
{ "en": "Bahasa Baku dari 'Kulonuwun'?", "id": "Permisi." },
{ "en": "Bahasa Baku dari 'Kumat'?", "id": "Kambuh." },
{ "en": "Bahasa Baku dari 'Kumulatip'?", "id": "Kumulatif." },
{ "en": "Bahasa Baku dari 'Kuping'?", "id": "Telinga." },
{ "en": "Bahasa Baku dari 'Kupu'?", "id": "Kupu-kupu." },
{ "en": "Bahasa Baku dari 'Kutang'?", "id": "Pakaian dalam wanita." },
{ "en": "Bahasa Baku dari 'Kutikel'?", "id": "Kutikula." },
{ "en": "Bahasa Baku dari 'Kutup'?", "id": "Kutub." },
{ "en": "Bahasa Baku dari 'La ilaha illallah'?", "id": "Lailahaillallah." },
{ "en": "Bahasa Baku dari 'Launching'?", "id": "Peluncuran." },
{ "en": "Bahasa Baku dari 'Laundry'?", "id": "Penatu." },
{ "en": "Bahasa Baku dari 'Leasing'?", "id": "Sewa guna usaha." },
{ "en": "Bahasa Baku dari 'Lebay'?", "id": "Berlebihan." },
{ "en": "Bahasa Baku dari 'Legistlatif'?", "id": "Legislatif." },
{ "en": "Bahasa Baku dari 'Lillahi'?", "id": "Lillahi." },
{ "en": "Bahasa Baku dari 'Limfa'?", "id": "Limfa." },
{ "en": "Bahasa Baku dari 'Link'?", "id": "Tautan." },
{ "en": "Bahasa Baku dari 'Live'?", "id": "Langsung." },
{ "en": "Bahasa Baku dari 'Loka karya'?", "id": "Lokakarya." },
{ "en": "Bahasa Baku dari 'Losmen'?", "id": "Losmen." },
{ "en": "Bahasa Baku dari 'Lotion'?", "id": "Losion." },
{ "en": "Bahasa Baku dari 'Lowbatt'?", "id": "Baterai lemah." },
{ "en": "Bahasa Baku dari 'Loyal'?", "id": "Setia." },
{ "en": "Bahasa Baku dari 'Mabal'?", "id": "Bolos." },
{ "en": "Bahasa Baku dari 'Macet'?", "id": "Macet." },
{ "en": "Bahasa Baku dari 'Madona'?", "id": "Madonna." },
{ "en": "Bahasa Baku dari 'Madrasah'?", "id": "Madrasah." },
{ "en": "Bahasa Baku dari 'Mafia'?", "id": "Mafia." },
{ "en": "Bahasa Baku dari 'Mahesa'?", "id": "Mahesa." },
{ "en": "Bahasa Baku dari 'Mahnet'?", "id": "Magnet." },
{ "en": "Bahasa Baku dari 'Medsos'?", "id": "Media sosial." },
{ "en": "Bahasa Baku dari 'Mainboard'?", "id": "Papan induk." },
{ "en": "Bahasa Baku dari 'Make up'?", "id": "Tata rias." },
{ "en": "Bahasa Baku dari 'Mako'?", "id": "Markas komando." },
{ "en": "Bahasa Baku dari 'Mbalelo'?", "id": "Membangkang." },
{ "en": "Bahasa Baku dari 'Meeting'?", "id": "Rapat." },
{ "en": "Bahasa Baku dari 'Melulu'?", "id": "Selalu." },
{ "en": "Bahasa Baku dari 'Mencontek'?", "id": "Menyontek." },
{ "en": "Bahasa Baku dari 'Mencolok'?", "id": "Menyolok." },
{ "en": "Bahasa Baku dari 'Mengkomsumsi'?", "id": "Mengonsumsi." },
{ "en": "Bahasa Baku dari 'Mengkritik'?", "id": "Mengkritik." },
{ "en": "Bahasa Baku dari 'Mengubah'?", "id": "Mengubah." },
{ "en": "Bahasa Baku dari 'Menstruasi'?", "id": "Menstruasi." },
{ "en": "Bahasa Baku dari 'Mentri'?", "id": "Menteri." },
{ "en": "Bahasa Baku dari 'Merem'?", "id": "Memenjamkan mata." },
{ "en": "Bahasa Baku dari 'Meteorologi'?", "id": "Meteorologi." },
{ "en": "Bahasa Baku dari 'Miris'?", "id": "Prihatin." },
{ "en": "Bahasa Baku dari 'Mlanding'?", "id": "Lamtoro." },
{ "en": "Bahasa Baku dari 'MOC'?", "id": "Modal." },
{ "en": "Bahasa Baku dari 'Moksa'?", "id": "Moksa." },
{ "en": "Bahasa Baku dari 'Monoril'?", "id": "Monorel." },
{ "en": "Bahasa Baku dari 'Mood'?", "id": "Suasana hati." },
{ "en": "Bahasa Baku dari 'Mortein'?", "id": "Obat nyamuk." },
{ "en": "Bahasa Baku dari 'Motherboard'?", "id": "Papan induk." },
{ "en": "Bahasa Baku dari 'Movie'?", "id": "Film." },
{ "en": "Bahasa Baku dari 'MPR'?", "id": "Majelis Permusyawaratan Rakyat." },
{ "en": "Bahasa Baku dari 'Mufrod'?", "id": "Mufrad." },
{ "en": "Bahasa Baku dari 'Mujijat'?", "id": "Mukjizat." },
{ "en": "Bahasa Baku dari 'Multi media'?", "id": "Multimedia." },
{ "en": "Bahasa Baku dari 'Munas'?", "id": "Musyawarah nasional." },
{ "en": "Bahasa Baku dari 'Musang'?", "id": "Musang." },
{ "en": "Bahasa Baku dari 'Musyrik'?", "id": "Musyrik." },
{ "en": "Bahasa Baku dari 'Naluri'?", "id": "Naluri." },
{ "en": "Bahasa Baku dari 'Naudzubillah'?", "id": "Nauzubillah." },
{ "en": "Bahasa Baku dari 'Nekat'?", "id": "Nekat." },
{ "en": "Bahasa Baku dari 'Ndeso'?", "id": "Kampungan." },
{ "en": "Bahasa Baku dari 'Deadline'?", "id": "Tenggat." },
{ "en": "Bahasa Baku dari 'Ngebut'?", "id": "Mengebut." },
{ "en": "Bahasa Baku dari 'Ngeh'?", "id": "Paham." },
{ "en": "Bahasa Baku dari 'Nge-gym'?", "id": "Berolahraga di gimnasium." },
{ "en": "Bahasa Baku dari 'Nge-share'?", "id": "Membagikan." },
{ "en": "Bahasa Baku dari 'Ngiri'?", "id": "Iri." },
{ "en": "Bahasa Baku dari 'Ngobrol'?", "id": "Bercakap-cakap." },
{ "en": "Bahasa Baku dari 'Ngomong'?", "id": "Berbicara." },
{ "en": "Bahasa Baku dari 'Ngumpet'?", "id": "Bersembunyi." },
{ "en": "Bahasa Baku dari 'Ngutang'?", "id": "Berutang." },
{ "en": "Bahasa Baku dari 'Nipon'?", "id": "Jepang." },
{ "en": "Bahasa Baku dari 'Njelimet'?", "id": "Rumit." },
{ "en": "Bahasa Baku dari 'Non formal'?", "id": "Nonformal." },
{ "en": "Bahasa Baku dari 'Nongkrong'?", "id": "Berkumpul." },
{ "en": "Bahasa Baku dari 'Nostalgi'?", "id": "Nostalgia." },
{ "en": "Bahasa Baku dari 'Nuklius'?", "id": "Nukleus." },
{ "en": "Bahasa Baku dari 'Nyali'?", "id": "Keberanian." },
{ "en": "Bahasa Baku dari 'Nyam-nyam'?", "id": "Lezat." },
{ "en": "Bahasa Baku dari 'Nyeplos'?", "id": "Berbicara tanpa dipikir." },
{ "en": "Bahasa Baku dari 'Nyerah'?", "id": "Menyerah." },
{ "en": "Bahasa Baku dari 'Nyoto'?", "id": "Makan soto." },
{ "en": "Bahasa Baku dari 'Ogah'?", "id": "Tidak mau." },
{ "en": "Bahasa Baku dari 'Ok'?", "id": "Setuju." },
{ "en": "Bahasa Baku dari 'Oke'?", "id": "Baiklah." },
{ "en": "Bahasa Baku dari 'Online'?", "id": "Daring." },
{ "en": "Bahasa Baku dari 'Opa'?", "id": "Kakek." },
{ "en": "Bahasa Baku dari 'Open'?", "id": "Buka." },
{ "en": "Bahasa Baku dari 'Oto didak'?", "id": "Otodidak." },
{ "en": "Bahasa Baku dari 'Out of date'?", "id": "Kedaluwarsa." },
{ "en": "Bahasa Baku dari 'Output'?", "id": "Keluaran." },
{ "en": "Bahasa Baku dari 'Pak'?", "id": "Bapak." },
{ "en": "Bahasa Baku dari 'Pakansi'?", "id": "Vakansi." },
{ "en": "Bahasa Baku dari 'Pampers'?", "id": "Popok." },
{ "en": "Bahasa Baku dari 'Pansos'?", "id": "Panjat sosial." },
{ "en": "Bahasa Baku dari 'Paparazzi'?", "id": "Paparazi." },
{ "en": "Bahasa Baku dari 'Paralel gram'?", "id": "Paralelogram." },
{ "en": "Bahasa Baku dari 'Part-time'?", "id": "Paruh waktu." },
{ "en": "Bahasa Baku dari 'Password'?", "id": "Kata sandi." },
{ "en": "Bahasa Baku dari 'Pastur'?", "id": "Pastor." },
{ "en": "Bahasa Baku dari 'Patsus'?", "id": "Penempatan khusus." },
{ "en": "Bahasa Baku dari 'Pelesetan'?", "id": "Plesetan." },
{ "en": "Bahasa Baku dari 'Pensi'?", "id": "Pentas seni." },
{ "en": "Bahasa Baku dari 'Peot'?", "id": "Peyot." },
{ "en": "Bahasa Baku dari 'Per'?", "id": "Pegas." },
{ "en": "Bahasa Baku dari 'Performa'?", "id": "Performa." },
{ "en": "Bahasa Baku dari 'Petinggi'?", "id": "Petinggi." },
{ "en": "Bahasa Baku dari 'Petis'?", "id": "Petis." },
{ "en": "Bahasa Baku dari 'Pijit'?", "id": "Pijat." },
{ "en": "Bahasa Baku dari 'Piknik'?", "id": "Piknik." },
{ "en": "Bahasa Baku dari 'Pilkada'?", "id": "Pemilihan kepala daerah." },
{ "en": "Bahasa Baku dari 'Pingpong'?", "id": "Tenis meja." },
{ "en": "Bahasa Baku dari 'Piranha'?", "id": "Piranha." },
{ "en": "Bahasa Baku dari 'Pitting'?", "id": "Fiting." },
{ "en": "Bahasa Baku dari 'Plin-plan'?", "id": "Plinplan." },
{ "en": "Bahasa Baku dari 'Polusi'?", "id": "Polusi." },
{ "en": "Bahasa Baku dari 'Polwan'?", "id": "Polisi wanita." },
{ "en": "Bahasa Baku dari 'Ponco'?", "id": "Ponco." },
{ "en": "Bahasa Baku dari 'Porno'?", "id": "Porno." },
{ "en": "Bahasa Baku dari 'Poros'?", "id": "Poros." },
{ "en": "Bahasa Baku dari 'Pos'?", "id": "Pos." },
{ "en": "Bahasa Baku dari 'Post-mortem'?", "id": "Pascameninggal." },
{ "en": "Bahasa Baku dari 'Potlot'?", "id": "Pensil." },
{ "en": "Bahasa Baku dari 'Prajurit'?", "id": "Prajurit." },
{ "en": "Bahasa Baku dari 'Prakata'?", "id": "Prakata." },
{ "en": "Bahasa Baku dari 'Pramubakti'?", "id": "Pramubakti." },
{ "en": "Bahasa Baku dari 'Prasangka'?", "id": "Prasangka." },
{ "en": "Bahasa Baku dari 'Prasasti'?", "id": "Prasasti." },
{ "en": "Bahasa Baku dari 'Prasmanan'?", "id": "Prasmanan." },
{ "en": "Bahasa Baku dari 'Premium'?", "id": "Premium." },
{ "en": "Bahasa Baku dari 'Premix'?", "id": "Premix." },
{ "en": "Bahasa Baku dari 'Pribumi'?", "id": "Pribumi." },
{ "en": "Bahasa Baku dari 'Pukul'?", "id": "Pukul." },
{ "en": "Bahasa Baku dari 'Pungli'?", "id": "Pungutan liar." },
{ "en": "Bahasa Baku dari 'Pupuk'?", "id": "Pupuk." },
{ "en": "Bahasa Baku dari 'Puspa'?", "id": "Puspa." },
{ "en": "Bahasa Baku dari 'Pus-pus'?", "id": "Pus." },
{ "en": "Bahasa Baku dari 'Putu'?", "id": "Putu." },
{ "en": "Bahasa Baku dari 'Puzzle'?", "id": "Teka-teki." },
{ "en": "Bahasa Baku dari 'Radio aktif'?", "id": "Radioaktif." },
{ "en": "Bahasa Baku dari 'Ragi'?", "id": "Ragi." },
{ "en": "Bahasa Baku dari 'Raka'?", "id": "Raka." },
{ "en": "Bahasa Baku dari 'Rakor'?", "id": "Rapat koordinasi." },
{ "en": "Bahasa Baku dari 'Ransel'?", "id": "Ransel." },
{ "en": "Bahasa Baku dari 'Rapel'?", "id": "Rapel." },
{ "en": "Bahasa Baku dari 'Rating'?", "id": "Peringkat." },
{ "en": "Bahasa Baku dari 'Reboisasi'?", "id": "Reboisasi." },
{ "en": "Bahasa Baku dari 'Regal'?", "id": "Regal." },
{ "en": "Bahasa Baku dari 'Reinkarnasi'?", "id": "Reinkarnasi." },
{ "en": "Bahasa Baku dari 'Rekening'?", "id": "Rekening." },
{ "en": "Bahasa Baku dari 'Rembes'?", "id": "Rembes." },
{ "en": "Bahasa Baku dari 'Remidi'?", "id": "Remedi." },
{ "en": "Bahasa Baku dari 'Renaisans'?", "id": "Renaisans." },
{ "en": "Bahasa Baku dari 'Renyah'?", "id": "Renyah." },
{ "en": "Bahasa Baku dari 'Residivis'?", "id": "Residivis." },
{ "en": "Bahasa Baku dari 'Resign'?", "id": "Mengundurkan diri." },
{ "en": "Bahasa Baku dari 'Rest area'?", "id": "Area istirahat." },
{ "en": "Bahasa Baku dari 'Revolusi'?", "id": "Revolusi." },
{ "en": "Bahasa Baku dari 'Rinso'?", "id": "Detergen." },
{ "en": "Bahasa Baku dari 'Rowayat'?", "id": "Riwayat." },
{ "en": "Bahasa Baku dari 'Ruko'?", "id": "Rumah toko." },
{ "en": "Bahasa Baku dari 'Runia'?", "id": "Dunia." },
{ "en": "Bahasa Baku dari 'Rurun'?", "id": "Turun." },
{ "en": "Bahasa Baku dari 'Saptu'?", "id": "Sabtu." },
{ "en": "Bahasa Baku dari 'Sahid'?", "id": "Syahid." },
{ "en": "Bahasa Baku dari 'Sair'?", "id": "Syair." },
{ "en": "Bahasa Baku dari 'Samber'?", "id": "Sambar." },
{ "en": "Bahasa Baku dari 'Sanggama'?", "id": "Senggama." },
{ "en": "Bahasa Baku dari 'Sardencis'?", "id": "Sarden." },
{ "en": "Bahasa Baku dari 'Sato'?", "id": "Hewan." },
{ "en": "Bahasa Baku dari 'Satpam'?", "id": "Satuan pengamanan." },
{ "en": "Bahasa Baku dari 'Sawang'?", "id": "Jaring laba-laba." },
{ "en": "Bahasa Baku dari 'Sawer'?", "id": "Menyawer." },
{ "en": "Bahasa Baku dari 'Scanner'?", "id": "Pemindai." },
{ "en": "Bahasa Baku dari 'Screen shot'?", "id": "Tangkapan layar." },
{ "en": "Bahasa Baku dari 'Seismograp'?", "id": "Seismograf." },
{ "en": "Bahasa Baku dari 'Sekip'?", "id": "Melewatkan." },
{ "en": "Bahasa Baku dari 'Sekretariatan'?", "id": "Sekretariat." },
{ "en": "Bahasa Baku dari 'Seksofon'?", "id": "Saksofon." },
{ "en": "Bahasa Baku dari 'Selebor'?", "id": "Ceroboh." },
{ "en": "Bahasa Baku dari 'Selebriti'?", "id": "Selebritas." },
{ "en": "Bahasa Baku dari 'Selot'?", "id": "Grendel." },
{ "en": "Bahasa Baku dari 'Semedi'?", "id": "Bertapa." },
{ "en": "Bahasa Baku dari 'Semi final'?", "id": "Semifinal." },
{ "en": "Bahasa Baku dari 'Sentausa'?", "id": "Sentosa." },
{ "en": "Bahasa Baku dari 'Sepakbor'?", "id": "Spakbor." },
{ "en": "Bahasa Baku dari 'Sepeling'?", "id": "Ejaan." },
{ "en": "Bahasa Baku dari 'Sepion'?", "id": "Kaca spion." },
{ "en": "Bahasa Baku dari 'Serban'?", "id": "Sorban." },
{ "en": "Bahasa Baku dari 'Serep'?", "id": "Cadangan." },
{ "en": "Bahasa Baku dari 'Server'?", "id": "Peladen." },
{ "en": "Bahasa Baku dari 'Setagen'?", "id": "Stagen." },
{ "en": "Bahasa Baku dari 'Setang'?", "id": "Stang." },
{ "en": "Bahasa Baku dari 'Setip'?", "id": "Penghapus." },
{ "en": "Bahasa Baku dari 'Setting'?", "id": "Pengaturan." },
{ "en": "Bahasa Baku dari 'Shogun'?", "id": "Syogun." },
{ "en": "Bahasa Baku dari 'Shooting'?", "id": "Syuting." },
{ "en": "Bahasa Baku dari 'Show'?", "id": "Pertunjukan." },
{ "en": "Bahasa Baku dari 'Showroom'?", "id": "Ruang pamer." },
{ "en": "Bahasa Baku dari 'Sifilis'?", "id": "Sipilis." },
{ "en": "Bahasa Baku dari 'Sikon'?", "id": "Situasi dan kondisi." },
{ "en": "Bahasa Baku dari 'Silaturohim'?", "id": "Silaturahmi." },
{ "en": "Bahasa Baku dari 'Simpang-siur'?", "id": "Simpang siur." },
{ "en": "Bahasa Baku dari 'Singlet'?", "id": "Kaus dalam." },
{ "en": "Bahasa Baku dari 'Sinkronisir'?", "id": "Sinkronisasi." },
{ "en": "Bahasa Baku dari 'Sinterklas'?", "id": "Sinterklas." },
{ "en": "Bahasa Baku dari 'Sinyalir'?", "id": "Sinyalemen." },
{ "en": "Bahasa Baku dari 'Siphoning'?", "id": "Penyifonan." },
{ "en": "Bahasa Baku dari 'Sirik'?", "id": "Syirik." },
{ "en": "Bahasa Baku dari 'Sit-up'?", "id": "Baring duduk." },
{ "en": "Bahasa Baku dari 'Ski'?", "id": "Ski." },
{ "en": "Bahasa Baku dari 'Slaf'?", "id": "Selaf." },
{ "en": "Bahasa Baku dari 'Slang'?", "id": "Slang." },
{ "en": "Bahasa Baku dari 'Slide'?", "id": "Salindia." },
{ "en": "Bahasa Baku dari 'Sliweran'?", "id": "Lalu-lalang." },
{ "en": "Bahasa Baku dari 'Smartphone'?", "id": "Ponsel pintar." },
{ "en": "Bahasa Baku dari 'Snack'?", "id": "Kudapan." },
{ "en": "Bahasa Baku dari 'Softball'?", "id": "Sofbol." },
{ "en": "Bahasa Baku dari 'Software'?", "id": "Perangkat lunak." },
{ "en": "Bahasa Baku dari 'Sok'?", "id": "Sok." },
{ "en": "Bahasa Baku dari 'Solder'?", "id": "Solder." },
{ "en": "Bahasa Baku dari 'Soleh'?", "id": "Saleh." },
{ "en": "Bahasa Baku dari 'Somay'?", "id": "Siomai." },
{ "en": "Bahasa Baku dari 'Sontek'?", "id": "Contek." },
{ "en": "Bahasa Baku dari 'Sound system'?", "id": "Tata suara." },
{ "en": "Bahasa Baku dari 'Soun'?", "id": "Soun." },
{ "en": "Bahasa Baku dari 'So'un'?", "id": "Soun." },
{ "en": "Bahasa Baku dari 'Spageti'?", "id": "Spageti." },
{ "en": "Bahasa Baku dari 'Spakbor'?", "id": "Sepakbor." },
{ "en": "Bahasa Baku dari 'Spare part'?", "id": "Suku cadang." },
{ "en": "Bahasa Baku dari 'Speedboat'?", "id": "Kapal cepat." },
{ "en": "Bahasa Baku dari 'Spesial'?", "id": "Istimewa." },
{ "en": "Bahasa Baku dari 'Spidol'?", "id": "Spidol." },
{ "en": "Bahasa Baku dari 'Spionase'?", "id": "Spionase." },
{ "en": "Bahasa Baku dari 'Spiritus'?", "id": "Spiritus." },
{ "en": "Bahasa Baku dari 'Spon'?", "id": "Spons." },
{ "en": "Bahasa Baku dari 'Sponsor'?", "id": "Sponsor." },
{ "en": "Bahasa Baku dari 'Sprei'?", "id": "Seprai." },
{ "en": "Bahasa Baku dari 'Squash'?", "id": "Skuas." },
{ "en": "Bahasa Baku dari 'SS'?", "id": "Surat suara." },
{ "en": "Bahasa Baku dari 'Stabilisator'?", "id": "Penyetabil." },
{ "en": "Bahasa Baku dari 'Stadion'?", "id": "Stadion." },
{ "en": "Bahasa Baku dari 'Stagnasi'?", "id": "Stagnasi." },
{ "en": "Bahasa Baku dari 'Stambuk'?", "id": "Buku induk." },
{ "en": "Bahasa Baku dari 'Stapler'?", "id": "Stepler." },
{ "en": "Bahasa Baku dari 'Stater'?", "id": "Starter." },
{ "en": "Bahasa Baku dari 'Stasiun'?", "id": "Stasiun." },
{ "en": "Bahasa Baku dari 'Stelsel'?", "id": "Stelsel." },
{ "en": "Bahasa Baku dari 'Stewardes'?", "id": "Pramugari." },
{ "en": "Bahasa Baku dari 'Stir'?", "id": "Setir." },
{ "en": "Bahasa Baku dari 'Stok'?", "id": "Stok." },
{ "en": "Bahasa Baku dari 'Stres'?", "id": "Stres." },
{ "en": "Bahasa Baku dari 'Strip'?", "id": "Setrip." },
{ "en": "Bahasa Baku dari 'Strok'?", "id": "Stroke." },
{ "en": "Bahasa Baku dari 'Struktural'?", "id": "Struktural." },
{ "en": "Bahasa Baku dari 'Studio'?", "id": "Studio." },
{ "en": "Bahasa Baku dari 'Sub poena'?", "id": "Subpoena." },
{ "en": "Bahasa Baku dari 'Sub Bab'?", "id": "Subbab." },
{ "en": "Bahasa Baku dari 'Sub kontraktor'?", "id": "Subkontraktor." },
{ "en": "Bahasa Baku dari 'Sub seksi'?", "id": "Subseksi." },
{ "en": "Bahasa Baku dari 'Subtropik'?", "id": "Subtropis." },
{ "en": "Bahasa Baku dari 'Suka cita'?", "id": "Sukacita." },
{ "en": "Bahasa Baku dari 'Super-man'?", "id": "Superman." },
{ "en": "Bahasa Baku dari 'Super star'?", "id": "Adibintang." },
{ "en": "Bahasa Baku dari 'Suplier'?", "id": "Pemasok." },
{ "en": "Bahasa Baku dari 'Support'?", "id": "Dukungan." },
{ "en": "Bahasa Baku dari 'Suryo'?", "id": "Matahari." },
{ "en": "Bahasa Baku dari 'Suspect'?", "id": "Tersangka." },
{ "en": "Bahasa Baku dari 'Swa daya'?", "id": "Swadaya." },
{ "en": "Bahasa Baku dari 'Sweater'?", "id": "Sweter." },
{ "en": "Bahasa Baku dari 'Syah'?", "id": "Sah." },
{ "en": "Bahasa Baku dari 'Syaraf'?", "id": "Saraf." },
{ "en": "Bahasa Baku dari 'Syeikh'?", "id": "Syekh." },
{ "en": "Bahasa Baku dari 'Syopir'?", "id": "Sopir." },
{ "en": "Bahasa Baku dari 'Syurga'?", "id": "Surga." },
{ "en": "Bahasa Baku dari 'Ta'jil'?", "id": "Takjil." },
{ "en": "Bahasa Baku dari 'Ta'lim'?", "id": "Taklim." },
{ "en": "Bahasa Baku dari 'Ta'ziyah'?", "id": "Takziah." },
{ "en": "Bahasa Baku dari 'Tabanas'?", "id": "Tabungan Pembangunan Nasional." },
{ "en": "Bahasa Baku dari 'Tahajjud'?", "id": "Tahajud." },
{ "en": "Bahasa Baku dari 'Taho'?", "id": "Tahu (makanan)." },
{ "en": "Bahasa Baku dari 'Takjup'?", "id": "Takjub." },
{ "en": "Bahasa Baku dari 'Talk'?", "id": "Talek." },
{ "en": "Bahasa Baku dari 'Tamar'?", "id": "Kurma." },
{ "en": "Bahasa Baku dari 'Tancep'?", "id": "Tancap." },
{ "en": "Bahasa Baku dari 'Tanggungjawab'?", "id": "Tanggung jawab." },
{ "en": "Bahasa Baku dari 'Tarif'?", "id": "Tarif." },
{ "en": "Bahasa Baku dari 'Tatoase'?", "id": "Tato." },
{ "en": "Bahasa Baku dari 'Tawakal'?", "id": "Tawakal." },
{ "en": "Bahasa Baku dari 'TBC'?", "id": "Tuberkulosis." },
{ "en": "Bahasa Baku dari 'Tee'?", "id": "Te." },
{ "en": "Bahasa Baku dari 'Teka teki silang'?", "id": "Teka-teki silang." },
{ "en": "Bahasa Baku dari 'Teken'?", "id": "Tanda tangan." },
{ "en": "Bahasa Baku dari 'Tekhnik'?", "id": "Teknik." },
{ "en": "Bahasa Baku dari 'Tekhno'?", "id": "Tekno." },
{ "en": "Bahasa Baku dari 'Teknokrat'?", "id": "Teknokrat." },
{ "en": "Bahasa Baku dari 'Tekstil'?", "id": "Tekstil." },
{ "en": "Bahasa Baku dari 'Telfon'?", "id": "Telepon." },
{ "en": "Bahasa Baku dari 'Teler'?", "id": "Mabuk." },
{ "en": "Bahasa Baku dari 'Teleskop'?", "id": "Teleskop." },
{ "en": "Bahasa Baku dari 'Telur'?", "id": "Telur." },
{ "en": "Bahasa Baku dari 'Tema'?", "id": "Tema." },
{ "en": "Bahasa Baku dari 'Tempel'?", "id": "Tempel." },
{ "en": "Bahasa Baku dari 'Tempo'?", "id": "Tempo." },
{ "en": "Bahasa Baku dari 'Tempo dulu'?", "id": "Tempo dulu." },
{ "en": "Bahasa Baku dari 'Tendang'?", "id": "Tendang." },
{ "en": "Bahasa Baku dari 'Tenggak'?", "id": "Tenggak." },
{ "en": "Bahasa Baku dari 'Tengil'?", "id": "Tengil." },
{ "en": "Bahasa Baku dari 'Tengkorak'?", "id": "Tengkorak." },
{ "en": "Bahasa Baku dari 'Tensi'?", "id": "Tensi." },
{ "en": "Bahasa Baku dari 'Tentrem'?", "id": "Tenteram." },
{ "en": "Bahasa Baku dari 'Teokrasi'?", "id": "Teokrasi." },
{ "en": "Bahasa Baku dari 'Tepok'?", "id": "Tepuk." },
{ "en": "Bahasa Baku dari 'Terawangan'?", "id": "Terawang." },
{ "en": "Bahasa Baku dari 'Tercatat'?", "id": "Tercatat." },
{ "en": "Bahasa Baku dari 'Terigu'?", "id": "Terigu." },
{ "en": "Bahasa Baku dari 'Terlepas'?", "id": "Terlepas." },
{ "en": "Bahasa Baku dari 'Terpedaya'?", "id": "Terperdaya." },
{ "en": "Bahasa Baku dari 'Terpercaya'?", "id": "Tepercaya." },
{ "en": "Bahasa Baku dari 'Tersetrum'?", "id": "Kesetrum." },
{ "en": "Bahasa Baku dari 'Tesis'?", "id": "Tesis." },
{ "en": "Bahasa Baku dari 'Testimoni'?", "id": "Testimonium." },
{ "en": "Bahasa Baku dari 'Tetra'?", "id": "Tetra." },
{ "en": "Bahasa Baku dari 'Tho'?", "id": "Toh." },
{ "en": "Bahasa Baku dari 'Timbel'?", "id": "Timbal." },
{ "en": "Bahasa Baku dari 'Timur'?", "id": "Timur." },
{ "en": "Bahasa Baku dari 'Tinja'?", "id": "Tinja." },
{ "en": "Bahasa Baku dari 'Tip'?", "id": "Tip." },
{ "en": "Bahasa Baku dari 'Tirai'?", "id": "Tirai." },
{ "en": "Bahasa Baku dari 'Tiran'?", "id": "Tiran." },
{ "en": "Bahasa Baku dari 'Titel'?", "id": "Titel." },
{ "en": "Bahasa Baku dari 'Titik'?", "id": "Titik." },
{ "en": "Bahasa Baku dari 'Toga'?", "id": "Toga." },
{ "en": "Bahasa Baku dari 'Tokek'?", "id": "Tokek." },
{ "en": "Bahasa Baku dari 'Tokoh'?", "id": "Tokoh." },
{ "en": "Bahasa Baku dari 'Tol'?", "id": "Jalan tol." },
{ "en": "Bahasa Baku dari 'Tombak'?", "id": "Tombak." },
{ "en": "Bahasa Baku dari 'Tomboi'?", "id": "Tomboi." },
{ "en": "Bahasa Baku dari 'Topan'?", "id": "Taufan." },
{ "en": "Bahasa Baku dari 'Topeng'?", "id": "Topeng." },
{ "en": "Bahasa Baku dari 'Topik'?", "id": "Topik." },
{ "en": "Bahasa Baku dari 'Torso'?", "id": "Torso." },
{ "en": "Bahasa Baku dari 'Tradisi'?", "id": "Tradisi." },
{ "en": "Bahasa Baku dari 'Trafik'?", "id": "Lalu lintas." },
{ "en": "Bahasa Baku dari 'Tragis'?", "id": "Tragis." },
{ "en": "Bahasa Baku dari 'Trampolin'?", "id": "Trampolin." },
{ "en": "Bahasa Baku dari 'Trans'?", "id": "Trans." },
{ "en": "Bahasa Baku dari 'Transfer'?", "id": "Transfer." },
{ "en": "Bahasa Baku dari 'Transkrip'?", "id": "Transkrip." },
{ "en": "Bahasa Baku dari 'Transmisi'?", "id": "Transmisi." },
{ "en": "Bahasa Baku dari 'Travel'?", "id": "Agen perjalanan." },
{ "en": "Bahasa Baku dari 'Trekking'?", "id": "Lintas alam." },
{ "en": "Bahasa Baku dari 'Tri wulan'?", "id": "Triwulan." },
{ "en": "Bahasa Baku dari 'Trifoid'?", "id": "Tifoid." },
{ "en": "Bahasa Baku dari 'Tropi'?", "id": "Trofi." },
{ "en": "Bahasa Baku dari 'Trus'?", "id": "Terus." },
{ "en": "Bahasa Baku dari 'Tugur'?", "id": "Tugu." },
{ "en": "Bahasa Baku dari 'Tuxedo'?", "id": "Tuksedo." },
{ "en": "Bahasa Baku dari 'Tulalit'?", "id": "Telat berpikir." },
{ "en": "Bahasa Baku dari 'Turban'?", "id": "Serban." },
{ "en": "Bahasa Baku dari 'Tuna grahita'?", "id": "Tunagrahita." },
{ "en": "Bahasa Baku dari 'Tuna rungu'?", "id": "Tunarungu." },
{ "en": "Bahasa Baku dari 'Turnament'?", "id": "Turnamen." },
{ "en": "Bahasa Baku dari 'Turnoi'?", "id": "Turnamen." },
{ "en": "Bahasa Baku dari 'Udel'?", "id": "Pusar." },
{ "en": "Bahasa Baku dari 'Uleg'?", "id": "Ulek." },
{ "en": "Bahasa Baku dari 'Ulem'?", "id": "Undangan." },
{ "en": "Bahasa Baku dari 'Ultra violet'?", "id": "Ultraviolet." },
{ "en": "Bahasa Baku dari 'Up to date'?", "id": "Mutakhir." },
{ "en": "Bahasa Baku dari 'Upgrade'?", "id": "Mutakhirkan." },
{ "en": "Bahasa Baku dari 'Upload'?", "id": "Unggah." },
{ "en": "Bahasa Baku dari 'Valentin'?", "id": "Valentine." },
{ "en": "Bahasa Baku dari 'Vampire'?", "id": "Vampir." },
{ "en": "Bahasa Baku dari 'Panili'?", "id": "Vanili." },
{ "en": "Bahasa Baku dari 'Vaselin'?", "id": "Vaselin." },
{ "en": "Bahasa Baku dari 'Ventilasi'?", "id": "Ventilasi." },
{ "en": "Bahasa Baku dari 'Vermaak'?", "id": "Permak." },
{ "en": "Bahasa Baku dari 'Video call'?", "id": "Panggilan video." },
{ "en": "Bahasa Baku dari 'Viewer'?", "id": "Penonton." },
{ "en": "Bahasa Baku dari 'Vokal'?", "id": "Vokal." },
{ "en": "Bahasa Baku dari 'Voucher'?", "id": "Voucer." },
{ "en": "Bahasa Baku dari 'Wajah'?", "id": "Wajah." },
{ "en": "Bahasa Baku dari 'Wajar'?", "id": "Wajar." },
{ "en": "Bahasa Baku dari 'Wajib'?", "id": "Wajib." },
{ "en": "Bahasa Baku dari 'Wakaf'?", "id": "Wakaf." },
{ "en": "Bahasa Baku dari 'Wakil'?", "id": "Wakil." },
{ "en": "Bahasa Baku dari 'Waktu'?", "id": "Waktu." },
{ "en": "Bahasa Baku dari 'Walet'?", "id": "Walet." },
{ "en": "Bahasa Baku dari 'Wallpaper'?", "id": "Kertas dinding." },
{ "en": "Bahasa Baku dari 'Wanita'?", "id": "Wanita." },
{ "en": "Bahasa Baku dari 'Warna'?", "id": "Warna." },
{ "en": "Bahasa Baku dari 'Warung'?", "id": "Warung." },
{ "en": "Bahasa Baku dari 'Wasiat'?", "id": "Wasiat." },
{ "en": "Bahasa Baku dari 'Welas asih'?", "id": "Welas asih." },
{ "en": "Bahasa Baku dari 'Welder'?", "id": "Juru las." },
{ "en": "Bahasa Baku dari 'Wenak'?", "id": "Enak." },
{ "en": "Bahasa Baku dari 'Wihara'?", "id": "Vihara." },
{ "en": "Bahasa Baku dari 'Wilayah'?", "id": "Wilayah." },
{ "en": "Bahasa Baku dari 'Wirausaha'?", "id": "Wirausaha." },
{ "en": "Bahasa Baku dari 'Wisma'?", "id": "Wisma." },
{ "en": "Bahasa Baku dari 'Wudhu'?", "id": "Wudu." },
{ "en": "Bahasa Baku dari 'Yaa'?", "id": "Ya." },
{ "en": "Bahasa Baku dari 'Yahudi'?", "id": "Yahudi." },
{ "en": "Bahasa Baku dari 'Yakin'?", "id": "Yakin." },
{ "en": "Bahasa Baku dari 'Yatim'?", "id": "Yatim." },
{ "en": "Bahasa Baku dari 'Yg'?", "id": "Yang." },
{ "en": "Bahasa Baku dari 'Yudisium'?", "id": "Yudisium." },
{ "en": "Bahasa Baku dari 'Yunior'?", "id": "Junior." },
{ "en": "Bahasa Baku dari 'Yurisdiksi'?", "id": "Yurisdiksi." },
{ "en": "Bahasa Baku dari 'Zakar'?", "id": "Zakar." },
{ "en": "Bahasa Baku dari 'Zaman'?", "id": "Zaman." },
{ "en": "Bahasa Baku dari 'Zarah'?", "id": "Zarah." },
{ "en": "Bahasa Baku dari 'Zebra'?", "id": "Zebra." },
{ "en": "Bahasa Baku dari 'Zenit'?", "id": "Zenit." },
{ "en": "Bahasa Baku dari 'Zigot'?", "id": "Zigot." },
{ "en": "Bahasa Baku dari 'Zina'?", "id": "Zina." },
{ "en": "Bahasa Baku dari 'Zion'?", "id": "Zion." },
{ "en": "Bahasa Baku dari 'Zodiak'?", "id": "Zodiak." },
{ "en": "Bahasa Baku dari 'Zombi'?", "id": "Zombi." },
{ "en": "Bahasa Baku dari 'Zona'?", "id": "Zona." },
{ "en": "Bahasa Baku dari 'Zoologi'?", "id": "Zoologi." },
{ "en": "Bahasa Baku dari 'Zoom'?", "id": "Zum." },
{ "en": "Bahasa Baku dari 'Zuhur'?", "id": "Zuhur." },
{ "en": "Bahasa Baku dari 'Adjektif'?", "id": "Adjektiva." },
{ "en": "Bahasa Baku dari 'Aki'?", "id": "Baterai." },
{ "en": "Bahasa Baku dari 'Akulturasi'?", "id": "Akulturasi." },
{ "en": "Bahasa Baku dari 'Akunting'?", "id": "Akuntansi." },
{ "en": "Bahasa Baku dari 'Aluminimum'?", "id": "Aluminium." },
{ "en": "Bahasa Baku dari 'Analist'?", "id": "Analis." },
{ "en": "Bahasa Baku dari 'Anarki'?", "id": "Anarki." },
{ "en": "Bahasa Baku dari 'Baper'?", "id": "Bawa perasaan." },
{ "en": "Bahasa Baku dari 'Bengep'?", "id": "Bengkak." },
{ "en": "Bahasa Baku dari 'Blow-up'?", "id": "Ledakkan." },
{ "en": "Bahasa Baku dari 'Bokap'?", "id": "Ayah." },
{ "en": "Bahasa Baku dari 'Bonyok'?", "id": "Babak belur." },
{ "en": "Bahasa Baku dari 'Borgol'?", "id": "Borgol." },
{ "en": "Bahasa Baku dari 'Bucin'?", "id": "Budak cinta." },
{ "en": "Bahasa Baku dari 'Budeg'?", "id": "Tuli." },
{ "en": "Bahasa Baku dari 'Buldozer'?", "id": "Buldozer." },
{ "en": "Bahasa Baku dari 'Cablak'?", "id": "Terus terang." },
{ "en": "Bahasa Baku dari 'Capcus'?", "id": "Cepat." },
{ "en": "Bahasa Baku dari 'Cakep'?", "id": "Tampan." },
{ "en": "Bahasa Baku dari 'Capcai'?", "id": "Kapcai." },
{ "en": "Bahasa Baku dari 'Caper'?", "id": "Cari perhatian." },
{ "en": "Bahasa Baku dari 'Casing'?", "id": "Selubung." },
{ "en": "Bahasa Baku dari 'Cas'?", "id": "Isi daya." },
{ "en": "Bahasa Baku dari 'Cegil'?", "id": "Cewek gila." },
{ "en": "Bahasa Baku dari 'Cekidot'?", "id": "Silakan lihat." },
{ "en": "Bahasa Baku dari 'Celebgram'?", "id": "Selebritas Instagram." },
{ "en": "Bahasa Baku dari 'Celentang'?", "id": "Telentang." },
{ "en": "Bahasa Baku dari 'Celingak-celinguk'?", "id": "Menoleh ke sana kemari." },
{ "en": "Bahasa Baku dari 'Cemen'?", "id": "Penakut." },
{ "en": "Bahasa Baku dari 'Cengli'?", "id": "Adil." },
{ "en": "Bahasa Baku dari 'Center'?", "id": "Pusat." },
{ "en": "Bahasa Baku dari 'Ceplas-ceplos'?", "id": "Berbicara blak-blakan." },
{ "en": "Bahasa Baku dari 'Cewek'?", "id": "Perempuan." },
{ "en": "Bahasa Baku dari 'Charger'?", "id": "Pengisi daya." },
{ "en": "Bahasa Baku dari 'Chat'?", "id": "Obrolan." },
{ "en": "Bahasa Baku dari 'Chatting'?", "id": "Mengobrol daring." },
{ "en": "Bahasa Baku dari 'Check-in'?", "id": "Lapor masuk." },
{ "en": "Bahasa Baku dari 'Check-out'?", "id": "Lapor keluar." },
{ "en": "Bahasa Baku dari 'Cinlok'?", "id": "Cinta lokasi." },
{ "en": "Bahasa Baku dari 'Clear'?", "id": "Jelas." },
{ "en": "Bahasa Baku dari 'Cogan'?", "id": "Cowok ganteng." },
{ "en": "Bahasa Baku dari 'Colong'?", "id": "Curi." },
{ "en": "Bahasa Baku dari 'Comeback'?", "id": "Kembali lagi." },
{ "en": "Bahasa Baku dari 'Congrat'?", "id": "Selamat." },
{ "en": "Bahasa Baku dari 'Contact person'?", "id": "Narahubung." },
{ "en": "Bahasa Baku dari 'Cuan'?", "id": "Untung." },
{ "en": "Bahasa Baku dari 'Cupu'?", "id": "Kikuk." },
{ "en": "Bahasa Baku dari 'Curcol'?", "id": "Curhat colongan." },
{ "en": "Bahasa Baku dari 'Dedemit'?", "id": "Roh halus." },
{ "en": "Bahasa Baku dari 'Default'?", "id": "Bawaan." },
{ "en": "Bahasa Baku dari 'Deking'?", "id": "Pelindung." },
{ "en": "Bahasa Baku dari 'Delete'?", "id": "Hapus." },
{ "en": "Bahasa Baku dari 'Delivery'?", "id": "Pengantaran." },
{ "en": "Bahasa Baku dari 'Demen'?", "id": "Suka." },
{ "en": "Bahasa Baku dari 'Demo'?", "id": "Demonstrasi." },
{ "en": "Bahasa Baku dari 'Dengkul'?", "id": "Lutut." },
{ "en": "Bahasa Baku dari 'Deo'?", "id": "Deodoran." },
{ "en": "Bahasa Baku dari 'Display'?", "id": "Tampilan." },
{ "en": "Bahasa Baku dari 'Distro'?", "id": "Toko distribusi." },
{ "en": "Bahasa Baku dari 'Doi'?", "id": "Dia." },
{ "en": "Bahasa Baku dari 'Dongo'?", "id": "Bodoh." },
{ "en": "Bahasa Baku dari 'Doyan'?", "id": "Gemar." },
{ "en": "Bahasa Baku dari 'DP'?", "id": "Uang muka." },
{ "en": "Bahasa Baku dari 'Dugem'?", "id": "Dunia gemerlap." },
{ "en": "Bahasa Baku dari 'EGP'?", "id": "Emang gue pikirin." },
{ "en": "Bahasa Baku dari 'Error'?", "id": "Galat." },
{ "en": "Bahasa Baku dari 'Asal tahu sama tahu'?", "id": "Sama-sama tahu." },
{ "en": "Bahasa Baku dari 'Foyer'?", "id": "Serambi." },
{ "en": "Bahasa Baku dari 'Gabut'?", "id": "Gaji buta." },
{ "en": "Bahasa Baku dari 'Galau'?", "id": "Risau." },
{ "en": "Bahasa Baku dari 'Gans'?", "id": "Ganteng." },
{ "en": "Bahasa Baku dari 'Garing'?", "id": "Tidak lucu." },
{ "en": "Bahasa Baku dari 'Gaje'?", "id": "Tidak jelas." },
{ "en": "Bahasa Baku dari 'Gaptek'?", "id": "Gagap teknologi." },
{ "en": "Bahasa Baku dari 'Gercep'?", "id": "Gerak cepat." },
{ "en": "Bahasa Baku dari 'Gimmick'?", "id": "Gimik." },
{ "en": "Bahasa Baku dari 'Gokil'?", "id": "Gila." },
{ "en": "Bahasa Baku dari 'GWS'?", "id": "Semoga lekas sembuh." },
{ "en": "Bahasa Baku dari 'Halu'?", "id": "Halusinasi." },
{ "en": "Bahasa Baku dari 'Headset'?", "id": "Peranti jemala." },
{ "en": "Bahasa Baku dari 'Hoarding'?", "id": "Penimbunan." },
{ "en": "Bahasa Baku dari 'Japri'?", "id": "Jalur pribadi." },
{ "en": "Bahasa Baku dari 'Jayus'?", "id": "Tidak lucu." },
{ "en": "Bahasa Baku dari 'Kamsia'?", "id": "Terima kasih." },
{ "en": "Bahasa Baku dari 'Kating'?", "id": "Kakak tingkat." },
{ "en": "Bahasa Baku dari 'Kaum rebahan'?", "id": "Orang malas." },
{ "en": "Bahasa Baku dari 'Kecot'?", "id": "Banyak bicara." },
{ "en": "Bahasa Baku dari 'Keleus'?", "id": "Kali." },
{ "en": "Bahasa Baku dari 'Keyboard'?", "id": "Papan ketik." },
{ "en": "Bahasa Baku dari 'Kicep'?", "id": "Diam." },
{ "en": "Bahasa Baku dari 'Kondangan'?", "id": "Menghadiri undangan." },
{ "en": "Bahasa Baku dari 'Kongkow'?", "id": "Berkumpul." },
{ "en": "Bahasa Baku dari 'Korsleting'?", "id": "Hubungan pendek." },
{ "en": "Bahasa Baku dari 'Kudet'?", "id": "Kurang `update`." },
{ "en": "Bahasa Baku dari 'Kuper'?", "id": "Kurang pergaulan." },
{ "en": "Bahasa Baku dari 'LDR'?", "id": "Hubungan jarak jauh." },
{ "en": "Bahasa Baku dari 'Lemot'?", "id": "Lambat." },
{ "en": "Bahasa Baku dari 'Like'?", "id": "Suka." },
{ "en": "Bahasa Baku dari 'Login'?", "id": "Masuk log." },
{ "en": "Bahasa Baku dari 'Logout'?", "id": "Keluar log." },
{ "en": "Bahasa Baku dari 'Mager'?", "id": "Malas gerak." },
{ "en": "Bahasa Baku dari 'Maksi'?", "id": "Makan siang." },
{ "en": "Bahasa Baku dari 'Mantan'?", "id": "Bekas pacar." },
{ "en": "Bahasa Baku dari 'Meme'?", "id": "Meme." },
{ "en": "Bahasa Baku dari 'Mantul'?", "id": "Mantap betul." },
{ "en": "Bahasa Baku dari 'Mouse'?", "id": "Tetikus." },
{ "en": "Bahasa Baku dari 'Narsis'?", "id": "Narsisis." },
{ "en": "Bahasa Baku dari 'Nelfon'?", "id": "Menelepon." },
{ "en": "Bahasa Baku dari 'Netizen'?", "id": "Warganet." },
{ "en": "Bahasa Baku dari 'Ngerubah'?", "id": "Mengubah." },
{ "en": "Bahasa Baku dari 'Ngetop'?", "id": "Terkenal." },
{ "en": "Bahasa Baku dari 'Nyokap'?", "id": "Ibu." },
{ "en": "Bahasa Baku dari 'Nyantai'?", "id": "Bersantai." },
{ "en": "Bahasa Baku dari 'PDKT'?", "id": "Pendekatan." },
{ "en": "Bahasa Baku dari 'PHP'?", "id": "Pemberi harapan palsu." }



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
            }, 7000);
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
