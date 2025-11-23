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


  { "en": "Apa Itu Model OSI Layer 1?", "id": "Physical Layer." },
  { "en": "Perangkat Apa Yang Bekerja Di Layer 1?", "id": "Hub Dan Kabel." },
  { "en": "Apa Itu Model OSI Layer 2?", "id": "Data Link Layer." },
  { "en": "Perangkat Apa Yang Bekerja Di Layer 2?", "id": "Switch Dan Bridge." },
  { "en": "Apa Alamat Identitas Pada Layer 2?", "id": "MAC Address." },
  { "en": "Apa Itu Model OSI Layer 3?", "id": "Network Layer." },
  { "en": "Perangkat Apa Yang Bekerja Di Layer 3?", "id": "Router." },
  { "en": "Apa Alamat Identitas Pada Layer 3?", "id": "IP Address." },
  { "en": "Apa Itu Model OSI Layer 4?", "id": "Transport Layer." },
  { "en": "Protokol Apa Yang Ada Di Layer 4?", "id": "TCP Dan UDP." },
  { "en": "Apa Itu Model OSI Layer 7?", "id": "Application Layer." },
  { "en": "Contoh Protokol Layer 7 Adalah?", "id": "HTTP FTP SMTP." },
  { "en": "Apa Itu Latency Jaringan?", "id": "Waktu Tunda Kirim Data." },
  { "en": "Apa Itu Jitter Jaringan?", "id": "Variasi Nilai Latency." },
  { "en": "Apa Itu Packet Loss?", "id": "Data Hilang Di Perjalanan." },
  { "en": "Apa Itu Throughput Jaringan?", "id": "Kecepatan Transfer Data Nyata." },
  { "en": "Apa Itu DHCP Server?", "id": "Pemberi IP Address Otomatis." },
  { "en": "Apa Itu DHCP Client?", "id": "Penerima IP Address Otomatis." },
  { "en": "Apa Itu IP Static?", "id": "Alamat IP Tetap Manual." },
  { "en": "Apa Itu IP Dynamic?", "id": "Alamat IP Berubah Ubah." },
  { "en": "Apa Itu Suhu Curie Magnet?", "id": "Suhu Magnet Hilang Sifat." },
  { "en": "Berapa Suhu Curie Besi?", "id": "770 Derajat Celcius." },
  { "en": "Apa Itu Efek Peltier?", "id": "Listrik Menjadi Panas Dingin." },
  { "en": "Sisi Dingin Peltier Disebut Sisi?", "id": "Absorpsi Kalor." },
  { "en": "Sisi Panas Peltier Harus Dipasang?", "id": "Heatsink Dan Kipas." },
  { "en": "Apa Itu Generator Termoelektrik (TEG)?", "id": "Panas Menjadi Listrik." },
  { "en": "Apa Itu Superkonduktor Suhu Tinggi?", "id": "Bekerja Di Atas Nitrogen Cair." },
  { "en": "Pendingin Nitrogen Cair Suhunya?", "id": "Minus 196 Derajat Celcius." },
  { "en": "Apa Itu Efek Meissner?", "id": "Levitasi Magnet Di Superkonduktor." },
  { "en": "Apa Itu Crowbar Circuit?", "id": "Proteksi Hubung Singkat Paksa." },
  { "en": "Komponen Utama Crowbar Circuit Adalah?", "id": "SCR Atau Thyristor." },
  { "en": "Kapan Crowbar Circuit Aktif?", "id": "Saat Over Voltage Ekstrem." },
  { "en": "Apa Itu Foldback Current Limiting?", "id": "Arus Turun Saat Short." },
  { "en": "Apa Bedanya Foldback Dan Constant Current?", "id": "Foldback Lebih Aman Transistor." },
  { "en": "Apa Itu Watchdog Timer Hardware?", "id": "IC Reset Eksternal." },
  { "en": "Apa Itu Power On Reset (POR)?", "id": "Sinyal Reset Saat Nyala." },
  { "en": "Apa Itu ESD Mat?", "id": "Alas Kerja Anti Statis." },
  { "en": "Berapa Resistansi ESD Mat Ke Ground?", "id": "1 Mega Ohm." },
  { "en": "Mengapa ESD Mat Tidak 0 Ohm?", "id": "Mencegah Sengatan Arus Besar." },
  { "en": "Apa Itu Ionizer Fan?", "id": "Kipas Penetral Muatan Statis." },
  { "en": "Apa Itu STC Panel Surya?", "id": "Standard Test Conditions." },
  { "en": "Berapa Suhu Pengujian STC?", "id": "25 Derajat Celcius." },
  { "en": "Berapa Irradiance Pengujian STC?", "id": "1000 Watt Per Meter." },
  { "en": "Berapa Air Mass Pengujian STC?", "id": "AM 1.5." },
  { "en": "Apa Itu NOCT Panel Surya?", "id": "Suhu Operasi Sel Normal." },
  { "en": "Kondisi NOCT Lebih Realistis Karena?", "id": "Suhu Kerja 45 Derajat." },
  { "en": "Apa Itu Hot Spot Heating Surya?", "id": "Sel Panas Akibat Bayangan." },
  { "en": "Apa Itu PID Pada Panel Surya?", "id": "Potential Induced Degradation." },
  { "en": "PID Menyebabkan Daya Panel?", "id": "Turun Permanen." },
  { "en": "Apa Itu Busbar Panel Surya?", "id": "Jalur Perak Di Atas Sel." },
  { "en": "Semakin Banyak Busbar Sel Surya?", "id": "Semakin Efisien Arus." },
  { "en": "Apa Itu Panel Surya Half Cut?", "id": "Sel Dipotong Jadi Dua." },
  { "en": "Keunggulan Half Cut Cell Adalah?", "id": "Tahan Efek Bayangan." },
  { "en": "Apa Itu Panel Bifacial?", "id": "Menangkap Cahaya Dua Sisi." },
  { "en": "Sisi Belakang Panel Bifacial Menangkap?", "id": "Pantulan Cahaya Lantai." },
  { "en": "Apa Itu Floating PV?", "id": "PLTS Terapung Di Air." },
  { "en": "Keunggulan Floating PV Adalah?", "id": "Pendinginan Air Alami." },
  { "en": "Apa Itu Solar Tracker?", "id": "Pengikut Gerak Matahari." },
  { "en": "Tracker Single Axis Bergerak?", "id": "Timur Ke Barat." },
  { "en": "Tracker Dual Axis Bergerak?", "id": "Segala Arah Matahari." },
  { "en": "Apa Itu Baterai OPzV?", "id": "Aki Gel Tubular Plate." },
  { "en": "Keunggulan Baterai OPzV Adalah?", "id": "Umur Pakai Sangat Panjang." },
  { "en": "Baterai NiCd Dilarang Di Eropa Karena?", "id": "Limbah Beracun Kadmium." },
  { "en": "Apa Itu Thermal Runaway Baterai?", "id": "Reaksi Panas Tak Terkendali." },
  { "en": "Apa Itu Battery Balancer?", "id": "Penyeimbang Tegangan Seri." },
  { "en": "Apa Itu Peukert Law?", "id": "Kapasitas Turun Saat Arus Besar." },
  { "en": "Hukum Peukert Berlaku Kuat Pada?", "id": "Baterai Lead Acid." },
  { "en": "Baterai Lithium Ion Memiliki Peukert?", "id": "Sangat Kecil Mendekati 1." },
  { "en": "Apa Itu CCA Aki?", "id": "Cold Cranking Amperes." },
  { "en": "Apa Itu CA Atau MCA Aki?", "id": "Marine Cranking Amperes." },
  { "en": "Suhu Ukur CCA Adalah?", "id": "Minus 18 Derajat Celcius." },
  { "en": "Suhu Ukur CA Adalah?", "id": "0 Derajat Celcius." },
  { "en": "Apa Itu RC (Reserve Capacity) Aki?", "id": "Menit Tahan Tanpa Alternator." },
  { "en": "Arus Beban Tes RC Adalah?", "id": "25 Ampere." },
  { "en": "Apa Itu Load Shedding Relay?", "id": "Relay Pelepas Beban Prioritas." },
  { "en": "Apa Itu Grid Code?", "id": "Aturan Sambungan Pembangkit." },
  { "en": "Frekuensi PLN Normal Adalah?", "id": "49,5 Hingga 50,5 Hz." },
  { "en": "Tegangan PLN Normal Adalah?", "id": "Plus Minus 5 Persen." },
  { "en": "Apa Itu Genset Silent Type?", "id": "Genset Dengan Peredam Suara." },
  { "en": "Apa Itu Genset Open Type?", "id": "Genset Rangka Terbuka." },
  { "en": "Tingkat Kebisingan Genset Silent Adalah?", "id": "Sekitar 70 Desibel." },
  { "en": "Apa Itu ATS 4 Pole?", "id": "Memutus Fasa Dan Netral." },
  { "en": "Apa Itu ATS 3 Pole?", "id": "Hanya Memutus Fasa." },
  { "en": "Resiko ATS 3 Pole Adalah?", "id": "Arus Balik Netral." },
  { "en": "Apa Itu Pilot Exciter?", "id": "Pembangkit Magnet Awal." },
  { "en": "Apa Itu Droop Kit Genset?", "id": "Modul Paralel Genset." },
  { "en": "Apa Itu Kabel NYYHY?", "id": "Kabel Serabut Hitam Fleksibel." },
  { "en": "Kabel NYYHY Tahan Terhadap?", "id": "Cuaca Dan Gesekan." },
  { "en": "Kabel NYMHY Cocok Untuk?", "id": "Peralatan Rumah Tangga." },
  { "en": "Apa Itu Pin Header?", "id": "Konektor Kaki Jarum." },
  { "en": "Jarak Pitch Header Standar Adalah?", "id": "2,54 Milimeter." },
  { "en": "Apa Itu JST Connector?", "id": "Konektor Putih Baterai." },
  { "en": "Apa Itu XT60 Connector?", "id": "Konektor Baterai Arus Besar." },
  { "en": "Konektor XT60 Tahan Arus?", "id": "60 Ampere." },
  { "en": "Apa Itu Dean T Plug?", "id": "Konektor Baterai Huruf T." },
  { "en": "Apa Itu Banana Plug?", "id": "Konektor Kabel Multimeter." },
  { "en": "Ukuran Standar Banana Plug?", "id": "4 Milimeter." },
  { "en": "Apa Itu Pantograf Kereta Listrik?", "id": "Alat Pengambil Listrik Atas." },
  { "en": "Apa Bahan Kontak Pantograf?", "id": "Karbon Keras." },
  { "en": "Apa Itu Listrik Aliran Atas (LAA)?", "id": "Kabel Suplai Daya Kereta." },
  { "en": "Apa Itu Kabel Catenary?", "id": "Kabel Penyangga Kawat Kontak." },
  { "en": "Apa Itu Kawat Kontak (Contact Wire)?", "id": "Kabel Yang Bergesekan Dengan Pantograf." },
  { "en": "Bentuk Penampang Kawat Kontak Adalah?", "id": "Berlure Seperti Angka 8." },
  { "en": "Berapa Tegangan KRL Jabodetabek?", "id": "1500 Volt DC." },
  { "en": "Apa Itu Sistem Third Rail?", "id": "Rel Ketiga Pengantar Listrik." },
  { "en": "Di Mana Third Rail Biasa Dipakai?", "id": "Kereta MRT Dan LRT." },
  { "en": "Apa Kelemahan Third Rail?", "id": "Bahaya Tersentuh Manusia." },
  { "en": "Apa Itu Return Current Kereta?", "id": "Arus Balik Melalui Rel." },
  { "en": "Apa Itu Stray Current (Arus Liar)?", "id": "Arus Bocor Ke Tanah." },
  { "en": "Bahaya Stray Current Adalah?", "id": "Korosi Pipa Bawah Tanah." },
  { "en": "Motor Traksi KRL Modern Menggunakan?", "id": "Motor Induksi AC 3 Fasa." },
  { "en": "Apa Itu VVVF Inverter?", "id": "Variable Voltage Variable Frequency." },
  { "en": "VVVF Inverter Mengendalikan Apa?", "id": "Kecepatan Motor Traksi AC." },
  { "en": "Apa Itu Resistor Pengereman (Braking Resistor)?", "id": "Membuang Energi Rem Jadi Panas." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengubah Energi Gerak Jadi Listrik." },
  { "en": "Energi Regeneratif Dikirim Ke Mana?", "id": "Kembali Ke Jaringan Listrik." },
  { "en": "Apa Itu Deadman Switch Kereta?", "id": "Pedal Keselamatan Masinis." },
  { "en": "Apa Itu Buck Converter?", "id": "Penurun Tegangan DC Ke DC." },
  { "en": "Apa Itu Boost Converter?", "id": "Penaik Tegangan DC Ke DC." },
  { "en": "Apa Itu Buck Boost Converter?", "id": "Bisa Naik Dan Turun Tegangan." },
  { "en": "Apa Itu Cuk Converter?", "id": "Konverter DC Dengan Output Negatif." },
  { "en": "Apa Itu SEPIC Converter?", "id": "Tegangan Output Sama Polaritas Input." },
  { "en": "Komponen Utama Konverter DC Adalah?", "id": "Induktor Kapasitor Dioda Switch." },
  { "en": "Switch Pada Konverter DC Biasanya?", "id": "MOSFET Atau IGBT." },
  { "en": "Apa Fungsi Induktor Pada Buck Converter?", "id": "Menyimpan Energi Arus." },
  { "en": "Apa Itu Continuous Conduction Mode (CCM)?", "id": "Arus Induktor Tidak Pernah Nol." },
  { "en": "Apa Itu Discontinuous Conduction Mode (DCM)?", "id": "Arus Induktor Sempat Nol." },
  { "en": "Apa Itu Ripple Current Induktor?", "id": "Riak Arus Naik Turun." },
  { "en": "Apa Itu ESR Kapasitor?", "id": "Equivalent Series Resistance." },
  { "en": "ESR Tinggi Menyebabkan Apa?", "id": "Kapasitor Panas Dan Ripple Besar." },
  { "en": "Kapasitor Low ESR Biasa Digunakan Di?", "id": "Power Supply Switching SMPS." },
  { "en": "Apa Itu Kapasitor Tantalum?", "id": "Kapasitor Stabil Ukuran Kecil." },
  { "en": "Kelemahan Kapasitor Tantalum Adalah?", "id": "Bisa Meledak Jika Terbalik." },
  { "en": "Garis Pada Kapasitor Tantalum Menandakan?", "id": "Kutub Positif." },
  { "en": "Garis Pada Elco Biasa Menandakan?", "id": "Kutub Negatif." },
  { "en": "Apa Itu Kapasitor Supercap?", "id": "Kapasitor Nilai Farad Besar." },
  { "en": "Fungsi Supercap Adalah?", "id": "Cadangan Daya Sesaat." },
  { "en": "Apa Itu Induktor Inti Udara?", "id": "Induktor Tanpa Besi." },
  { "en": "Kelebihan Induktor Inti Udara?", "id": "Bagus Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu Induktor Inti Toroid?", "id": "Inti Berbentuk Cincin Donat." },
  { "en": "Kelebihan Toroid Adalah?", "id": "Minim Kebocoran Fluks Magnet." },
  { "en": "Apa Itu Skin Effect Pada Kawat?", "id": "Arus Mengalir Di Kulit Kabel." },
  { "en": "Cara Mengurangi Skin Effect Adalah?", "id": "Gunakan Kawat Litz Wire." },
  { "en": "Apa Itu Litz Wire?", "id": "Kawat Serabut Terisolasi Individu." },
  { "en": "Trafo Frekuensi Tinggi Menggunakan Kawat?", "id": "Litz Wire." },
  { "en": "Apa Itu EMI Filter?", "id": "Penyaring Gangguan Elektromagnetik." },
  { "en": "Komponen EMI Filter Terdiri Dari?", "id": "Induktor Common Mode Dan Kapasitor." },
  { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Antar Fasa Netral." },
  { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Antar Fasa Ground." },
  { "en": "Kapasitor Y Harus Jenis Apa?", "id": "Jenis Safety Class Y." },
  { "en": "Mengapa Kapasitor Y Harus Aman?", "id": "Mencegah Nyetrum Jika Bocor." },
  { "en": "Apa Itu Timer On Delay (TON)?", "id": "Menunda Waktu Hidup." },
  { "en": "Apa Itu Timer Off Delay (TOF)?", "id": "Menunda Waktu Mati." },
  { "en": "Apa Itu Retentive Timer (RTO)?", "id": "Menyimpan Waktu Saat Mati." },
  { "en": "Apa Itu Counter Up (CTU)?", "id": "Menghitung Maju." },
  { "en": "Apa Itu Counter Down (CTD)?", "id": "Menghitung Mundur." },
  { "en": "Apa Itu Bit Memori PLC?", "id": "Penyimpan Status Sementara." },
  { "en": "Apa Itu Analog Input PLC?", "id": "Menerima Sinyal Tegangan Arus." },
  { "en": "Resolusi Analog PLC 12 Bit Artinya?", "id": "4096 Tingkat Data." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Layar Sentuh Pengendali Mesin." },
  { "en": "HMI Terhubung Ke PLC Melalui?", "id": "Kabel Komunikasi Data." },
  { "en": "Apa Itu Recipe Pada HMI?", "id": "Penyimpanan Setelan Parameter Mesin." },
  { "en": "Apa Itu Trend Graph HMI?", "id": "Grafik Riwayat Data." },
  { "en": "Apa Itu Alarm History HMI?", "id": "Riwayat Kerusakan Mesin." },
  { "en": "Apa Itu Motor Shaded Pole?", "id": "Motor Induksi Kecil Murah." },
  { "en": "Ciri Fisik Motor Shaded Pole?", "id": "Ada Cincin Tembaga Di Kutub." },
  { "en": "Aplikasi Motor Shaded Pole Adalah?", "id": "Kipas Angin Kecil." },
  { "en": "Efisiensi Motor Shaded Pole Adalah?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Motor Kapasitor Permanen (PSC)?", "id": "Kapasitor Terhubung Terus." },
  { "en": "Motor Pompa Air Biasanya Jenis?", "id": "Motor Kapasitor Permanen." },
  { "en": "Apa Itu Centrifugal Switch Motor?", "id": "Sakelar Pemutus Kapasitor Start." },
  { "en": "Kapan Centrifugal Switch Bekerja?", "id": "Saat Kecepatan 75 Persen." },
  { "en": "Apa Itu Motor 3 Fasa Dual Voltage?", "id": "Bisa 220 Volt Dan 380 Volt." },
  { "en": "Jika Motor 220/380V Diberi 380V Wajib?", "id": "Hubungan Star Y." },
  { "en": "Jika Motor 380/660V Diberi 380V Wajib?", "id": "Hubungan Delta." },
  { "en": "Apa Itu Nameplate Frame Size?", "id": "Ukuran Fisik Standar Motor." },
  { "en": "Apa Itu Locked Rotor KVA?", "id": "Daya Semu Saat Start." },
  { "en": "Kode NEMA Design B Adalah?", "id": "Torsi Normal Arus Start Normal." },
  { "en": "Kode NEMA Design D Adalah?", "id": "Torsi Start Sangat Tinggi." },
  { "en": "Motor Design D Cocok Untuk?", "id": "Lift Dan Kerek." },
  { "en": "Apa Itu Thermistor PTC Motor?", "id": "Sensor Panas Gulungan." },
  { "en": "Jika Suhu Naik Resistansi PTC Akan?", "id": "Naik Drastis." },
  { "en": "Apa Itu Space Heater Motor?", "id": "Pemanas Anti Embun." },
  { "en": "Apa Itu Drip Proof Motor?", "id": "Tahan Tetesan Air Vertikal." },
  { "en": "Apa Itu TEFC (Totally Enclosed Fan Cooled)?", "id": "Motor Tertutup Berpendingin Kipas." },
  { "en": "Apa Itu TENV (Totally Enclosed Non Ventilated)?", "id": "Motor Tertutup Tanpa Kipas." },
  { "en": "Motor TENV Mendinginkan Diri Dengan?", "id": "Konveksi Alami Sirip Body." },
  { "en": "Apa Itu Isolator Tumpu (Pin Insulator)?", "id": "Isolator Tegak Di Atas Tiang." },
  { "en": "Apa Itu Isolator Tarik (Strain Insulator)?", "id": "Isolator Posisi Mendatar." },
  { "en": "Bahan Isolator Polimer Adalah?", "id": "Karet Silikon." },
  { "en": "Keunggulan Isolator Polimer Adalah?", "id": "Ringan Dan Tahan Polusi." },
  { "en": "Apa Itu Arcing Horn Isolator?", "id": "Tanduk Logam Pelindung Petir." },
  { "en": "Fungsi Arcing Horn Adalah?", "id": "Mengarahkan Busur Api Jauh Isolator." },
  { "en": "Apa Itu Dielectric Strength?", "id": "Kekuatan Isolasi Menahan Tegangan." },
  { "en": "Satuan Dielectric Strength Adalah?", "id": "kV Per mm." },
  { "en": "Apa Itu Breakdown Voltage Minyak?", "id": "Tegangan Tembus Minyak Trafo." },
  { "en": "Standar Breakdown Voltage Minyak Baru?", "id": "Minimal 30 kV." },
  { "en": "Apa Itu Tan Delta Isolasi?", "id": "Faktor Rugi Rugi Dielektrik." },
  { "en": "Nilai Tan Delta Yang Buruk?", "id": "Di Atas 1 Persen." },
  { "en": "Apa Itu Megger Test?", "id": "Uji Tahanan Isolasi Kabel." },
  { "en": "Tegangan Megger Untuk Kabel 380V?", "id": "500 Volt Atau 1000 Volt." },
  { "en": "Tegangan Megger Untuk Kabel 20kV?", "id": "5000 Volt." },
  { "en": "Berapa Lama Waktu Test Megger?", "id": "Minimal 1 Menit." },
  { "en": "Apa Itu Polarization Index (PI)?", "id": "Rasio Megger 10 Dan 1 Menit." },
  { "en": "Nilai PI Di Bawah 1 Artinya?", "id": "Isolasi Buruk Atau Basah." },
  { "en": "Apa Itu Earth Tester 3 Pole?", "id": "Metode Jatuh Tegangan Standar." },
  { "en": "Jarak Antar Paku Earth Tester?", "id": "5 Sampai 10 Meter." },
  { "en": "Apa Itu Clamp Earth Tester?", "id": "Ukur Grounding Tanpa Lepas Kabel." },
  { "en": "Syarat Pakai Clamp Earth Tester?", "id": "Harus Ada Loop Grounding." },
  { "en": "Apa Itu Kabel N2XSY?", "id": "Kabel Tegangan Menengah Isolasi XLPE." },
  { "en": "Apa Warna Selubung Kabel N2XSY?", "id": "Merah." },
  { "en": "Apa Itu Kabel N2XY?", "id": "Kabel Tegangan Rendah Isolasi XLPE." },
  { "en": "Apa Bedanya NYY Dan N2XY?", "id": "Isolasi PVC Vs XLPE." },
  { "en": "Suhu Kerja Maksimal Kabel NYY?", "id": "70 Derajat Celcius." },
  { "en": "Suhu Kerja Maksimal Kabel N2XY?", "id": "90 Derajat Celcius." },
  { "en": "Apa Itu Screen Tembaga Kabel TM?", "id": "Meratakan Medan Listrik." },
  { "en": "Screen Tembaga Harus Dihubungkan Ke?", "id": "Grounding Atau Tanah." },
  { "en": "Apa Itu Terminasi Indoor?", "id": "Sambungan Kabel Di Dalam Ruangan." },
  { "en": "Apa Itu Terminasi Outdoor?", "id": "Sambungan Kabel Di Luar Ruangan." },
  { "en": "Apa Fungsi Stress Control Tube?", "id": "Mengendalikan Medan Listrik Ujung Kabel." },
  { "en": "Apa Itu Jointing Kabel?", "id": "Penyambungan Dua Kabel." },
  { "en": "Apa Itu Resin Jointing?", "id": "Cor Cairan Pengeras Sambungan." },
  { "en": "Apa Itu Heat Shrink Jointing?", "id": "Selongsong Bakar Sambungan." },
  { "en": "Apa Itu Relay OCR Instantaneous?", "id": "Trip Seketika Tanpa Tunda." },
  { "en": "Kode ANSI Relay OCR Instantaneous?", "id": "50." },
  { "en": "Apa Itu Relay OCR Time Delay?", "id": "Trip Dengan Tunda Waktu." },
  { "en": "Kode ANSI Relay OCR Time Delay?", "id": "51." },
  { "en": "Apa Itu Relay Ground Fault?", "id": "Proteksi Arus Ke Tanah." },
  { "en": "Kode ANSI Relay Ground Fault?", "id": "51G Atau 51N." },
  { "en": "Apa Itu Relay Over Voltage?", "id": "Proteksi Tegangan Lebih." },
  { "en": "Kode ANSI Relay Over Voltage?", "id": "59." },
  { "en": "Apa Itu Relay Under Voltage?", "id": "Proteksi Tegangan Kurang." },
  { "en": "Kode ANSI Relay Under Voltage?", "id": "27." },
  { "en": "Apa Itu Relay Differential Trafo?", "id": "Membandingkan Arus Primer Sekunder." },
  { "en": "Kode ANSI Relay Differential?", "id": "87." },
  { "en": "Apa Itu Relay Buchholz?", "id": "Proteksi Gas Trafo Minyak." },
  { "en": "Kode ANSI Relay Buchholz?", "id": "63." },
  { "en": "Apa Itu Relay Distance?", "id": "Proteksi Impedansi Saluran Transmisi." },
  { "en": "Kode ANSI Relay Distance?", "id": "21." },
  { "en": "Apa Itu Relay Lockout?", "id": "Mengunci Sistem Setelah Trip." },
  { "en": "Kode ANSI Relay Lockout?", "id": "86." },
  { "en": "Relay Lockout Harus Direset Secara?", "id": "Manual." },
  { "en": "Apa Itu CT Jenuh (Saturation)?", "id": "Inti CT Tidak Mampu Transfer." },
  { "en": "Akibat CT Jenuh Adalah?", "id": "Arus Sekunder Tidak Linier." },
  { "en": "Apa Itu Knee Point Voltage CT?", "id": "Titik Mulai Jenuh CT." },
  { "en": "Apa Itu Class X CT?", "id": "CT Khusus Proteksi Differential." },
  { "en": "Apa Itu Burden VA CT?", "id": "Kapasitas Daya Beban CT." },
  { "en": "Apa Itu PT Atau VT?", "id": "Potential Transformer." },
  { "en": "Apa Itu CVT (Capacitive Voltage Transformer)?", "id": "PT Menggunakan Kapasitor Pembagi." },
  { "en": "Kelebihan CVT Dibanding PT Induktif?", "id": "Lebih Murah Di Tegangan Tinggi." },
  { "en": "Apa Itu Wave Trap (Line Trap)?", "id": "Perangkap Sinyal Komunikasi PLC." },
  { "en": "Wave Trap Dipasang Secara?", "id": "Seri Dengan Jaringan." },
  { "en": "Apa Itu PLC (Power Line Carrier)?", "id": "Komunikasi Via Kabel Listrik." },
  { "en": "Apa Fungsi Coupling Capacitor?", "id": "Memasukkan Sinyal PLC Ke Kabel." },
  { "en": "Apa Itu Gardu Induk GIS?", "id": "Gas Insulated Switchgear." },
  { "en": "Apa Itu Gardu Induk AIS?", "id": "Air Insulated Switchgear." },
  { "en": "Kelebihan AIS Adalah?", "id": "Biaya Awal Lebih Murah." },
  { "en": "Kelebihan GIS Adalah?", "id": "Tahan Polusi Dan Hemat Lahan." },
  { "en": "Apa Itu Marshalling Kiosk?", "id": "Kotak Terminal Kabel Kontrol Gardu." },
  { "en": "Apa Itu Bay Control Unit?", "id": "Pengontrol Satu Bay Gardu." },
  { "en": "Apa Itu HMI Scada?", "id": "Layar Monitor Sistem Tenaga." },
  { "en": "Apa Itu Single Line Diagram?", "id": "Gambar Satu Garis Sistem." },
  { "en": "Simbol Lingkaran Dengan Silang Adalah?", "id": "Lampu Atau Indikator." },
  { "en": "Simbol Lingkaran Huruf M Adalah?", "id": "Motor Listrik." },
  { "en": "Simbol Lingkaran Huruf G Adalah?", "id": "Generator." },
  { "en": "Simbol Zig Zag Adalah?", "id": "Resistor." },
  { "en": "Simbol Dua Garis Sejajar Adalah?", "id": "Kapasitor." },
  { "en": "Simbol Gulungan Kawat Adalah?", "id": "Induktor." },
  { "en": "Apa Itu MCCB 3 Pole?", "id": "Pemutus 3 Fasa." },
  { "en": "Apa Itu MCCB 4 Pole?", "id": "Pemutus 3 Fasa Dan Netral." },
  { "en": "Apa Itu Shunt Trip Coil?", "id": "Koil Pemutus Jarak Jauh." },
  { "en": "Apa Itu Under Voltage Release (UVR)?", "id": "Koil Pemutus Saat Tegangan Hilang." },
  { "en": "Apa Itu Motorized MCCB?", "id": "MCCB Dioperasikan Motor Listrik." },
  { "en": "Apa Itu Handle Operasi Rotary?", "id": "Tuas Putar MCCB." },
  { "en": "Apa Itu Interlock Mekanik?", "id": "Mencegah Dua Breaker On Bersamaan." },
  { "en": "Interlock Mekanik Biasa Dipakai Di?", "id": "Panel ATS Genset PLN." },
  { "en": "Apa Itu Change Over Switch (COS)?", "id": "Sakelar Pindah Sumber Daya." },
  { "en": "COS Tipe Ohm Saklar Berbentuk?", "id": "Tuas Geser Manual." },
  { "en": "Apa Itu LBS (Load Break Switch)?", "id": "Sakelar Pemutus Beban Menengah." },
  { "en": "LBS Dilengkapi Dengan Fuse Untuk?", "id": "Proteksi Hubung Singkat." },
  { "en": "Jika Satu Fuse LBS Putus Maka?", "id": "LBS Akan Trip 3 Fasa." },
  { "en": "Apa Itu Striker Pin Fuse?", "id": "Pin Penonjok Mekanik Fuse." },
  { "en": "Apa Fungsi Heater Di Panel?", "id": "Mencegah Kondensasi Air." },
  { "en": "Apa Fungsi Thermostat Di Panel?", "id": "Mengatur Suhu Kipas Heater." },
  { "en": "Apa Fungsi Hygrostat Di Panel?", "id": "Mengatur Kelembaban Udara." },
  { "en": "Apa Itu Busbar Tembaga Elektrolit?", "id": "Tembaga Murni Daya Hantar Tinggi." },
  { "en": "Standar Kemurnian Tembaga Busbar?", "id": "99,9 Persen." },
  { "en": "Apa Itu Heat Shrink Busbar?", "id": "Isolasi Warna Warni Busbar." },
  { "en": "Warna Standar Busbar Fasa R?", "id": "Merah." },
  { "en": "Warna Standar Busbar Fasa S?", "id": "Kuning." },
  { "en": "Warna Standar Busbar Fasa T?", "id": "Hitam." },
  { "en": "Warna Standar Busbar Netral?", "id": "Biru." },
  { "en": "Warna Standar Busbar Grounding?", "id": "Kuning Hijau." },
  { "en": "Apa Komponen Penyearah Pada Input VFD?", "id": "Dioda Bridge." },
  { "en": "Apa Komponen Switching Pada Output VFD?", "id": "IGBT." },
  { "en": "Apa Fungsi DC Link Pada VFD?", "id": "Menyimpan Energi DC Sementara." },
  { "en": "Apa Komponen Utama DC Link VFD?", "id": "Kapasitor Elektrolit Besar." },
  { "en": "Berapa Tegangan DC Bus Input 380V?", "id": "Sekitar 540 Volt DC." },
  { "en": "Apa Fungsi Braking Chopper?", "id": "Sakelar Pembuang Energi Rem." },
  { "en": "Braking Resistor Mengubah Energi Menjadi?", "id": "Energi Panas." },
  { "en": "Apa Itu Regenerative Braking Unit?", "id": "Mengembalikan Listrik Ke Jala." },
  { "en": "Apa Fungsi Line Reactor VFD?", "id": "Meredam Harmonisa Input." },
  { "en": "Apa Fungsi Load Reactor VFD?", "id": "Melindungi Isolasi Motor." },
  { "en": "Panjang Kabel Output VFD Maksimal Tanpa Filter?", "id": "Biasanya 50 Meter." },
  { "en": "Akibat Kabel VFD Terlalu Panjang Adalah?", "id": "Tegangan Refleksi Merusak Motor." },
  { "en": "Apa Itu Cable Tray?", "id": "Rak Penyangga Kabel." },
  { "en": "Apa Itu Cable Ladder?", "id": "Rak Kabel Bentuk Tangga." },
  { "en": "Cable Ladder Digunakan Untuk Kabel Jenis?", "id": "Kabel Daya Ukuran Besar." },
  { "en": "Cable Tray Solid Buntu Digunakan Untuk?", "id": "Kabel Data Sensitif Noise." },
  { "en": "Apa Fungsi Bonding Jumper Cable Tray?", "id": "Menyambung Grounding Antar Rak." },
  { "en": "Jarak Tumpuan Cable Tray Standar Adalah?", "id": "1,5 Sampai 2 Meter." },
  { "en": "Apa Itu Busduct Aluminium?", "id": "Rel Daya Batang Aluminium." },
  { "en": "Kelebihan Busduct Dibanding Kabel Besar?", "id": "Lebih Rapi Dan Dingin." },
  { "en": "Apa Itu Tap Off Box Busduct?", "id": "Kotak Cabang Daya Busduct." },
  { "en": "Apa Itu Sulfasi Pada Aki Basah?", "id": "Kristal Timbal Sulfat Mengeras." },
  { "en": "Penyebab Utama Sulfasi Aki Adalah?", "id": "Aki Kosong Terlalu Lama." },
  { "en": "Apa Itu Equalizing Charge Aki?", "id": "Cas Tegangan Tinggi Terkendali." },
  { "en": "Fungsi Equalizing Charge Adalah?", "id": "Merontokkan Sulfasi Ringan." },
  { "en": "Tanda Aki Mengalami Short Cell Adalah?", "id": "Satu Sel Mendidih Duluan." },
  { "en": "Apa Itu Hydrometer Aki?", "id": "Alat Ukur Berat Jenis." },
  { "en": "Berat Jenis Air Aki Penuh Adalah?", "id": "1,26 Sampai 1,28." },
  { "en": "Berat Jenis Air Aki Kosong Adalah?", "id": "Di Bawah 1,15." },
  { "en": "Apa Itu Single Phasing Motor 3 Fasa?", "id": "Hilang 1 Fasa Suplai." },
  { "en": "Akibat Single Phasing Pada Motor Adalah?", "id": "Motor Berdengung Dan Panas." },
  { "en": "Proteksi Terhadap Single Phasing Adalah?", "id": "Phase Failure Relay." },
  { "en": "Apa Itu Locked Rotor Test?", "id": "Uji Motor Rotor Ditahan." },
  { "en": "Apa Itu No Load Test Motor?", "id": "Uji Motor Tanpa Beban." },
  { "en": "Arus No Load Motor Biasanya?", "id": "30 Sampai 50 Persen." },
  { "en": "Apa Itu Harmonisa Urutan Nol?", "id": "Harmonisa Kelipatan 3." },
  { "en": "Harmonisa Urutan Nol Mengalir Di?", "id": "Kabel Netral." },
  { "en": "Akibat Harmonisa Urutan Nol Adalah?", "id": "Kabel Netral Sangat Panas." },
  { "en": "Apa Itu K-Rated Transformer?", "id": "Trafo Tahan Arus Harmonisa." },
  { "en": "Trafo K-4 Digunakan Untuk Beban?", "id": "Beban Elektronik Komputer." },
  { "en": "Trafo K-13 Digunakan Untuk Beban?", "id": "Data Center Dan UPS." },
  { "en": "Apa Itu Penangkal Petir Faraday?", "id": "Sangkar Kawat Keliling Bangunan." },
  { "en": "Apa Itu Penangkal Petir Franklin?", "id": "Tombak Runcing Di Atap." },
  { "en": "Sudut Perlindungan Franklin Rod Adalah?", "id": "Sekitar 45 Derajat." },
  { "en": "Apa Itu Down Conductor Petir?", "id": "Kabel Penurun Arus Petir." },
  { "en": "Kabel Down Conductor Minimal Ukuran?", "id": "50 Milimeter Persegi." },
  { "en": "Bahan Terbaik Grounding Rod Adalah?", "id": "Tembaga Murni." },
  { "en": "Mengapa Tanah Kering Buruk Untuk Grounding?", "id": "Resistivitas Tanah Tinggi." },
  { "en": "Cara Memperbaiki Grounding Tanah Kering?", "id": "Tambah Bentonit Atau Arang." },
  { "en": "Apa Itu Step Down Regulator Linear?", "id": "Menurunkan Tegangan Membuang Panas." },
  { "en": "Apa Itu Step Down Regulator Switching?", "id": "Menurunkan Tegangan Efisiensi Tinggi." },
  { "en": "IC LM7805 Termasuk Jenis Regulator?", "id": "Linear Regulator." },
  { "en": "Modul LM2596 Termasuk Jenis Regulator?", "id": "Switching Regulator Buck." },
  { "en": "Transistor PNP Aktif Jika Basis?", "id": "Diberi Tegangan Negatif." },
  { "en": "Transistor NPN Aktif Jika Basis?", "id": "Diberi Tegangan Positif." },
  { "en": "Apa Fungsi Heatsink Compound (Pasta)?", "id": "Mengisi Celah Udara Mikro." },
  { "en": "Apa Itu Thermal Pad?", "id": "Bantalan Penghantar Panas." },
  { "en": "Bahan Isolator Mica Digunakan Untuk?", "id": "Isolasi Listrik Transistor Heatsink." },
  { "en": "Apa Itu Galvanic Isolation Transformer?", "id": "Memisahkan Ground Primer Sekunder." },
  { "en": "Apa Itu Autotransformator (Variac)?", "id": "Satu Gulungan Tanpa Isolasi." },
  { "en": "Keunggulan Variac Adalah?", "id": "Tegangan Output Bisa Diatur." },
  { "en": "Bahaya Variac Adalah?", "id": "Nyeterum Karena Tidak Terisolasi." },
  { "en": "Apa Itu Crimping Skun?", "id": "Menekan Terminal Kabel." },
  { "en": "Skun Garpu (Fork Lug) Digunakan Di?", "id": "Terminal Baut Terminal Block." },
  { "en": "Skun Ring (Ring Lug) Digunakan Di?", "id": "Baut Busbar Atau Grounding." },
  { "en": "Skun Pin (Pin Lug) Digunakan Di?", "id": "Lubang MCB." },
  { "en": "Warna Isolasi Skun Merah Untuk Kabel?", "id": "0,5 Sampai 1,5 mm." },
  { "en": "Warna Isolasi Skun Biru Untuk Kabel?", "id": "1,5 Sampai 2,5 mm." },
  { "en": "Warna Isolasi Skun Kuning Untuk Kabel?", "id": "4 Sampai 6 mm." },
  { "en": "Apa Itu Busbar Isolator (Support)?", "id": "Penyangga Busbar Di Panel." },
  { "en": "Bahan Busbar Support Biasanya?", "id": "Resin Atau Keramik." },
  { "en": "Apa Itu Pilot Light 22mm?", "id": "Lampu Indikator Panel Standar." },
  { "en": "Apa Itu Push Button Momentary?", "id": "Balik Posisi Saat Dilepas." },
  { "en": "Apa Itu Push Button Latching?", "id": "Terkunci Saat Ditekan." },
  { "en": "Apa Itu Emergency Stop Mushroom?", "id": "Tombol Jamur Merah Besar." },
  { "en": "Cara Reset Emergency Stop Adalah?", "id": "Putar Kepala Tombol." },
  { "en": "Apa Itu Selector Switch 3 Posisi?", "id": "Sakelar Manual Off Auto." },
  { "en": "Apa Itu Panel LVMDP?", "id": "Low Voltage Main Distribution." },
  { "en": "Apa Itu Panel Cap Bank?", "id": "Panel Kapasitor Bank." },
  { "en": "Apa Fungsi Power Factor Controller?", "id": "Mengatur Step Kapasitor Otomatis." },
  { "en": "Apa Itu Step Kapasitor?", "id": "Jumlah Tahapan Kapasitor." },
  { "en": "Kontaktor Kapasitor Harus Memiliki?", "id": "Resistor Damping Awal." },
  { "en": "Mengapa Perlu Resistor Pada Kontaktor?", "id": "Meredam Arus Inrush Kapasitor." },
  { "en": "Apa Itu Detuned Reactor?", "id": "Induktor Seri Kapasitor Bank." },
  { "en": "Fungsi Detuned Reactor Adalah?", "id": "Mencegah Resonansi Harmonisa." },
  { "en": "Apa Itu THDv?", "id": "Total Distorsi Harmonisa Tegangan." },
  { "en": "Apa Itu THDi?", "id": "Total Distorsi Harmonisa Arus." },
  { "en": "Standar THDv Maksimal Di Industri?", "id": "5 Persen." },
  { "en": "Apa Itu Flicker Meter?", "id": "Alat Ukur Kedipan Tegangan." },
  { "en": "Apa Itu Power Quality Analyzer?", "id": "Alat Analisis Kualitas Daya." },
  { "en": "PQA Mengukur Parameter Apa?", "id": "Harmonisa Transien Dan Dip." },
  { "en": "Apa Itu Voltage Dip (Sag)?", "id": "Tegangan Turun Sesaat." },
  { "en": "Apa Itu Voltage Swell?", "id": "Tegangan Naik Sesaat." },
  { "en": "Penyebab Utama Voltage Dip Adalah?", "id": "Start Motor Besar." },
  { "en": "Apa Itu UPS Double Conversion?", "id": "Selalu Lewat Inverter Online." },
  { "en": "Apa Itu LED Driver Constant Current?", "id": "Arus Output Tetap Stabil." },
  { "en": "Apa Itu LED Driver Constant Voltage?", "id": "Tegangan Output Tetap Stabil." },
  { "en": "Lampu LED Strip Menggunakan Driver Jenis?", "id": "Constant Voltage." },
  { "en": "Lampu Sorot High Power Menggunakan Driver?", "id": "Constant Current." },
  { "en": "Apa Itu Luminous Efficacy?", "id": "Efisiensi Cahaya Per Watt." },
  { "en": "Satuan Luminous Efficacy Adalah?", "id": "Lumen Per Watt." },
  { "en": "Apa Itu Binning Pada LED?", "id": "Pengelompokan Kualitas Chip LED." },
  { "en": "Apa Itu MacAdam Ellipse?", "id": "Standar Konsistensi Warna Cahaya." },
  { "en": "Apa Itu L70 Rating Pada LED?", "id": "Umur Saat Cahaya Sisa 70%." },
  { "en": "Apa Itu LED COB (Chip On Board)?", "id": "Banyak Chip Dalam Satu Modul." },
  { "en": "Apa Itu LED SMD 5050?", "id": "Ukuran 5,0 Kali 5,0 mm." },
  { "en": "Apa Itu LED SMD 2835?", "id": "Ukuran 2,8 Kali 3,5 mm." },
  { "en": "Apa Itu LED Filament?", "id": "LED Bentuk Benang Pijar." },
  { "en": "Apa Fungsi Thermal Pad PCB LED?", "id": "Membuang Panas Chip Ke Heatsink." },
  { "en": "Apa Itu MCPCB (Metal Core PCB)?", "id": "PCB Dengan Inti Logam Aluminium." },
  { "en": "MCPCB Wajib Digunakan Untuk?", "id": "LED Daya Tinggi." },
  { "en": "Apa Itu Sinkronoskop?", "id": "Alat Melihat Beda Fasa Genset." },
  { "en": "Jarum Sinkronoskop Jam 12 Artinya?", "id": "Fasa Sinkron Sempurna." },
  { "en": "Jarum Berputar Cepat Searah Jarum Jam?", "id": "Frekuensi Genset Terlalu Cepat." },
  { "en": "Jarum Berputar Berlawanan Jarum Jam?", "id": "Frekuensi Genset Terlalu Lambat." },
  { "en": "Apa Itu Isochronous Control Genset?", "id": "Frekuensi Tetap Walau Beban Berubah." },
  { "en": "Apa Itu Droop Control Genset?", "id": "Frekuensi Turun Saat Beban Naik." },
  { "en": "Tujuan Droop Control Adalah?", "id": "Berbagi Beban Genset Paralel." },
  { "en": "Apa Itu Pitch Factor Pada Generator?", "id": "Rasio Kisar Lilitan Stator." },
  { "en": "Apa Itu Distribution Factor Generator?", "id": "Penyebaran Lilitan Dalam Alur." },
  { "en": "Apa Itu Kabel FRC (Fire Resistant Cable)?", "id": "Kabel Tahan Api Saat Kebakaran." },
  { "en": "Isolasi Kabel FRC Menggunakan Bahan?", "id": "Mica Tape Dan XLPE." },
  { "en": "Berapa Lama Kabel FRC Tahan Api?", "id": "Minimal 3 Jam." },
  { "en": "Apa Itu Kabel Low Smoke Zero Halogen?", "id": "Sedikit Asap Tidak Beracun." },
  { "en": "Kabel LSZH Wajib Digunakan Di?", "id": "Gedung Bertingkat Dan Kapal." },
  { "en": "Apa Bahaya Kabel PVC Saat Terbakar?", "id": "Gas Klorin Beracun." },
  { "en": "Apa Itu Kabel Instrumentasi Screened?", "id": "Kabel Sinyal Anti Noise." },
  { "en": "Apa Fungsi Drain Wire Pada Kabel?", "id": "Membuang Noise Shield Ke Ground." },
  { "en": "Apa Itu Trip Circuit Supervision (TCS)?", "id": "Memantau Jalur Kabel Trip Breaker." },
  { "en": "Kode Relay TCS Adalah?", "id": "74." },
  { "en": "Jika Lampu TCS Mati Artinya?", "id": "Jalur Trip Breaker Putus." },
  { "en": "Apa Itu Auto Reclose Relay?", "id": "Menutup Breaker Otomatis Setelah Trip." },
  { "en": "Kode Relay Auto Reclose Adalah?", "id": "79." },
  { "en": "Auto Reclose Hanya Boleh Untuk?", "id": "Gangguan Sesaat Saluran Udara." },
  { "en": "Apa Itu Dead Bus Check?", "id": "Memastikan Busbar Tak Bertegangan." },
  { "en": "Apa Itu Synchro Check Relay?", "id": "Memastikan Syarat Sinkron Terpenuhi." },
  { "en": "Kode Relay Synchro Check Adalah?", "id": "25." },
  { "en": "Apa Itu Negative Sequence Relay?", "id": "Proteksi Beban Tidak Seimbang." },
  { "en": "Kode Relay Negative Sequence Adalah?", "id": "46." },
  { "en": "Arus Urutan Negatif Menyebabkan?", "id": "Rotor Generator Sangat Panas." },
  { "en": "Apa Itu BMS Balancing Pasif?", "id": "Membuang Energi Sel Tinggi." },
  { "en": "BMS Pasif Menggunakan Komponen?", "id": "Resistor Pembuang Panas." },
  { "en": "Apa Itu BMS Balancing Aktif?", "id": "Memindah Energi Ke Sel Rendah." },
  { "en": "BMS Aktif Menggunakan Komponen?", "id": "Induktor Atau Kapasitor." },
  { "en": "Apa Itu Thermal runaway Pada Baterai?", "id": "Suhu Naik Tak Terkendali." },
  { "en": "Penyebab Thermal Runaway Adalah?", "id": "Short Circuit Internal Atau Overcharge." },
  { "en": "Apa Itu Venting Pada Baterai?", "id": "Pelepasan Gas Tekanan Tinggi." },
  { "en": "Apa Itu Separator Baterai?", "id": "Pemisah Anoda Dan Katoda." },
  { "en": "Fungsi Separator Adalah?", "id": "Mencegah Hubung Singkat Internal." },
  { "en": "Apa Itu Dendrit Pada Baterai Lithium?", "id": "Kristal Tajam Penembus Separator." },
  { "en": "Apa Itu Grounding Grid Gardu Induk?", "id": "Jaring Kawat Tanam Bawah Tanah." },
  { "en": "Tujuan Grounding Grid Adalah?", "id": "Menurunkan Tegangan Langkah." },
  { "en": "Apa Itu Pigtail Grounding?", "id": "Kabel Serabut Grounding Fleksibel." },
  { "en": "Apa Itu Bonding Equipotential?", "id": "Menyamakan Potensial Semua Logam." },
  { "en": "Apa Itu Grounding Busbar Utama?", "id": "Terminal Pusat Pembumian Gedung." },
  { "en": "Apa Itu Neutral Grounding Resistor (NGR)?", "id": "Resistor Di Titik Netral Trafo." },
  { "en": "Fungsi NGR Adalah?", "id": "Membatasi Arus Gangguan Tanah." },
  { "en": "Trafo Tanpa NGR Disebut?", "id": "Solidly Grounded." },
  { "en": "Arus Gangguan Tanah Solid Grounding Adalah?", "id": "Sangat Besar." },
  { "en": "Apa Itu Motor Excitation?", "id": "Pemberian Arus Penguat Medan." },
  { "en": "Motor DC Seri Tanpa Beban Akan?", "id": "Kecepatan Naik Sampai Rusak." },
  { "en": "Mengapa Motor DC Seri Bahaya Kosong?", "id": "Torsi Dan Fluks Berbanding Terbalik." },
  { "en": "Apa Itu Regenerative Braking Lift?", "id": "Lift Turun Menghasilkan Listrik." },
  { "en": "Listrik Regeneratif Lift Disalurkan Ke?", "id": "Jaringan Gedung Atau Resistor." },
  { "en": "Apa Itu Duty Cycle Mesin Spot Welding?", "id": "Sangat Rendah Arus Besar." },
  { "en": "Elektroda Spot Welding Terbuat Dari?", "id": "Tembaga Khusus." },
  { "en": "Apa Itu Ferromagnetic Resonance?", "id": "Resonansi Inti Besi Trafo." },
  { "en": "Apa Itu Eddy Current Brake?", "id": "Pengereman Menggunakan Medan Magnet." },
  { "en": "Eddy Current Brake Adalah Pengereman?", "id": "Tanpa Kontak Fisik." },
  { "en": "Apa Itu Solenoid Valve 5/2?", "id": "5 Lubang 2 Posisi." },
  { "en": "Apa Itu Solenoid Valve 3/2?", "id": "3 Lubang 2 Posisi." },
  { "en": "Lubang P Pada Pneumatik Adalah?", "id": "Pressure Atau Supply Angin." },
  { "en": "Lubang R Dan S Pada Pneumatik?", "id": "Exhaust Atau Pembuangan." },
  { "en": "Lubang A Dan B Pada Pneumatik?", "id": "Output Ke Silinder." },
  { "en": "Apa Itu Air Service Unit?", "id": "Filter Regulator Pelumas Angin." },
  { "en": "Tekanan Angin Instrumentasi Standar?", "id": "Sekitar 4 Sampai 6 Bar." },
  { "en": "Apa Itu Protocol Hart?", "id": "Sinyal Digital Di Atas 4-20mA." },
  { "en": "Apa Itu Protocol Foundation Fieldbus?", "id": "Komunikasi Digital Full Bus." },
  { "en": "Apa Itu Protocol Modbus ASCII?", "id": "Format Data Teks Terbaca." },
  { "en": "Apa Itu Protocol Modbus RTU?", "id": "Format Data Biner Kompak." },
  { "en": "CRC (Cyclic Redundancy Check) Digunakan Untuk?", "id": "Cek Error Data Modbus." },
  { "en": "Apa Itu Kabel Belden 9841?", "id": "Kabel Standar RS485." },
  { "en": "Impedansi Kabel Belden 9841 Adalah?", "id": "120 Ohm." },
  { "en": "Apa Itu Fiber Optic Single Core?", "id": "Komunikasi 2 Arah 1 Kabel." },
  { "en": "Teknologi Single Core Menggunakan?", "id": "WDM Wavelength Division Multiplexing." },
  { "en": "Sinar Kirim Dan Terima Dibedakan?", "id": "Panjang Gelombang Warna Cahaya." },
  { "en": "Panjang Gelombang Umum FO Single Mode?", "id": "1310 nm Dan 1550 nm." },
  { "en": "Apa Itu dB Loss Budget?", "id": "Batas Maksimal Redaman Kabel." },
  { "en": "Apa Itu Holding Current Pada SCR?", "id": "Arus Minimal Agar Tetap On." },
  { "en": "Apa Itu Latching Current Pada SCR?", "id": "Arus Minimal Untuk Mulai On." },
  { "en": "Mana Lebih Besar Latching Atau Holding?", "id": "Latching Current Lebih Besar." },
  { "en": "Apa Itu Reverse Recovery Time Dioda?", "id": "Waktu Pemulihan Blokir Arus Balik." },
  { "en": "Dioda Fast Recovery Digunakan Di Mana?", "id": "Power Supply Switching Frekuensi Tinggi." },
  { "en": "Apa Itu SOA (Safe Operating Area)?", "id": "Area Kerja Aman Transistor." },
  { "en": "Apa Akibat Transistor Keluar Dari SOA?", "id": "Komponen Rusak Terbakar." },
  { "en": "Apa Itu Thermal Derating Curve?", "id": "Kurva Penurunan Daya Akibat Suhu." },
  { "en": "Apa Itu Thermal Resistance (Rth)?", "id": "Hambatan Alir Panas Ke Heatsink." },
  { "en": "Satuan Thermal Resistance Adalah?", "id": "Derajat Celcius Per Watt." },
  { "en": "Apa Itu Gain P (Proportional) Pada PID?", "id": "Respon Sesuai Besar Error." },
  { "en": "Apa Itu Gain I (Integral) Pada PID?", "id": "Akumulasi Error Seiring Waktu." },
  { "en": "Apa Itu Gain D (Derivative) Pada PID?", "id": "Prediksi Laju Perubahan Error." },
  { "en": "Efek Gain P Terlalu Besar Adalah?", "id": "Sistem Berosilasi Tidak Stabil." },
  { "en": "Efek Gain I Terlalu Besar Adalah?", "id": "Respon Lambat Dan Overshoot." },
  { "en": "Apa Fungsi Integral Windup Protection?", "id": "Mencegah Akumulasi Error Berlebih." },
  { "en": "Apa Itu Setpoint (SP) Kendali?", "id": "Nilai Target Yang Diinginkan." },
  { "en": "Apa Itu Process Variable (PV)?", "id": "Nilai Aktual Dari Sensor." },
  { "en": "Apa Itu Manipulated Variable (MV)?", "id": "Sinyal Output Ke Aktuator." },
  { "en": "Apa Itu Sistem Per Unit (PU)?", "id": "Normalisasi Nilai Terhadap Dasar." },
  { "en": "Keuntungan Sistem Per Unit Adalah?", "id": "Mudah Hitung Beda Level Tegangan." },
  { "en": "Apa Itu Komponen Simetris Urutan Positif?", "id": "Arah Putaran Normal Generator." },
  { "en": "Apa Itu Komponen Simetris Urutan Negatif?", "id": "Arah Putaran Berlawanan Generator." },
  { "en": "Apa Itu Komponen Simetris Urutan Nol?", "id": "Arus Ke Tanah Atau Netral." },
  { "en": "Gangguan 1 Fasa Ke Tanah Menghasilkan?", "id": "Urutan Positif Negatif Dan Nol." },
  { "en": "Gangguan Antar Fasa Murni Menghasilkan?", "id": "Hanya Urutan Positif Dan Negatif." },
  { "en": "Apa Itu Swing Equation?", "id": "Persamaan Kestabilan Rotor Generator." },
  { "en": "Apa Itu Critical Clearing Time?", "id": "Waktu Maksimal Memutus Gangguan." },
  { "en": "Apa Itu Infinite Bus (Rel Tak Hingga)?", "id": "Tegangan Dan Frekuensi Konstan." },
  { "en": "Apa Itu Smith Chart?", "id": "Grafik Impedansi Dan Refleksi RF." },
  { "en": "Titik Tengah Smith Chart Adalah?", "id": "Impedansi 50 Ohm Sempurna." },
  { "en": "Lingkaran Luar Smith Chart Adalah?", "id": "Reaktansi Murni Tanpa Resistansi." },
  { "en": "Apa Itu Waveguide?", "id": "Pipa Logam Pemandu Gelombang." },
  { "en": "Mengapa Waveguide Bukan Kabel Biasa?", "id": "Menghindari Rugi Rugi Frekuensi Tinggi." },
  { "en": "Apa Itu Mode TE Pada Waveguide?", "id": "Transverse Electric Mode." },
  { "en": "Apa Itu Mode TM Pada Waveguide?", "id": "Transverse Magnetic Mode." },
  { "en": "Apa Itu Flux Weakening Motor?", "id": "Mengurangi Medan Agar Putaran Naik." },
  { "en": "Kapan Flux Weakening Digunakan?", "id": "Kecepatan Di Atas Rated Speed." },
  { "en": "Apa Resiko Flux Weakening?", "id": "Torsi Motor Menjadi Turun." },
  { "en": "Apa Itu Motor Servo Absolute?", "id": "Posisi Tetap Tahu Walau Mati." },
  { "en": "Apa Itu Motor Servo Incremental?", "id": "Posisi Hilang Saat Listrik Mati." },
  { "en": "Apa Itu Resolver Motor?", "id": "Sensor Posisi Rotor Analog." },
  { "en": "Keunggulan Resolver Dibanding Encoder?", "id": "Tahan Panas Dan Getaran Kasar." },
  { "en": "Apa Itu SIL (Safety Integrity Level)?", "id": "Tingkat Keandalan Fungsi Keselamatan." },
  { "en": "SIL Level 4 Artinya?", "id": "Tingkat Keamanan Paling Tinggi." },
  { "en": "Apa Itu SIS (Safety Instrumented System)?", "id": "Sistem Otomatis Pencegah Bahaya." },
  { "en": "Tombol ESD Biasanya Berwarna Apa?", "id": "Merah Dengan Latar Kuning." },
  { "en": "Apa Itu Proof Test Interval?", "id": "Jadwal Uji Fungsi Keselamatan." },
  { "en": "Apa Itu Burden Resistor CT?", "id": "Resistor Beban Sekunder Trafo Arus." },
  { "en": "Apa Bahaya Burden CT Terlalu Besar?", "id": "CT Jenuh Dan Tidak Akurat." },
  { "en": "Apa Itu Kawat Email (Magnet Wire)?", "id": "Kawat Tembaga Lapis Isolasi Tipis." },
  { "en": "Kelas Isolasi Email E Tahan Suhu?", "id": "120 Derajat Celcius." },
  { "en": "Kelas Isolasi Email F Tahan Suhu?", "id": "155 Derajat Celcius." },
  { "en": "Kelas Isolasi Email H Tahan Suhu?", "id": "180 Derajat Celcius." },
  { "en": "Apa Itu Internal Resistance Baterai?", "id": "Hambatan Dalam Sel Baterai." },
  { "en": "Hambatan Dalam Tinggi Menyebabkan Apa?", "id": "Tegangan Drop Saat Diberi Beban." },
  { "en": "Apa Itu Sulfasi Keras (Hard Sulfation)?", "id": "Kerusakan Aki Permanen Kristal Keras." },
  { "en": "Apa Itu Stratifikasi Asam Aki?", "id": "Asam Berat Mengendap Di Bawah." },
  { "en": "Apa Fungsi PLL (Phase Locked Loop)?", "id": "Mengunci Frekuensi Dan Fasa Sinyal." },
  { "en": "Apa Itu VCO (Voltage Controlled Oscillator)?", "id": "Frekuensi Berubah Sesuai Tegangan Input." },
  { "en": "Osilator Hartley Menggunakan Umpan Balik?", "id": "Induktor Center Tap." },
  { "en": "Osilator Colpitts Menggunakan Umpan Balik?", "id": "Pembagi Tegangan Kapasitor." },
  { "en": "Keunggulan Osilator Kristal Adalah?", "id": "Frekuensi Sangat Stabil." },
  { "en": "Apa Itu Heat Gun?", "id": "Pemanas Udara Panas Listrik." },
  { "en": "Apa Itu Thermal Imaging Camera?", "id": "Kamera Pendeteksi Suhu Panas." },
  { "en": "Apa Itu Emisivitas Benda?", "id": "Kemampuan Benda Memancarkan Panas." },
  { "en": "Nilai Emisivitas Benda Hitam Sempurna?", "id": "1." },
  { "en": "Nilai Emisivitas Tembaga Mengkilap?", "id": "Sangat Rendah." },
  { "en": "Agar Thermogun Akurat Pada Logam Harus?", "id": "Tempel Lakban Hitam Dulu." },
  { "en": "Apa Itu Current Mirror?", "id": "Menyalin Arus Satu Ke Lainnya." },
  { "en": "Rangkaian Current Mirror Menggunakan?", "id": "Dua Transistor Identik." },
  { "en": "Apa Itu Wilson Current Mirror?", "id": "Cermin Arus Presisi 3 Transistor." },
  { "en": "Apa Itu Bandgap Reference?", "id": "Referensi Tegangan Stabil Suhu." },
  { "en": "Berapa Tegangan Bandgap Silicon?", "id": "Sekitar 1,25 Volt." },
  { "en": "Apa Itu Folded Cascode?", "id": "Topologi Penguat Op Amp." },
  { "en": "Apa Itu Rail To Rail Op Amp?", "id": "Output Bisa Mencapai Tegangan Suplai." },
  { "en": "Op Amp Biasa Outputnya Terpotong Sebesar?", "id": "1 Sampai 2 Volt." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Error Input Op Amp." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Bocor Masuk Kaki Input." },
  { "en": "Apa Itu GBWP (Gain Bandwidth Product)?", "id": "Bandwidth Pada Penguatan 1 Kali." },
  { "en": "Apa Itu Stability Margin?", "id": "Batas Kestabilan Sistem Kendali." },
  { "en": "Apa Itu Phase Margin?", "id": "Sisa Fasa Sebelum Osilasi." },
  { "en": "Apa Itu Gain Margin?", "id": "Sisa Penguatan Sebelum Osilasi." },
  { "en": "Apa Itu Bode Plot?", "id": "Grafik Respon Frekuensi Sistem." },
  { "en": "Apa Itu Nyquist Plot?", "id": "Grafik Kestabilan Koordinat Polar." },
  { "en": "Apa Itu Root Locus?", "id": "Tempat Kedudukan Akar Kestabilan." },
  { "en": "Sistem Stabil Jika Pole Berada Di?", "id": "Sebelah Kiri Sumbu Imajiner." },
  { "en": "Apa Itu Lead Compensator?", "id": "Mempercepat Respon Transien." },
  { "en": "Apa Itu Lag Compensator?", "id": "Mengurangi Error Steady State." },
  { "en": "Apa Itu Zener Barrier?", "id": "Pembatas Energi Area Berbahaya." },
  { "en": "Zener Barrier Wajib Memiliki?", "id": "Grounding Yang Sangat Baik." },
  { "en": "Apa Itu Fuse Karakteristik aM?", "id": "Khusus Proteksi Motor." },
  { "en": "Apa Itu Fuse Karakteristik gG?", "id": "Penggunaan Umum Kabel." },
  { "en": "Apa Itu HRC Fuse (High Rupturing)?", "id": "Sekering Kapasitas Putus Tinggi." },
  { "en": "Isi Pasir Kuarsa Dalam Fuse Berfungsi?", "id": "Memadamkan Busur Api." },
  { "en": "Apa Itu Fuse Holder?", "id": "Rumah Dudukan Sekering." },
  { "en": "Apa Itu NH Fuse (HRC Pisau)?", "id": "Sekering Tegangan Rendah Daya Besar." },
  { "en": "Alat Pasang NH Fuse Disebut?", "id": "NH Fuse Puller Handle." },
  { "en": "Apa Itu Arc Chute Breaker?", "id": "Kisi Kisi Pemadam Api." },
  { "en": "Apa Itu Teorema Millman?", "id": "Menyederhanakan Banyak Sumber Paralel." },
  { "en": "Apa Itu Teorema Reciprocity?", "id": "Pertukaran Posisi Sumber Dan Arus." },
  { "en": "Apa Itu Efek Proximity Pada Kabel?", "id": "Arus Terpusat Akibat Kabel Berdekatan." },
  { "en": "Apa Fungsi Corona Ring?", "id": "Meratakan Medan Listrik Tegangan Tinggi." },
  { "en": "Apa Itu Bahan Piezoelektrik?", "id": "Kristal Menghasilkan Listrik Saat Ditekan." },
  { "en": "Apa Itu Optoisolator?", "id": "Pemisah Sinyal Menggunakan Cahaya." },
  { "en": "Apa Itu Sziklai Pair?", "id": "Pasangan Transistor NPN Dan PNP." },
  { "en": "Apa Itu Bootstrap Capacitor?", "id": "Menyimpan Tegangan Untuk Gate Driver." },
  { "en": "Apa Itu Snubber RC?", "id": "Resistor Kapasitor Peredam Lonjakan." },
  { "en": "Apa Itu Avalanche Breakdown?", "id": "Kerusakan Dioda Akibat Tegangan Tinggi." },
  { "en": "Apa Itu Zener Breakdown?", "id": "Dadal Tegangan Rendah Terkontrol." },
  { "en": "Apa Itu ESR Meter?", "id": "Alat Ukur Hambatan Seri Kapasitor." },
  { "en": "Apa Itu Dry Joint Solder?", "id": "Timah Tidak Membasahi Kaki Komponen." },
  { "en": "Apa Itu Flux Rosin Core?", "id": "Timah Berisi Inti Flux." },
  { "en": "Suhu Leleh Timah 60/40 Adalah?", "id": "Sekitar 183 Derajat Celcius." },
  { "en": "Suhu Leleh Timah Lead Free?", "id": "Sekitar 217 Derajat Celcius." },
  { "en": "Apa Itu Jumper Wire?", "id": "Kabel Penghubung Jalur Rangkaian." },
  { "en": "Apa Itu Test Point PCB?", "id": "Titik Pengukuran Probe Alat Ukur." },
  { "en": "Apa Itu Fiducial Mark PCB?", "id": "Tanda Referensi Mesin Pick Place." },
  { "en": "Apa Itu Panelization PCB?", "id": "Menggabungkan Banyak PCB Satu Papan." },
  { "en": "Apa Itu Solder Mask Dam?", "id": "Penyekat Solder Antar Pad." },
  { "en": "Apa Itu Thermal Relief Pad?", "id": "Memudahkan Solder Kaki Ground." },
  { "en": "Mengapa Thermal Relief Penting?", "id": "Mencegah Panas Terserap Ground Plane." },
  { "en": "Apa Itu Via Stitching?", "id": "Banyak Via Menghubungkan Ground Plane." },
  { "en": "Apa Fungsi Via Stitching?", "id": "Mengurangi Impedansi Dan Shielding." },
  { "en": "Apa Itu EMI Shielding Can?", "id": "Kaleng Logam Penutup Rangkaian RF." },
  { "en": "Apa Itu Skun Y (Spade)?", "id": "Terminal Kabel Bentuk Garpu." },
  { "en": "Apa Itu Heat Shrink Ratio 2:1?", "id": "Menyusut Setengah Ukuran Asli." },
  { "en": "Apa Itu Heat Shrink Ratio 3:1?", "id": "Menyusut Sepertiga Ukuran Asli." },
  { "en": "Apa Itu Cable Tie Nylon?", "id": "Pengikat Kabel Plastik Standar." },
  { "en": "Apa Itu Cable Tie Stainless?", "id": "Pengikat Kabel Tahan Panas Karat." },
  { "en": "Apa Itu Spiral Wrapping Band?", "id": "Pelindung Kabel Fleksibel Spiral." },
  { "en": "Apa Itu Braided Sleeving?", "id": "Pelindung Kabel Anyaman Jaring." },
  { "en": "Apa Itu Duck Duct (Trunking)?", "id": "Pelindung Kabel Kotak Di Dinding." },
  { "en": "Apa Itu Floor Duct?", "id": "Pelindung Kabel Di Lantai." },
  { "en": "Apa Itu Grommet Karet?", "id": "Pelindung Kabel Lubang Panel." },
  { "en": "Apa Fungsi Grommet?", "id": "Mencegah Kabel Tergores Plat Tajam." },
  { "en": "Apa Itu Strain Relief Kabel?", "id": "Penahan Tarikan Pada Pangkal Kabel." },
  { "en": "Apa Itu Power Entry Module?", "id": "Soket Sakelar Sekering Jadi Satu." },
  { "en": "Soket IEC C13 Digunakan Untuk?", "id": "Kabel Power Komputer PC." },
  { "en": "Soket IEC C14 Adalah?", "id": "Lubang Colokan Di Power Supply." },
  { "en": "Soket IEC C19 Digunakan Untuk?", "id": "Peralatan Server Daya Besar." },
  { "en": "Apa Itu Schuko Plug?", "id": "Steker Bulat Standar Eropa Indonesia." },
  { "en": "Apa Itu NEMA 5-15 Plug?", "id": "Steker Pipih Standar Amerika." },
  { "en": "Apa Itu BS 1363 Plug?", "id": "Steker Kaki Tiga Inggris." },
  { "en": "Apa Itu Fuse Slow Blow?", "id": "Sekering Putus Lambat." },
  { "en": "Apa Itu Fuse Fast Blow?", "id": "Sekering Putus Cepat." },
  { "en": "Kapan Pakai Fuse Slow Blow?", "id": "Beban Induktif Arus Start Besar." },
  { "en": "Kode Fuse T Artinya?", "id": "Time Delay Atau Slow Blow." },
  { "en": "Kode Fuse F Artinya?", "id": "Fast Acting Atau Cepat." },
  { "en": "Apa Itu Resettable Fuse PPTC?", "id": "Sekering Bisa Sambung Sendiri." },
  { "en": "Apa Itu Spark Gap?", "id": "Celah Udara Loncat Api Tegangan." },
  { "en": "Apa Itu TVS Unidirectional?", "id": "Proteksi Satu Arah Polaritas." },
  { "en": "Apa Itu TVS Bidirectional?", "id": "Proteksi Dua Arah Bolak Balik." },
  { "en": "Apa Itu Load Cell Strain Gauge?", "id": "Sensor Berat Jembatan Wheatstone." },
  { "en": "Kabel Load Cell Biasanya Berapa?", "id": "4 Kabel Atau 6 Kabel." },
  { "en": "Apa Fungsi Kabel Sense Load Cell?", "id": "Kompensasi Rugi Tegangan Kabel." },
  { "en": "Apa Itu Excitation Voltage Sensor?", "id": "Tegangan Suplai Jembatan Sensor." },
  { "en": "Apa Itu Thermocouple Cold Junction?", "id": "Sambungan Referensi Suhu Kamar." },
  { "en": "Apa Itu Cold Junction Compensation?", "id": "Koreksi Suhu Sambungan Terminal." },
  { "en": "Kabel Thermocouple Type J Warna?", "id": "Putih Dan Merah." },
  { "en": "Apa Itu PT100 2 Wire?", "id": "Kurang Akurat Karena Kabel Panjang." },
  { "en": "Apa Itu PT100 3 Wire?", "id": "Kompensasi Panjang Kabel Standar Industri." },
  { "en": "Apa Itu PT100 4 Wire?", "id": "Paling Akurat Laboratorium." },
  { "en": "Apa Itu Ultrasonic Level Transmitter?", "id": "Ukur Level Tangki Tanpa Sentuh." },
  { "en": "Apa Itu Radar Level Transmitter?", "id": "Ukur Level Dengan Gelombang Radio." },
  { "en": "Apa Itu Capacitive Level Sensor?", "id": "Ukur Level Berdasarkan Dielektrik." },
  { "en": "Apa Itu Hydrostatic Level Sensor?", "id": "Ukur Level Berdasarkan Tekanan Air." },
  { "en": "Posisi Pasang Sensor Hydrostatic?", "id": "Tenggelam Di Dasar Tangki." },
  { "en": "Apa Itu Flow Switch Paddle?", "id": "Sakelar Aliran Mekanik Dayung." },
  { "en": "Apa Itu Rotameter Flow?", "id": "Tabung Kaca Bola Mengapung." },
  { "en": "Apa Itu Orifice Plate Flow Meter?", "id": "Ukur Aliran Beda Tekanan Pelat." },
  { "en": "Apa Itu Venturi Tube?", "id": "Pipa Menyempit Ukur Aliran." },
  { "en": "Apa Itu Coriolis Flow Meter?", "id": "Ukur Massa Aliran Sangat Akurat." },
  { "en": "Apa Itu pH Meter Probe?", "id": "Mengukur Keasaman Cairan." },
  { "en": "Elektroda pH Meter Harus Disimpan?", "id": "Dalam Cairan KCL." },
  { "en": "Apa Itu Conductivity Meter?", "id": "Mengukur Daya Hantar Cairan." },
  { "en": "Satuan Konduktivitas Air Adalah?", "id": "Micro Siemens Per cm." },
  { "en": "Apa Itu TDS Meter?", "id": "Total Dissolved Solids." },
  { "en": "Hubungan TDS Dan Konduktivitas?", "id": "TDS Adalah Setengah Konduktivitas." },
  { "en": "Apa Itu Turbidity Meter?", "id": "Mengukur Kekeruhan Air." },
  { "en": "Satuan Kekeruhan Air Adalah?", "id": "NTU." },
  { "en": "Apa Itu DO Meter?", "id": "Dissolved Oxygen Meter." },
  { "en": "Apa Itu Relay Safety Pilz?", "id": "Relay Kuning Standar Keselamatan." },
  { "en": "Apa Fungsi Relay Safety?", "id": "Memastikan Mesin Mati Saat Bahaya." },
  { "en": "Apa Itu Light Curtain Sensor?", "id": "Tirai Sensor Cahaya Pengaman." },
  { "en": "Fungsi Light Curtain Adalah?", "id": "Stop Mesin Jika Tangan Masuk." },
  { "en": "Apa Itu Two Hand Operation?", "id": "Wajib Tekan Dua Tombol Bersamaan." },
  { "en": "Tujuannya Two Hand Operation?", "id": "Menjaga Kedua Tangan Aman." },
  { "en": "Apa Itu Pull Cord Switch?", "id": "Tali Tarik Darurat Conveyor." },
  { "en": "Apa Itu Misalignment Switch Conveyor?", "id": "Deteksi Sabuk Miring Keluar Jalur." },
  { "en": "Apa Itu Speed Switch Conveyor?", "id": "Deteksi Slip Atau Putus Sabuk." },
  { "en": "Apa Itu Chute Switch?", "id": "Deteksi Corong Macet Penuh." },
  { "en": "Apa Itu Solenoid Valve Namur?", "id": "Standar Dudukan Valve Pneumatik." },
  { "en": "Apa Itu Batas Pendekatan Terbatas (Limited Approach Boundary)?", "id": "Jarak Aman Untuk Personil Non Listrik." },
  { "en": "Apa Itu Batas Pendekatan Terlarang (Restricted Approach Boundary)?", "id": "Hanya Teknisi Terlatih Dengan APD." },
  { "en": "Apa Itu AFCI (Arc Fault Circuit Interrupter)?", "id": "Pemutus Sirkuit Deteksi Percikan Api." },
  { "en": "Apa Itu GFCI (Ground Fault Circuit Interrupter)?", "id": "Pemutus Arus Bocor Ke Tanah." },
  { "en": "Apa Bedanya ELCB Dan RCBO?", "id": "RCBO Gabungan MCB Dan ELCB." },
  { "en": "Sensitivitas ELCB 30mA Digunakan Untuk?", "id": "Proteksi Keselamatan Manusia." },
  { "en": "Sensitivitas ELCB 300mA Digunakan Untuk?", "id": "Proteksi Bahaya Kebakaran Gedung." },
  { "en": "Apa Itu Double Insulation (Isolasi Ganda)?", "id": "Alat Listrik Tanpa Kabel Ground." },
  { "en": "Simbol Isolasi Ganda Adalah?", "id": "Dua Kotak Persegi Dalam." },
  { "en": "Apa Itu Kelas Perlindungan Listrik Class 1?", "id": "Wajib Menggunakan Grounding." },
  { "en": "Apa Itu Kelas Perlindungan Listrik Class 2?", "id": "Isolasi Ganda Tanpa Grounding." },
  { "en": "Apa Itu Kelas Perlindungan Listrik Class 3?", "id": "Tegangan Rendah Ekstra Aman." },
  { "en": "Apa Itu Tegangan SELV (Safety Extra Low Voltage)?", "id": "Tegangan Di Bawah 50 Volt AC." },
  { "en": "Apa Itu Tegangan PELV (Protective Extra Low Voltage)?", "id": "SELV Dengan Titik Grounding." },
  { "en": "Apa Impedansi Sinyal Line Level Audio?", "id": "Sekitar 100 Sampai 600 Ohm." },
  { "en": "Apa Itu Balanced Audio Signal?", "id": "Sinyal Tahan Noise Jarak Jauh." },
  { "en": "Apa Itu Unbalanced Audio Signal?", "id": "Rentan Noise Kabel Pendek." },
  { "en": "Kabel RCA Termasuk Jenis Balanced Atau Unbalanced?", "id": "Unbalanced." },
  { "en": "Kabel XLR Termasuk Jenis Balanced Atau Unbalanced?", "id": "Balanced." },
  { "en": "Pin 1 Pada Konektor XLR Adalah?", "id": "Ground Atau Shield." },
  { "en": "Pin 2 Pada Konektor XLR Adalah?", "id": "Hot Atau Positif." },
  { "en": "Pin 3 Pada Konektor XLR Adalah?", "id": "Cold Atau Negatif." },
  { "en": "Apa Itu SpeakON Connector?", "id": "Konektor Speaker Daya Besar." },
  { "en": "Apa Keunggulan SpeakON Dibanding Jack Biasa?", "id": "Terkunci Dan Arus Besar." },
  { "en": "Apa Itu Pink Noise Audio?", "id": "Suara Untuk Kalibrasi Equalizer." },
  { "en": "Apa Itu White Noise Audio?", "id": "Suara Desis Frekuensi Rata." },
  { "en": "Apa Itu Resistor Pull Up Internal Arduino?", "id": "Resistor 20k Ohm Dalam Chip." },
  { "en": "Perintah PinMode Input Pullup Berfungsi?", "id": "Mengaktifkan Resistor Internal." },
  { "en": "Apa Itu Switch Bouncing?", "id": "Sinyal Pantul Saat Tombol Ditekan." },
  { "en": "Apa Itu Teknik Debouncing?", "id": "Menghilangkan Sinyal Pantul Tombol." },
  { "en": "Capacitor Debouncing Termasuk Jenis?", "id": "Debouncing Hardware." },
  { "en": "Delay Program Termasuk Jenis Debouncing?", "id": "Debouncing Software." },
  { "en": "Apa Itu Interupt External Arduino?", "id": "Deteksi Sinyal Pin Prioritas." },
  { "en": "Fungsi AttachInterrupt Digunakan Untuk?", "id": "Menjalankan Fungsi Saat Sinyal Berubah." },
  { "en": "Apa Itu Mode Rising Edge?", "id": "Sinyal Berubah Low Ke High." },
  { "en": "Apa Itu Mode Falling Edge?", "id": "Sinyal Berubah High Ke Low." },
  { "en": "Apa Itu Mode Change Interrupt?", "id": "Setiap Perubahan Logika." },
  { "en": "Baterai LiFePO4 Tegangan Nominalnya?", "id": "3,2 Volt." },
  { "en": "Baterai LiFePO4 Tegangan Penuh?", "id": "3,65 Volt." },
  { "en": "Siklus Hidup LiFePO4 Mencapai?", "id": "2000 Sampai 5000 Siklus." },
  { "en": "Baterai Li-Ion NMC Tegangan Nominalnya?", "id": "3,7 Volt." },
  { "en": "Baterai Li-Ion NMC Tegangan Penuh?", "id": "4,2 Volt." },
  { "en": "Mana Lebih Aman LiFePO4 Atau Li-Ion?", "id": "LiFePO4 Lebih Stabil." },
  { "en": "Apa Itu Baterai LTO (Lithium Titanate)?", "id": "Baterai Charging Sangat Cepat." },
  { "en": "Tegangan Nominal Baterai LTO Adalah?", "id": "2,4 Volt." },
  { "en": "Apa Itu Baterai Primer?", "id": "Sekali Pakai Buang." },
  { "en": "Apa Itu Baterai Sekunder?", "id": "Bisa Diisi Ulang." },
  { "en": "Baterai Alkaline Termasuk Jenis?", "id": "Baterai Primer." },
  { "en": "Baterai Zinc Carbon Termasuk Jenis?", "id": "Baterai Primer." },
  { "en": "Apa Itu Kabel NYM 3x2,5mm?", "id": "Isi 3 Kawat 2,5 mm." },
  { "en": "Kabel NYM 3x2,5mm Biasa Untuk?", "id": "Stopkontak Listrik Rumah." },
  { "en": "Kabel NYM 2x1,5mm Biasa Untuk?", "id": "Lampu Penerangan Rumah." },
  { "en": "Apa Itu T-Dus (Junction Box)?", "id": "Kotak Sambungan Kabel." },
  { "en": "Sambungan Kabel Ekor Babi Disebut?", "id": "Pig Tail Joint." },
  { "en": "Apa Fungsi Lasdop (Wire Nut)?", "id": "Penutup Sambungan Ekor Babi." },
  { "en": "Apa Fungsi Isolasi Listrik (Electrical Tape)?", "id": "Membungkus Sambungan Kabel." },
  { "en": "Warna Isolasi Kabel Standar Grounding?", "id": "Kuning Hijau." },
  { "en": "Apa Itu MCB 1 Pole?", "id": "Memutus Fasa Saja." },
  { "en": "Apa Itu MCB 2 Pole?", "id": "Memutus Fasa Dan Netral." },
  { "en": "Mengapa MCB 2 Pole Lebih Aman?", "id": "Isolasi Total Saat Perbaikan." },
  { "en": "Apa Itu Busbar Sisir (Comb Busbar)?", "id": "Jamper MCB Yang Rapi." },
  { "en": "Apa Itu Box MCB (Group Panel)?", "id": "Rumah MCB Dalam Tembok." },
  { "en": "Tinggi Sakelar Lampu Standar Adalah?", "id": "150 cm Dari Lantai." },
  { "en": "Tinggi Stopkontak Bawah Standar Adalah?", "id": "30 cm Dari Lantai." },
  { "en": "Stopkontak AC Biasanya Dipasang Di?", "id": "Tinggi Dekat Plafon." },
  { "en": "Apa Itu Relay DPDT?", "id": "Double Pole Double Throw." },
  { "en": "Relay DPDT Memiliki Berapa Kaki Kontak?", "id": "6 Kaki Kontak." },
  { "en": "Apa Itu Relay SPDT?", "id": "Single Pole Double Throw." },
  { "en": "Relay SPDT Memiliki Berapa Kaki Kontak?", "id": "3 Kaki Kontak." },
  { "en": "Kaki COM Pada Relay Artinya?", "id": "Common Atau Pusat." },
  { "en": "Apa Itu Flyback Diode Pada Relay?", "id": "Pengaman Arus Balik Koil." },
  { "en": "Tanpa Dioda Flyback Transistor Driver Akan?", "id": "Rusak Terkena Tegangan Tinggi." },
  { "en": "Apa Itu Motor DC Brushless Outrunner?", "id": "Bagian Luar Yang Berputar." },
  { "en": "Motor Outrunner Memiliki Torsi?", "id": "Lebih Besar." },
  { "en": "Apa Itu Motor DC Brushless Inrunner?", "id": "Bagian Dalam Yang Berputar." },
  { "en": "Motor Inrunner Memiliki RPM?", "id": "Lebih Tinggi." },
  { "en": "Apa Itu Gearbox Planetary?", "id": "Gearbox Ringkas Torsi Besar." },
  { "en": "Apa Itu Worm Gear Motor?", "id": "Gearbox Cacing Mengunci Sendiri." },
  { "en": "Sifat Worm Gear Adalah?", "id": "Output Tidak Bisa Memutar Input." },
  { "en": "Aplikasi Worm Gear Adalah?", "id": "Lift Atau Wiper Mobil." },
  { "en": "Apa Itu Kabel Ribbon (Pelangi)?", "id": "Kabel Pipih Banyak Jalur." },
  { "en": "Kabel Ribbon Digunakan Untuk?", "id": "Data Komputer Atau Elektronik." },
  { "en": "Konektor Kabel Ribbon Adalah?", "id": "IDC Socket." },
  { "en": "Apa Itu Kabel AWG 24?", "id": "Kabel Tipis Untuk Elektronik." },
  { "en": "Apa Itu Kabel AWG 0?", "id": "Kabel Sangat Besar." },
  { "en": "Semakin Besar Angka AWG Kabel Semakin?", "id": "Kecil." },
  { "en": "Apa Itu Ferrite Bead Pada Kabel?", "id": "Silinder Magnet Peredam Noise." },
  { "en": "Ferrite Bead Efektif Meredam Frekuensi?", "id": "Frekuensi Tinggi." },
  { "en": "Apa Itu Heat Sink Active?", "id": "Heatsink Dengan Kipas." },
  { "en": "Apa Itu Heat Sink Passive?", "id": "Heatsink Tanpa Kipas." },
  { "en": "Apa Itu Thermal Paste Warna Abu?", "id": "Mengandung Serbuk Perak Aluminium." },
  { "en": "Apa Itu Thermal Paste Warna Putih?", "id": "Silikon Keramik Biasa." },
  { "en": "Thermal Pad Berwarna Biru Biasanya?", "id": "Silikon Pad Standar." },
  { "en": "Apa Itu Soldering Flux Paste?", "id": "Pasta Pembersih Kaki Komponen." },
  { "en": "Apa Itu Tip Tinner?", "id": "Pembersih Mata Solder Hitam." },
  { "en": "Mengapa Mata Solder Menjadi Hitam?", "id": "Oksidasi Karena Panas Berlebih." },
  { "en": "Cara Merawat Mata Solder Adalah?", "id": "Beri Timah Sebelum Dimatikan." },
  { "en": "Apa Itu Helping Hand Solder?", "id": "Penjepit PCB Kaca Pembesar." },
  { "en": "Apa Itu Fume Extractor?", "id": "Kipas Penyedot Asap Solder." },
  { "en": "Apa Itu VDR (Voltage Dependent Resistor)?", "id": "Nama Lain Varistor." },
  { "en": "Apa Fungsi Optocoupler 4N35?", "id": "Isolasi Sinyal Digital Standar." },
  { "en": "Apa Fungsi Optocoupler MOC3021?", "id": "Pemicu Triac Isolasi Optik." },
  { "en": "Apa Itu Dark Current Photodiode?", "id": "Arus Bocor Saat Gelap." },
  { "en": "Apa Itu Saturation Current Transistor?", "id": "Arus Maksimal Saat Sakelar On." },
  { "en": "Apa Itu Cut-in Voltage Dioda Silikon?", "id": "0,7 Volt." },
  { "en": "Apa Itu Cut-in Voltage Dioda Germanium?", "id": "0,3 Volt." },
  { "en": "Apa Itu Knee Voltage Zener?", "id": "Tegangan Mulai Stabil Zener." },
  { "en": "Apa Itu Peak Inverse Voltage (PIV)?", "id": "Tegangan Balik Maksimal Dioda." },
  { "en": "Apa Itu Loading Effect Voltmeter?", "id": "Beban Alat Ukur Mengubah Tegangan." },
  { "en": "Voltmeter Ideal Memiliki Hambatan Dalam?", "id": "Tak Terhingga." },
  { "en": "Amperemeter Ideal Memiliki Hambatan Dalam?", "id": "0 Ohm." },
  { "en": "Apa Itu Galvanometer D'Arsonval?", "id": "Alat Ukur Kumparan Putar." },
  { "en": "Apa Fungsi Shunt Resistor Amperemeter?", "id": "Memperbesar Batas Ukur Arus." },
  { "en": "Apa Fungsi Multiplier Resistor Voltmeter?", "id": "Memperbesar Batas Ukur Tegangan." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Pengukur Hambatan Presisi." },
  { "en": "Apa Itu Jembatan Kelvin?", "id": "Pengukur Hambatan Sangat Rendah." },
  { "en": "Apa Itu Jembatan Maxwell?", "id": "Pengukur Induktansi Kumparan." },
  { "en": "Apa Itu Jembatan Schering?", "id": "Pengukur Kapasitansi Kapasitor." },
  { "en": "Apa Itu Megger Insulation Tester?", "id": "Pengukur Tahanan Isolasi Tinggi." },
  { "en": "Berapa Tegangan Uji Megger Kabel 220V?", "id": "500 Volt." },
  { "en": "Apa Itu LCR Meter?", "id": "Ukur Induktansi Kapasitansi Resistansi." },
  { "en": "Apa Itu Q Factor Induktor?", "id": "Kualitas Efisiensi Kumparan." },
  { "en": "Semakin Tinggi Q Factor Maka?", "id": "Rugi Rugi Semakin Kecil." },
  { "en": "Apa Itu Dissipation Factor Kapasitor?", "id": "Faktor Kerugian Dielektrik." },
  { "en": "Apa Itu ESR Low Impedance?", "id": "Hambatan Seri Sangat Kecil." },
  { "en": "Kapasitor Low ESR Cocok Untuk?", "id": "Power Supply Switching." },
  { "en": "Apa Itu Varistor MOV?", "id": "Resistor Berubah Sesuai Tegangan." },
  { "en": "MOV Digunakan Untuk Proteksi Apa?", "id": "Lonjakan Tegangan Petir." },
  { "en": "Apa Itu NTC Inrush Current Limiter?", "id": "Pembatas Arus Start Awal." },
  { "en": "Saat Panas Resistansi NTC Akan?", "id": "Turun." },
  { "en": "Saat Panas Resistansi PTC Akan?", "id": "Naik." },
  { "en": "PTC Fuse Sering Disebut Apa?", "id": "Polyfuse Atau Resettable Fuse." },
  { "en": "Apa Itu Gas Discharge Tube (GDT)?", "id": "Tabung Gas Anti Petir." },
  { "en": "GDT Bekerja Dengan Cara?", "id": "Mengalirkan Arus Saat Breakdown." },
  { "en": "Apa Itu Transient Voltage Suppressor (TVS)?", "id": "Dioda Pemotong Tegangan Cepat." },
  { "en": "Kecepatan Respon TVS Diode Adalah?", "id": "Sangat Cepat Nanodetik." },
  { "en": "Apa Itu Thermal Cutoff (TCO)?", "id": "Sekering Suhu Sekali Pakai." },
  { "en": "TCO Sering Ditemukan Di Mana?", "id": "Magic Com Dan Setrika." },
  { "en": "Apa Itu Bimetal Switch?", "id": "Sakelar Suhu Mekanik." },
  { "en": "Bimetal Bekerja Berdasarkan?", "id": "Pemuaian Dua Logam Berbeda." },
  { "en": "Apa Itu Reed Switch Magnet?", "id": "Sakelar Kaca Aktif Magnet." },
  { "en": "Reed Switch Biasa Dipakai Untuk?", "id": "Sensor Pintu Alarm." },
  { "en": "Apa Itu Hall Effect Sensor?", "id": "Sensor Magnet Semikonduktor." },
  { "en": "Sensor Hall Effect Analog Outputnya?", "id": "Tegangan Sebanding Medan Magnet." },
  { "en": "Sensor Hall Effect Digital Outputnya?", "id": "Logika 0 Atau 1." },
  { "en": "Apa Itu Proximity Inductive?", "id": "Deteksi Logam Tanpa Sentuh." },
  { "en": "Apa Itu Proximity Capacitive?", "id": "Deteksi Benda Padat Cair." },
  { "en": "Apa Itu Photoelectric Sensor?", "id": "Sensor Deteksi Objek Cahaya." },
  { "en": "Apa Itu Through Beam Sensor?", "id": "Pemancar Penerima Terpisah." },
  { "en": "Apa Itu Retro Reflective Sensor?", "id": "Pemancar Penerima Satu Sisi." },
  { "en": "Apa Itu Diffuse Reflective Sensor?", "id": "Pantulan Cahaya Dari Objek." },
  { "en": "Sensor Fiber Optik Digunakan Untuk?", "id": "Deteksi Benda Kecil Presisi." },
  { "en": "Apa Itu Rotary Encoder Incremental?", "id": "Menghitung Pulsa Putaran." },
  { "en": "Apa Itu Rotary Encoder Absolute?", "id": "Mengetahui Posisi Sudut Pasti." },
  { "en": "Sinyal Encoder Phase A Dan B Beda?", "id": "90 Derajat." },
  { "en": "Apa Itu Gray Code Encoder?", "id": "Kode Biner Satu Bit Berubah." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Regangan Logam." },
  { "en": "Perubahan Resistansi Strain Gauge Karena?", "id": "Perubahan Panjang Dan Luas." },
  { "en": "Load Cell Terdiri Dari Apa?", "id": "4 Strain Gauge Jembatan." },
  { "en": "Apa Itu LVDT Sensor?", "id": "Sensor Posisi Linier Induktif." },
  { "en": "Keunggulan LVDT Adalah?", "id": "Resolusi Tinggi Tahan Lama." },
  { "en": "Apa Itu Thermocouple Type K?", "id": "Chromel Alumel." },
  { "en": "Rentang Suhu Thermocouple Type K?", "id": "-200 Sampai 1250 Celcius." },
  { "en": "Apa Itu Thermocouple Type J?", "id": "Iron Constantan." },
  { "en": "Apa Itu RTD PT100?", "id": "Platinum 100 Ohm." },
  { "en": "Koefisien Suhu PT100 Adalah?", "id": "0,385 Ohm Per Derajat." },
  { "en": "Apa Itu Thermistor NTC 10K?", "id": "10 Kiloohm Suhu Ruang." },
  { "en": "Apa Itu Beta Value Thermistor?", "id": "Konstanta Sensitivitas Suhu." },
  { "en": "Apa Itu Pyrometer Inframerah?", "id": "Termometer Tanpa Sentuh." },
  { "en": "Apa Itu Emissivity 0.95?", "id": "Standar Benda Non Logam." },
  { "en": "Apa Itu Flow Meter Turbine?", "id": "Baling Baling Putar Dalam Pipa." },
  { "en": "Apa Itu Flow Meter Electromagnetic?", "id": "Hukum Faraday Cairan Konduktif." },
  { "en": "Apa Itu Flow Meter Ultrasonic Transit?", "id": "Beda Waktu Rambat Suara." },
  { "en": "Apa Itu Flow Meter Vortex?", "id": "Pusaran Aliran Di Belakang Benda." },
  { "en": "Apa Itu Level Sensor Radar?", "id": "Gelombang Mikro Pantul Permukaan." },
  { "en": "Apa Itu Level Sensor Ultrasonic?", "id": "Gelombang Suara Pantul Permukaan." },
  { "en": "Apa Itu Level Sensor Hydrostatic?", "id": "Tekanan Air Dasar Tangki." },
  { "en": "Apa Itu Level Switch Float?", "id": "Sakelar Pelampung Bola." },
  { "en": "Apa Itu Level Switch Electrode?", "id": "Konduktivitas Air Antar Batang." },
  { "en": "Apa Itu pH Meter?", "id": "Ukur Derajat Keasaman." },
  { "en": "Apa Itu Buffer Solution pH?", "id": "Cairan Kalibrasi pH Meter." },
  { "en": "Apa Itu Conductivity TDS Meter?", "id": "Ukur Padatan Terlarut Air." },
  { "en": "Satuan TDS Adalah?", "id": "PPM Part Per Million." },
  { "en": "Apa Itu Turbidity Sensor?", "id": "Ukur Kekeruhan Air." },
  { "en": "Apa Itu DO Meter?", "id": "Dissolved Oxygen Oksigen Terlarut." },
  { "en": "Apa Itu Gas Detector LEL?", "id": "Lower Explosive Limit." },
  { "en": "Gas Detector CO Mendeteksi?", "id": "Karbon Monoksida Beracun." },
  { "en": "Gas Detector H2S Mendeteksi?", "id": "Hidrogen Sulfida Bau Telur." },
  { "en": "Apa Itu Solenoid Valve NC?", "id": "Normally Closed Tertutup Normal." },
  { "en": "Apa Itu Solenoid Valve NO?", "id": "Normally Open Terbuka Normal." },
  { "en": "Apa Itu Motorized Valve?", "id": "Kran Digerakkan Motor Listrik." },
  { "en": "Waktu Buka Motorized Valve?", "id": "Lambat Beberapa Detik." },
  { "en": "Waktu Buka Solenoid Valve?", "id": "Cepat Instan." },
  { "en": "Apa Itu Pneumatic Actuator?", "id": "Penggerak Valve Tenaga Angin." },
  { "en": "Apa Itu I/P Positioner?", "id": "Sinyal Arus Ke Tekanan." },
  { "en": "Sinyal Standar Instrumentasi Arus?", "id": "4 Sampai 20 mA." },
  { "en": "Sinyal Standar Instrumentasi Tegangan?", "id": "0 Sampai 10 Volt." },
  { "en": "Apa Itu Hart Protocol?", "id": "Data Digital Di Kabel Analog." },
  { "en": "Apa Itu Fieldbus Foundation?", "id": "Protokol Digital Full Bus." },
  { "en": "Apa Itu Profibus PA?", "id": "Process Automation Fieldbus." },
  { "en": "Apa Itu Nyquist Rate?", "id": "Minimal 2 Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing Pada Sinyal?", "id": "Tumpang Tindih Frekuensi Sampling Rendah." },
  { "en": "Apa Itu Kuantisasi ADC?", "id": "Pembulatan Nilai Analog Ke Digital." },
  { "en": "Apa Itu Resolution Bit ADC?", "id": "Ketelitian Konversi Data Analog." },
  { "en": "Apa Itu IGBT Tail Current?", "id": "Arus Sisa Saat Mematikan." },
  { "en": "Apa Itu Reverse Blocking Thyristor?", "id": "Menahan Tegangan Balik Tinggi." },
  { "en": "Apa Itu Quadrant 1 TRIAC?", "id": "Gate Positif MT2 Positif." },
  { "en": "Apa Itu Zero Voltage Switching?", "id": "Sakelar Hidup Saat Tegangan 0." },
  { "en": "Apa Itu Zero Current Switching?", "id": "Sakelar Mati Saat Arus 0." },
  { "en": "Apa Itu Hard Switching?", "id": "Sakelar Paksa Timbul Rugi Daya." },
  { "en": "Apa Itu Soft Switching?", "id": "Sakelar Halus Minim Rugi Daya." },
  { "en": "Generator Kutub Salient Digunakan Di?", "id": "PLTA Putaran Rendah." },
  { "en": "Generator Kutub Silinder Digunakan Di?", "id": "PLTU Putaran Tinggi." },
  { "en": "Apa Itu Armature Reaction Generator?", "id": "Distorsi Medan Magnet Utama." },
  { "en": "Apa Itu Synchronous Reactance?", "id": "Reaktansi Internal Generator Sinkron." },
  { "en": "Apa Itu Short Circuit Ratio Generator?", "id": "Ukuran Stabilitas Generator." },
  { "en": "Apa Itu Creepage Distance Isolator?", "id": "Jarak Rambat Permukaan Isolator." },
  { "en": "Apa Itu Strike Distance Isolator?", "id": "Jarak Loncat Api Udara." },
  { "en": "Apa Itu Flashover Isolator?", "id": "Loncat Api Lewat Permukaan." },
  { "en": "Apa Itu Puncture Isolator?", "id": "Tembus Isolasi Bagian Dalam." },
  { "en": "Apa Itu Corona Discharge?", "id": "Pendaran Cahaya Di Kabel HV." },
  { "en": "Apa Itu Grading Ring?", "id": "Cincin Pemerata Medan Listrik." },
  { "en": "Apa Itu PLC Scan Time?", "id": "Waktu 1 Siklus Program." },
  { "en": "Apa Itu Forcing Input PLC?", "id": "Memaksa Nilai Input Software." },
  { "en": "Apa Itu Retentive Timer PLC?", "id": "Menyimpan Waktu Saat Mati." },
  { "en": "Apa Itu Pulse Output PLC?", "id": "Sinyal Kotak Kecepatan Tinggi." },
  { "en": "Apa Itu High Speed Counter PLC?", "id": "Hitung Pulsa Cepat Encoder." },
  { "en": "Apa Itu Sink Input PLC?", "id": "Input Aktif Low Ground." },
  { "en": "Apa Itu Source Input PLC?", "id": "Input Aktif High Positif." },
  { "en": "Apa Itu Relay Interposing?", "id": "Relay Perantara PLC Dan Beban." },
  { "en": "Apa Itu Cable Gland Armoured?", "id": "Gland Penjepit Kawat Baja." },
  { "en": "Apa Itu Cable Cleat?", "id": "Klem Kabel Short Circuit." },
  { "en": "Apa Itu Trefoil Formation Kabel?", "id": "Susunan Kabel Segitiga Rapat." },
  { "en": "Apa Itu Flat Formation Kabel?", "id": "Susunan Kabel Berjejer Datar." },
  { "en": "Apa Kelebihan Trefoil Formation?", "id": "Induktansi Seimbang Tiap Fasa." },
  { "en": "Apa Kelebihan Flat Formation?", "id": "Pendinginan Lebih Baik." },
  { "en": "Apa Itu Cross Bonding Kabel?", "id": "Menghilangkan Arus Sirkulasi Shield." },
  { "en": "Apa Itu Link Box Kabel?", "id": "Kotak Sambungan Grounding Shield." },
  { "en": "Apa Itu Partial Discharge Test Kabel?", "id": "Deteksi Cacat Isolasi Dini." },
  { "en": "Apa Itu VLF Test Kabel?", "id": "Very Low Frequency 0,1 Hz." },
  { "en": "Mengapa Uji VLF Digunakan?", "id": "Trafo Uji Lebih Kecil Ringan." },
  { "en": "Apa Itu Tan Delta Kabel?", "id": "Mengukur Penuaan Isolasi Kabel." },
  { "en": "Apa Itu Water Treeing Kabel?", "id": "Kerusakan Isolasi Akibat Air." },
  { "en": "Apa Itu Electrical Treeing?", "id": "Jalur Karbon Akibat PD." },
  { "en": "Apa Itu Sheath Voltage Limiter?", "id": "Proteksi Tegangan Lebih Shield." },
  { "en": "Apa Itu Transposition Tower?", "id": "Menara Tukar Posisi Fasa." },
  { "en": "Apa Itu Sagging Line?", "id": "Mengatur Kekenduran Kabel Transmisi." },
  { "en": "Apa Itu Galloping Conductor?", "id": "Getaran Kabel Amplitudo Besar." },
  { "en": "Apa Itu Stockbridge Damper?", "id": "Peredam Getaran Kabel Transmisi." },
  { "en": "Apa Itu Aircraft Warning Ball?", "id": "Bola Oranye Penanda Kabel." },
  { "en": "Apa Itu OPGW Fiber?", "id": "Optical Ground Wire." },
  { "en": "Fungsi OPGW Adalah?", "id": "Grounding Dan Komunikasi Data." },
  { "en": "Apa Itu ADSS Fiber Cable?", "id": "All Dielectric Self Supporting." },
  { "en": "Kabel ADSS Dipasang Di Mana?", "id": "Di Bawah Kabel Fase SUTM." },
  { "en": "Apa Itu Splice Box OPGW?", "id": "Kotak Sambungan Fiber Menara." },
  { "en": "Apa Itu Patch Cord Fiber?", "id": "Kabel Jumper Optik Kuning." },
  { "en": "Apa Itu Pigtail Fiber Optik?", "id": "Ujung Kabel Serabut Optik." },
  { "en": "Konektor SC Berbentuk?", "id": "Kotak Biru Push Pull." },
  { "en": "Konektor LC Berbentuk?", "id": "Kotak Kecil Mirip RJ45." },
  { "en": "Konektor FC Berbentuk?", "id": "Bulat Ulir Logam." },
  { "en": "Konektor ST Berbentuk?", "id": "Bulat Bayonet Putar." },
  { "en": "Apa Itu UPC Polish?", "id": "Ujung Konektor Datar Biru." },
  { "en": "Apa Itu APC Polish?", "id": "Ujung Konektor Miring Hijau." },
  { "en": "Kelebihan APC Polish Adalah?", "id": "Pantulan Balik Sangat Rendah." },
  { "en": "Apa Itu Fusion Splicer?", "id": "Alat Sambung Fiber Optik." },
  { "en": "Apa Itu Cleaver Fiber Optik?", "id": "Pemotong Kaca Fiber Presisi." },
  { "en": "Apa Itu Stripper Fiber Optik?", "id": "Pengupas Coating Serat Kaca." },
  { "en": "Apa Itu Visual Fault Locator?", "id": "Senter Laser Merah Cek Putus." },
  { "en": "Apa Itu Optical Power Meter?", "id": "Alat Ukur Kekuatan Cahaya." },
  { "en": "Apa Itu Light Source Fiber?", "id": "Sumber Cahaya Laser Stabil." },
  { "en": "Apa Itu Media Converter Fiber?", "id": "Ubah Optik Ke Ethernet LAN." },
  { "en": "Apa Itu SFP Module?", "id": "Modul Transceiver Fiber Switch." },
  { "en": "Jarak Jangkau SFP Single Mode?", "id": "Bisa Sampai 80 km." },
  { "en": "Jarak Jangkau SFP Multi Mode?", "id": "Hanya Sekitar 550 Meter." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Banyak Sinyal 1 Kabel." },
  { "en": "Apa Itu CWDM?", "id": "Coarse Wavelength Division Multiplexing." },
  { "en": "Apa Itu DWDM?", "id": "Dense Wavelength Division Multiplexing." },
  { "en": "Apa Itu Attenuator Fiber Optik?", "id": "Peredam Sinyal Terlalu Kuat." },
  { "en": "Apa Itu Fiber Optic Closure?", "id": "Rumah Sambungan Kabel Tanah." },
  { "en": "Apa Itu Slack Kabel Fiber?", "id": "Sisa Gulungan Kabel Tiang." },
  { "en": "Apa Itu Microduct Fiber?", "id": "Pipa Kecil Jalur Fiber." },
  { "en": "Apa Itu Air Blown Fiber?", "id": "Tiup Kabel Pakai Angin." },
  { "en": "Apa Itu Load Break Switch (LBS)?", "id": "Sakelar Pemutus Berbeban." },
  { "en": "Apa Itu Disconnecting Switch (DS)?", "id": "Sakelar Pemisah Tanpa Beban." },
  { "en": "Apa Itu Earthing Switch?", "id": "Sakelar Pembumian Keamanan." },
  { "en": "Apa Itu Interlock DS Dan ES?", "id": "DS Tak Bisa Masuk Jika ES Masuk." },
  { "en": "Apa Itu Gas SF6 Pressure Gauge?", "id": "Manometer Tekanan Gas Isolasi." },
  { "en": "Apa Itu Partial Discharge Monitor?", "id": "Alat Pantau PD Online." },
  { "en": "Apa Itu Thermography Inspection?", "id": "Inspeksi Panas Tanpa Sentuh." },
  { "en": "Apa Itu Ultrasound Inspection?", "id": "Deteksi Suara Bocor Listrik." },
  { "en": "Apa Itu Transient Earth Voltage (TEV)?", "id": "Deteksi PD Pada Permukaan Panel." },
  { "en": "Apa Itu Transistor UJT?", "id": "Uni Junction Transistor." },
  { "en": "UJT Sering Digunakan Sebagai Apa?", "id": "Pembangkit Pulsa Gigi Gergaji." },
  { "en": "Apa Itu Transistor PUT?", "id": "Programmable Uni Junction Transistor." },
  { "en": "Apa Fungsi Snubber Pada TRIAC?", "id": "Mencegah Trigger Palsu." },
  { "en": "Berapa Tegangan Tembus DIAC Standar?", "id": "Sekitar 30 Volt." },
  { "en": "Apa Itu Motor Linear?", "id": "Motor Gerak Lurus Tanpa Roda." },
  { "en": "Kereta Maglev Menggunakan Prinsip Apa?", "id": "Levitasi Magnetik Superkonduktor." },
  { "en": "Apa Kelebihan Semikonduktor Silicon Carbide (SiC)?", "id": "Tahan Tegangan Dan Suhu Tinggi." },
  { "en": "Charger GaN (Gallium Nitride) Lebih Bagus Karena?", "id": "Ukuran Kecil Efisiensi Tinggi." },
  { "en": "Apa Itu Traceability Kalibrasi Alat?", "id": "Ketelusuran Ke Standar Internasional." },
  { "en": "Berapa Rasio TUR Kalibrasi Ideal?", "id": "4 Banding 1." },
  { "en": "Apa Itu Metode Bola Gelinding (Rolling Sphere)?", "id": "Desain Proteksi Petir Gedung." },
  { "en": "Bahaya Tegangan Langkah Dikurangi Dengan?", "id": "Grid Grounding Rapat." },
  { "en": "Bahaya Tegangan Sentuh Dikurangi Dengan?", "id": "Bonding Equipotential." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Cerdas Dua Arah." },
  { "en": "Apa Itu AMI Pada PLN?", "id": "Meteran Pintar Komunikasi Dua Arah." },
  { "en": "Apa Itu Air Gap Jaringan?", "id": "Isolasi Fisik Jaringan Aman." },
  { "en": "Apa Fungsi Firewall Industri?", "id": "Menyaring Paket Data Jaringan OT." },
  { "en": "Port Berapa Modbus TCP Menggunakan?", "id": "Port 502." },
  { "en": "Port Berapa MQTT Broker Standar?", "id": "Port 1883." },
  { "en": "Port Berapa MQTT Secure SSL?", "id": "Port 8883." },
  { "en": "Discharge 2C Pada Baterai Artinya?", "id": "Habis Dalam 30 Menit." },
  { "en": "Discharge 0.5C Pada Baterai Artinya?", "id": "Habis Dalam 2 Jam." },
  { "en": "Apa Itu Cycle Life Baterai?", "id": "Jumlah Cas Habis Sampai Rusak." },
  { "en": "DoD 100 Persen Baterai Artinya?", "id": "Baterai Dikuras Sampai Habis." },
  { "en": "Apa Kelebihan UPS Double Conversion?", "id": "Output Stabil Tanpa Jeda." },
  { "en": "Fungsi Static Bypass Pada UPS?", "id": "Pindah Ke PLN Saat Rusak." },
  { "en": "Fungsi Maintenance Bypass Switch UPS?", "id": "Bypass Manual Saat Perbaikan." },
  { "en": "Apa Itu K Factor Trafo?", "id": "Rating Tahan Panas Harmonisa." },
  { "en": "Apa Fungsi Trafo Zigzag?", "id": "Membuat Titik Netral Buatan." },
  { "en": "Apa Fungsi Hubungan Scott T?", "id": "Ubah 3 Fasa Jadi 2 Fasa." },
  { "en": "Apa Itu Hubungan Open Delta?", "id": "3 Fasa Menggunakan 2 Trafo." },
  { "en": "Kapan Efisiensi Trafo Maksimal?", "id": "Rugi Tembaga Sama Dengan Inti." },
  { "en": "Berapa Kali Recloser Mencoba Menutup?", "id": "Biasanya 3 Sampai 4 Kali." },
  { "en": "Apa Bedanya Recloser Dan Sectionalizer?", "id": "Sectionalizer Tidak Memutus Arus Gangguan." },
  { "en": "Apa Fungsi FCO (Fuse Cut Out)?", "id": "Sekering Tegangan Menengah Trafo." },
  { "en": "Di Mana Arrester Harus Dipasang?", "id": "Sedekat Mungkin Dengan Trafo." },
  { "en": "Apa Itu Travers Atau Cross Arm?", "id": "Besi Palang Dudukan Isolator." },
  { "en": "Apa Fungsi Strain Clamp?", "id": "Menjepit Kabel Tarikan Ujung." },
  { "en": "Apa Fungsi Suspension Clamp?", "id": "Menggantung Kabel Lurus." },
  { "en": "Apa Fungsi Bird Guard Di Tiang?", "id": "Mencegah Burung Hinggap Di Isolator." },
  { "en": "Apa Itu Kerapatan Arus J?", "id": "Arus Per Satuan Luas Penampang." },
  { "en": "Apa Itu Skin Depth?", "id": "Kedalaman Arus Pada Konduktor." },
  { "en": "Tegangan Satu Sel Superkapasitor Adalah?", "id": "Sekitar 2,7 Volt." },
  { "en": "Mengapa Superkapasitor Seri Perlu Balancing?", "id": "Tegangan Tiap Sel Tidak Sama." },
  { "en": "ESR Superkapasitor Biasanya Bernilai?", "id": "Sangat Kecil Mili Ohm." },
  { "en": "Apa Kelemahan Utama Superkapasitor?", "id": "Arus Bocor Self Discharge Tinggi." },
  { "en": "Frekuensi Resonansi Piezo Buzzer Adalah?", "id": "Sekitar 2 kHz Sampai 4 kHz." },
  { "en": "Microphone Electret Butuh Tegangan Bias?", "id": "Ya Butuh Tegangan DC." },
  { "en": "Microphone Dinamis Butuh Tegangan Bias?", "id": "Tidak Butuh Tegangan." },
  { "en": "Berapa Tegangan Phantom Power Standar?", "id": "48 Volt DC." },
  { "en": "Apa Itu Sensitivitas Speaker?", "id": "Keras Suara Per 1 Watt." },
  { "en": "Satuan Kekerasan Suara Adalah?", "id": "dB SPL." },
  { "en": "Apa Fungsi Speaker Line Array?", "id": "Jangkauan Suara Jauh Merata." },
  { "en": "Apa Itu Crossover Aktif?", "id": "Membagi Frekuensi Sebelum Amplifier." },
  { "en": "Apa Itu Crossover Pasif?", "id": "Membagi Frekuensi Setelah Amplifier." },
  { "en": "Apa Itu Teknik Bi Amping?", "id": "Dua Amplifier Untuk Satu Speaker." },
  { "en": "Apa Itu Mode Bridge Amplifier?", "id": "Gabung 2 Channel Jadi 1 Mono." },
  { "en": "Damping Factor Tinggi Bagus Untuk?", "id": "Mengontrol Gerakan Woofer Bass." },
  { "en": "Slew Rate Rendah Menyebabkan?", "id": "Suara Treble Cacat Tumpul." },
  { "en": "Apa Tanda Suara Clipping?", "id": "Suara Pecah Ujung Gelombang Kotak." },
  { "en": "Suara Dengung 50Hz Disebabkan Oleh?", "id": "Ground Loop Atau Kabel Buruk." },
  { "en": "Fungsi Ferrite Bead Pada Kabel HDMI?", "id": "Meredam Radiasi Frekuensi Tinggi." },
  { "en": "Impedansi Pasangan Kabel HDMI Adalah?", "id": "100 Ohm Differensial." },
  { "en": "Sinyal USB Menggunakan Teknik Apa?", "id": "Pasangan Diferensial D Plus D Minus." },
  { "en": "Nilai Resistor Terminasi RS485 Adalah?", "id": "120 Ohm." },
  { "en": "Fungsi Bias Resistor RS485?", "id": "Menjaga Bus Stabil Saat Idle." },
  { "en": "Jika Baud Rate Beda Maka?", "id": "Data Menjadi Sampah Karakter Aneh." },
  { "en": "Apa Itu Parity Error?", "id": "Jumlah Bit Ganjil Genap Salah." },
  { "en": "Apa Itu Flow Control XON/XOFF?", "id": "Menggunakan Kode Software." },
  { "en": "Apa Itu Flow Control RTS/CTS?", "id": "Menggunakan Kabel Hardware." },
  { "en": "Kabel Serial Null Modem Adalah?", "id": "Kabel Silang TX Ke RX." },
  { "en": "Apa Fungsi Loopback Test?", "id": "Menguji Port Komunikasi Sendiri." },
  { "en": "Standar Isolasi Motor 1kV Adalah?", "id": "Minimal 1 Mega Ohm." },
  { "en": "Arrester Kelas B Untuk Apa?", "id": "Proteksi Sambaran Petir Langsung." },
  { "en": "Arrester Kelas C Untuk Apa?", "id": "Proteksi Induksi Petir Panel." },
  { "en": "Arrester Kelas D Untuk Apa?", "id": "Proteksi Peralatan Elektronik Sensitif." },
  { "en": "Apa Itu Joule Rating Varistor?", "id": "Energi Maksimal Yang Bisa Diserap." },
  { "en": "Apa Itu Clamping Voltage TVS?", "id": "Tegangan Saat Proteksi Aktif." },
  { "en": "Apa Itu Breakdown Voltage TVS?", "id": "Tegangan Mulai Mengalirkan Arus." },
  { "en": "Apa Kelebihan Pemanas PTC?", "id": "Mengatur Suhu Sendiri Self Regulating." },
  { "en": "Apa Itu Histeresis Thermostat?", "id": "Selisih Suhu On Dan Off." },
  { "en": "Fungsi Autotune Pada Temperature Controller?", "id": "Mencari Nilai PID Otomatis." },
  { "en": "SSR Zero Cross Bagus Untuk?", "id": "Beban Resistif Pemanas." },
  { "en": "SSR Random Cross Bagus Untuk?", "id": "Beban Induktif Dan Phase Control." },
  { "en": "Apa Itu Snubberless Triac?", "id": "Triac Tahan dv/dt Tinggi." },
  { "en": "Apa Kelemahan Triac 4 Kuadran?", "id": "Sensitivitas Gate Rendah Kuadran 4." },
  { "en": "Tegangan Pemicu DIAC DB3 Adalah?", "id": "Sekitar 32 Volt." },
  { "en": "Apa Bedanya PUT Dan UJT?", "id": "Tegangan Pemicu PUT Bisa Diatur." },
  { "en": "Fungsi Resistor Gate IGBT?", "id": "Mengatur Kecepatan Switching." },
  { "en": "Efek Kapasitansi Gate Besar Adalah?", "id": "Butuh Arus Driver Besar." },
  { "en": "Apa Itu Miller Plateau?", "id": "Tegangan Gate Datar Saat Switching." },
  { "en": "Proteksi Desat Pada IGBT Adalah?", "id": "Deteksi Hubung Singkat Saat On." },
  { "en": "Fungsi Active Miller Clamp?", "id": "Mencegah Gate Nyala Sendiri." },
  { "en": "Apa Fungsi Trafo Pulsa?", "id": "Isolasi Sinyal Gate Driver." },
  { "en": "Apa Kelebihan Opto Gate Driver?", "id": "Isolasi Galvanik Tinggi." },
  { "en": "Apa Itu Fan Out Gerbang Logika?", "id": "Jumlah Beban Gerbang Maksimal." },
  { "en": "Apa Itu Propagation Delay?", "id": "Waktu Tunda Sinyal Input Output." },
  { "en": "Apa Itu Setup Time Flip Flop?", "id": "Waktu Data Stabil Sebelum Clock." },
  { "en": "Apa Itu Hold Time Flip Flop?", "id": "Waktu Data Tahan Setelah Clock." },
  { "en": "Apa Itu Metastability Digital?", "id": "Output Tidak Stabil Antara 0-1." },
  { "en": "Berapa Tegangan Logika High TTL?", "id": "2 Volt Sampai 5 Volt." },
  { "en": "Berapa Tegangan Logika Low TTL?", "id": "0 Volt Sampai 0,8 Volt." },
  { "en": "Apa Itu CMOS Logic Level?", "id": "Logika Berbasis Tegangan Suplai." },
  { "en": "Apa Kelebihan CMOS Dibanding TTL?", "id": "Konsumsi Daya Lebih Rendah." },
  { "en": "Apa Itu Open Drain Output?", "id": "Perlu Resistor Pull Up Eksternal." },
  { "en": "Apa Itu Shift Register?", "id": "Geser Bit Data Serial Paralel." },
  { "en": "Apa Fungsi Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Apa Fungsi Demultiplexer (DEMUX)?", "id": "Membagi Satu Input Ke Banyak." },
  { "en": "Apa Itu Priority Encoder?", "id": "Mementingkan Input Tertinggi." },
  { "en": "Apa Itu BCD (Binary Coded Decimal)?", "id": "4 Bit Untuk 1 Angka." },
  { "en": "Apa Itu Hamming Code?", "id": "Kode Koreksi Kesalahan Data." },
  { "en": "Apa Itu Checksum Data?", "id": "Penjumlahan Verifikasi Integritas Data." },
  { "en": "Apa Itu CRC-16?", "id": "Cyclic Redundancy Check 16 Bit." },
  { "en": "Apa Itu Sinyal Dominant CAN Bus?", "id": "Logika 0 Tegangan Beda Tinggi." },
  { "en": "Apa Itu Sinyal Recessive CAN Bus?", "id": "Logika 1 Tegangan Beda Nol." },
  { "en": "Apa Itu Arbitration ID CAN Bus?", "id": "Prioritas Pesan Dalam Jaringan." },
  { "en": "Tegangan CAN High Saat Dominant?", "id": "3,5 Volt." },
  { "en": "Tegangan CAN Low Saat Dominant?", "id": "1,5 Volt." },
  { "en": "Tegangan CAN Bus Saat Recessive?", "id": "2,5 Volt." },
  { "en": "Apa Itu Protokol LIN Bus?", "id": "Jaringan Mobil Kecepatan Rendah." },
  { "en": "Berapa Kabel Pada LIN Bus?", "id": "1 Kabel Data." },
  { "en": "Apa Itu Protokol FlexRay?", "id": "Jaringan Mobil Kecepatan Tinggi." },
  { "en": "Apa Itu Protokol EtherCAT?", "id": "Ethernet Industri Real Time Cepat." },
  { "en": "Apa Itu Protokol Profinet IRT?", "id": "Isochronous Real Time." },
  { "en": "Apa Itu Protokol Modbus TCP?", "id": "Modbus Lewat Kabel Ethernet." },
  { "en": "Apa Itu OPC UA?", "id": "Standar Pertukaran Data Industri." },
  { "en": "Apa Itu IO-Link Master?", "id": "Pusat Sensor Cerdas." },
  { "en": "Apa Kelebihan Sensor IO-Link?", "id": "Bisa Setting Parameter Jarak Jauh." },
  { "en": "Apa Itu Field Oriented Control (FOC)?", "id": "Kontrol Motor AC Seperti DC." },
  { "en": "Apa Itu Transformasi Clarke?", "id": "Ubah 3 Fasa Ke 2 Fasa." },
  { "en": "Apa Itu Transformasi Park?", "id": "Ubah Diam Ke Kerangka Putar." },
  { "en": "Apa Sumbu d Pada FOC?", "id": "Sumbu Fluks Magnet." },
  { "en": "Apa Sumbu q Pada FOC?", "id": "Sumbu Torsi Motor." },
  { "en": "Apa Itu SVM (Space Vector Modulation)?", "id": "Teknik Switching Inverter Vektor." },
  { "en": "Apa Kelebihan SVM Dibanding SPWM?", "id": "Tegangan Output Lebih Tinggi." },
  { "en": "Apa Itu Enkoder Absolut Multi Turn?", "id": "Hitung Posisi Lebih 1 Putaran." },
  { "en": "Apa Itu Enkoder Absolut Single Turn?", "id": "Hanya Posisi Dalam 1 Putaran." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Beda Presisi Tinggi." },
  { "en": "Instrumentation Amplifier Terdiri Dari?", "id": "3 Buah Op Amp." },
  { "en": "Apa Itu Isolation Amplifier?", "id": "Penguat Terpisah Secara Galvanis." },
  { "en": "Apa Itu Logarithmic Amplifier?", "id": "Output Logaritma Dari Input." },
  { "en": "Apa Itu Precision Rectifier?", "id": "Penyearah Dioda Aktif Op Amp." },
  { "en": "Fungsi Precision Rectifier Adalah?", "id": "Menyearahkan Sinyal Sangat Kecil." },
  { "en": "Apa Itu Peak Detector Circuit?", "id": "Menahan Nilai Puncak Sinyal." },
  { "en": "Apa Itu Sample And Hold Circuit?", "id": "Mengambil Dan Menahan Tegangan." },
  { "en": "Komponen Utama Sample And Hold?", "id": "Kapasitor Dan Switch." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Osilator Gelombang Kotak Kontinyu." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Satu Pulsa Timer Sesaat." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Flip Flop Memori 1 Bit." },
  { "en": "Pin Control Voltage 555 Berfungsi?", "id": "Mengatur Ambang Batas Comparator." },
  { "en": "Pin Reset 555 Aktif Pada Logika?", "id": "Logika Low 0." },
  { "en": "Pin Discharge 555 Terhubung Ke?", "id": "Kolektor Transistor Internal." },
  { "en": "Apa Itu Surge Impedance Loading (SIL)?", "id": "Daya Alami Saluran Transmisi." },
  { "en": "Jika Beban Di Bawah SIL Maka?", "id": "Tegangan Saluran Akan Naik." },
  { "en": "Jika Beban Di Atas SIL Maka?", "id": "Tegangan Saluran Akan Turun." },
  { "en": "Apa Itu Subsynchronous Resonance (SSR)?", "id": "Interaksi Turbin Dan Kapasitor Seri." },
  { "en": "SSR Berbahaya Bagi Komponen Apa?", "id": "Poros Generator Pembangkit." },
  { "en": "Apa Itu Power Swing?", "id": "Ayunan Daya Akibat Gangguan." },
  { "en": "Apa Itu Out Of Step Protection?", "id": "Proteksi Hilang Sinkronisasi Generator." },
  { "en": "Kode Relay Out Of Step Adalah?", "id": "78." },
  { "en": "Apa Itu Excitation Loss Relay?", "id": "Proteksi Hilang Medan Penguat." },
  { "en": "Kode Relay Loss Of Field?", "id": "40." },
  { "en": "Akibat Hilang Eksitasi Adalah?", "id": "Generator Menjadi Generator Induksi." },
  { "en": "Apa Itu Reverse Phase Relay?", "id": "Proteksi Urutan Fasa Terbalik." },
  { "en": "Kode Relay Reverse Phase Adalah?", "id": "47." },
  { "en": "Apa Itu Breaker Failure Relay?", "id": "Proteksi Jika Breaker Macet." },
  { "en": "Kode Relay Breaker Failure Adalah?", "id": "50BF." },
  { "en": "Apa Itu Busbar Protection?", "id": "Proteksi Differential Busbar." },
  { "en": "Apa Itu High Impedance Differential?", "id": "Proteksi Busbar Stabil." },
  { "en": "Apa Itu Trip Circuit Supervision?", "id": "Memantau Kesiapan Koil Trip." },
  { "en": "Apa Itu AOI (Automated Optical Inspection)?", "id": "Kamera Inspeksi PCB Otomatis." },
  { "en": "AOI Digunakan Untuk Mendeteksi?", "id": "Salah Komponen Atau Solderan Buruk." },
  { "en": "Apa Itu X-Ray Inspection PCB?", "id": "Melihat Solderan Di Bawah IC." },
  { "en": "X-Ray Wajib Untuk Komponen?", "id": "BGA Ball Grid Array." },
  { "en": "Apa Itu ICT (In Circuit Test)?", "id": "Uji Jarum Tiap Titik PCB." },
  { "en": "Apa Itu FCT (Functional Circuit Test)?", "id": "Uji Fungsi PCB Menyala." },
  { "en": "Apa Itu Burn In Test?", "id": "Uji Nyala Durasi Lama." },
  { "en": "Tujuan Burn In Test Adalah?", "id": "Menyingkirkan Produk Cacat Awal." },
  { "en": "Apa Itu Depanelization PCB?", "id": "Memisahkan PCB Dari Panel Utama." },
  { "en": "Apa Itu V-Cut Scoring?", "id": "Garis Potong Patah PCB." },
  { "en": "Apa Itu Routing PCB?", "id": "Memotong PCB Dengan Mata Bor." },
  { "en": "Apa Itu Stencil Printer?", "id": "Alat Oles Pasta Solder." },
  { "en": "Ketebalan Stencil Menentukan Apa?", "id": "Volume Timah Solder." },
  { "en": "Apa Itu Solder Paste?", "id": "Campuran Serbuk Timah Dan Flux." },
  { "en": "Ukuran Butir Pasta Tipe 4 Adalah?", "id": "Lebih Halus Dari Tipe 3." },
  { "en": "Komponen 0402 Butuh Pasta Tipe?", "id": "Tipe 4 Atau Tipe 5." },
  { "en": "Apa Itu Solder Ball Defect?", "id": "Bola Timah Liar Di PCB." },
  { "en": "Penyebab Solder Ball Adalah?", "id": "Pemanasan Terlalu Cepat." },
  { "en": "Apa Itu Tombstoning Defect?", "id": "Komponen Berdiri Sebelah." },
  { "en": "Penyebab Tombstoning Adalah?", "id": "Panas Pad Tidak Seimbang." },
  { "en": "Apa Itu Voiding Pada Solder?", "id": "Rongga Udara Dalam Timah." },
  { "en": "Apa Itu Cold Solder?", "id": "Solderan Kusam Tidak Matang." },
  { "en": "Apa Itu Conservator Tank Trafo?", "id": "Tangki Pemuaian Minyak Trafo." },
  { "en": "Apa Fungsi Breather Pada Trafo?", "id": "Saluran Napas Udara Trafo." },
  { "en": "Apa Isi Dalam Breather Trafo?", "id": "Silica Gel Penyerap Air." },
  { "en": "Apa Itu Sudden Pressure Relay?", "id": "Proteksi Lonjakan Tekanan Minyak." },
  { "en": "Apa Itu Pressure Relief Valve?", "id": "Katup Buang Tekanan Berlebih." },
  { "en": "Apa Itu Radiator Trafo?", "id": "Sirip Pendingin Minyak." },
  { "en": "Apa Itu Winding Temperature Indicator?", "id": "Termometer Suhu Gulungan Trafo." },
  { "en": "Apa Itu Oil Temperature Indicator?", "id": "Termometer Suhu Minyak Atas." },
  { "en": "Apa Itu Hermetically Sealed Trafo?", "id": "Trafo Kedap Udara Total." },
  { "en": "Kelebihan Trafo Hermetic Adalah?", "id": "Minyak Tidak Terkontaminasi Udara." },
  { "en": "Apa Itu Corrugated Tank?", "id": "Tangki Sirip Bergelombang." },
  { "en": "Apa Itu Dry Type Transformer?", "id": "Trafo Kering Tanpa Minyak." },
  { "en": "Isolasi Trafo Kering Menggunakan?", "id": "Resin Epoksi Cast Resin." },
  { "en": "Kelebihan Trafo Kering Adalah?", "id": "Anti Kebakaran Dan Ledakan." },
  { "en": "Apa Itu Vector Group Dyn5?", "id": "Delta Star Netral 150 Derajat." },
  { "en": "Apa Itu Vector Group Yyn0?", "id": "Star Star Netral 0 Derajat." },
  { "en": "Apa Itu Impedansi Ukur Trafo?", "id": "Persentase Tegangan Short Circuit." },
  { "en": "Trafo Impedansi Tinggi Menyebabkan?", "id": "Drop Tegangan Besar." },
  { "en": "Trafo Impedansi Rendah Menyebabkan?", "id": "Arus Hubung Singkat Besar." },
  { "en": "Apa Itu Inrush Current Trafo?", "id": "Lonjakan Arus Saat Energize." },
  { "en": "Apa Itu Remanensi Magnet?", "id": "Sisa Magnet Di Inti Besi." },
  { "en": "Apa Itu Ladder Logic Rung?", "id": "Satu Baris Logika Program." },
  { "en": "Apa Itu Normally Open Contact?", "id": "Terputus Jika Tidak Aktif." },
  { "en": "Apa Itu Normally Closed Contact?", "id": "Tersambung Jika Tidak Aktif." },
  { "en": "Apa Itu Rising Edge Pulse?", "id": "Sinyal Sesaat Saat Naik." },
  { "en": "Apa Itu Falling Edge Pulse?", "id": "Sinyal Sesaat Saat Turun." },
  { "en": "Apa Itu Set Coil PLC?", "id": "Mengunci Output Jadi On." },
  { "en": "Apa Itu Reset Coil PLC?", "id": "Mematikan Output Terkunci." },
  { "en": "Apa Itu Timer On Delay?", "id": "Menunda Output Menyala." },
  { "en": "Apa Itu Timer Off Delay?", "id": "Menunda Output Mati." },
  { "en": "Apa Itu Timer Pulse?", "id": "Menyala Sesaat Sesuai Waktu." },
  { "en": "Apa Itu Counter Up?", "id": "Menghitung Naik 1, 2, 3." },
  { "en": "Apa Itu Counter Down?", "id": "Menghitung Turun 3, 2, 1." },
  { "en": "Apa Itu PID Loop Tuning?", "id": "Mengatur Parameter P I D." },
  { "en": "Apa Itu HMI Touchscreen?", "id": "Layar Sentuh Antarmuka Mesin." },
  { "en": "Apa Itu Tag Database HMI?", "id": "Daftar Variabel Data Mesin." },
  { "en": "Apa Itu Alarm Logging?", "id": "Mencatat Riwayat Kerusakan." },
  { "en": "Apa Itu Trend Chart?", "id": "Grafik Data Real Time." },
  { "en": "Apa Itu Permittivity Bahan?", "id": "Kemampuan Menyimpan Medan Listrik." },
  { "en": "Apa Itu Dielectric Constant?", "id": "Konstanta Dielektrik Relatif." },
  { "en": "Konstanta Dielektrik Udara Adalah?", "id": "1." },
  { "en": "Konstanta Dielektrik Keramik Adalah?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Breakdown Strength?", "id": "Kekuatan Tahan Tembus Tegangan." },
  { "en": "Apa Itu Surface Resistivity?", "id": "Tahanan Permukaan Bahan." },
  { "en": "Apa Itu Volume Resistivity?", "id": "Tahanan Dalam Bahan." },
  { "en": "Apa Itu Piezoelectric Effect?", "id": "Tekanan Menjadi Tegangan Listrik." },
  { "en": "Apa Itu Pyroelectric Effect?", "id": "Panas Menjadi Tegangan Listrik." },
  { "en": "Apa Itu Triboelectric Effect?", "id": "Gesekan Menjadi Listrik Statis." },
  { "en": "Apa Itu Superconductivity?", "id": "Hambatan Nol Suhu Rendah." },
  { "en": "Suhu Kritis Superkonduktor Adalah?", "id": "Suhu Mulai Hambatan Nol." },
  { "en": "Apa Itu Distributed Generation?", "id": "Pembangkit Listrik Tersebar." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Mandiri Kecil." },
  { "en": "Apa Itu Demand Response?", "id": "Pengurangan Beban Saat Puncak." },
  { "en": "Apa Itu Peak Shaving?", "id": "Memotong Beban Puncak Listrik." },
  { "en": "Apa Itu Load Shifting?", "id": "Geser Beban Ke Luar Puncak." },
  { "en": "Apa Itu V2G (Vehicle To Grid)?", "id": "Mobil Listrik Suplai PLN." },
  { "en": "Apa Itu Phasor Measurement Unit?", "id": "Alat Ukur Fasa Sinkron." },
  { "en": "Apa Itu WAMS (Wide Area Measurement)?", "id": "Sistem Pantau Grid Luas." },
  { "en": "Apa Itu IED (Intelligent Electronic Device)?", "id": "Alat Proteksi Kontrol Cerdas." },
  { "en": "Protokol Komunikasi Gardu Induk?", "id": "IEC 61850." },
  { "en": "Apa Itu GOOSE Message?", "id": "Pesan Cepat Antar IED." },
  { "en": "Apa Itu Merging Unit?", "id": "Antarmuka CT PT Digital." },
  { "en": "Apa Itu Process Bus IEC 61850?", "id": "Jaringan Data Sensor Gardu." },
  { "en": "Apa Itu Station Bus IEC 61850?", "id": "Jaringan Data Antar IED." },
  { "en": "Apa Itu Time Synchronization PTP?", "id": "Sinkronisasi Waktu Presisi Jaringan." },
  { "en": "Apa Itu GPS Clock?", "id": "Jam Satelit Untuk Sinkronisasi." },
  { "en": "Apa Itu Cyber Security OT?", "id": "Keamanan Jaringan Operasional Teknologi." },
  { "en": "Apa Itu Air Gap Network?", "id": "Jaringan Terpisah Fisik Total." },
  { "en": "Apa Itu Industrial Firewall?", "id": "Penyaring Data Jaringan Pabrik." },
  { "en": "Apa Itu VPN Tunnel?", "id": "Jalur Aman Akses Jarak Jauh." },
  { "en": "Apa Itu Thyristor GTO?", "id": "Gate Turn Off Thyristor." },
  { "en": "Kelebihan GTO Dibanding SCR?", "id": "Bisa Dimatikan Lewat Gate." },
  { "en": "Apa Itu IGCT?", "id": "Integrated Gate Commutated Thyristor." },
  { "en": "IGCT Digunakan Untuk Aplikasi?", "id": "Daya Sangat Besar MV." },
  { "en": "Apa Itu MCT (MOS Controlled Thyristor)?", "id": "Thyristor Dikendalikan Tegangan MOS." },
  { "en": "Apa Itu IPM (Intelligent Power Module)?", "id": "IGBT Dengan Driver Internal." },
  { "en": "Apa Itu PIM (Power Integrated Module)?", "id": "Modul IGBT Penyearah Lengkap." },
  { "en": "Apa Itu Freewheeling Diode?", "id": "Dioda Paralel Beban Induktif." },
  { "en": "Fungsi Freewheeling Diode Adalah?", "id": "Mencegah Lonjakan Tegangan Balik." },
  { "en": "Apa Itu Snubber Circuit RCD?", "id": "Resistor Capacitor Diode." },
  { "en": "Snubber Berfungsi Untuk?", "id": "Meredam Spike Saat Switching." },
  { "en": "Apa Itu Hard Switching?", "id": "Switching Saat Ada Tegangan Arus." },
  { "en": "Apa Itu Soft Switching ZVS?", "id": "Switching Saat Tegangan Nol." },
  { "en": "Apa Itu Soft Switching ZCS?", "id": "Switching Saat Arus Nol." },
  { "en": "Apa Itu Resonant Converter?", "id": "Konverter Menggunakan Resonansi LC." },
  { "en": "Kelebihan Resonant Converter?", "id": "Efisiensi Tinggi Minim EMI." },
  { "en": "Apa Itu EMI Filter Power Line?", "id": "Filter Gangguan Jala Jala." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Kembar Peredam Noise." },
  { "en": "Common Mode Noise Adalah?", "id": "Gangguan Pada Kedua Jalur." },
  { "en": "Differential Mode Noise Adalah?", "id": "Gangguan Antar Dua Jalur." },
  { "en": "Kapasitor Kelas X Untuk Noise?", "id": "Differential Mode." },
  { "en": "Kapasitor Kelas Y Untuk Noise?", "id": "Common Mode." },
  { "en": "Apa Itu Ferrite Bead Kabel?", "id": "Cincin Magnet Peredam Frekuensi." },
  { "en": "Apa Itu Shielded Cable?", "id": "Kabel Dengan Anyaman Logam." },
  { "en": "Apa Itu Dead Band Sensor?", "id": "Zona Tanpa Respon Output." },
  { "en": "Apa Itu Hysteresis Error Sensor?", "id": "Beda Nilai Naik Dan Turun." },
  { "en": "Apa Itu Range Sensor?", "id": "Rentang Ukur Minimal Maksimal." },
  { "en": "Apa Itu Span Sensor?", "id": "Selisih Nilai Maksimal Dan Minimal." },
  { "en": "Apa Itu Zero Suppression?", "id": "Menggeser Titik Nol Ke Bawah." },
  { "en": "Apa Itu Zero Elevation?", "id": "Menggeser Titik Nol Ke Atas." },
  { "en": "Apa Itu Turndown Ratio Sensor?", "id": "Rasio Rentang Maksimum Dan Minimum." },
  { "en": "Apa Itu Calibration Certificate?", "id": "Dokumen Bukti Akurasi Alat." },
  { "en": "Apa Itu Traceability Kalibrasi?", "id": "Ketelusuran Ke Standar Internasional." },
  { "en": "Apa Itu Smart Transmitter?", "id": "Sensor Dengan Mikroprosesor Digital." },
  { "en": "Apa Itu Cold Junction Thermocouple?", "id": "Titik Referensi Suhu Ruang." },
  { "en": "Apa Itu Hot Junction Thermocouple?", "id": "Titik Pengukuran Suhu Panas." },
  { "en": "Apa Itu Thermowell?", "id": "Pipa Pelindung Sensor Suhu." },
  { "en": "Fungsi Thermowell Adalah?", "id": "Melindungi Sensor Dari Tekanan Cairan." },
  { "en": "Apa Itu Slot Harmonics Motor?", "id": "Gangguan Frekuensi Akibat Alur Stator." },
  { "en": "Apa Itu Crawling Pada Motor?", "id": "Putaran Lambat Akibat Harmonisa Ganjil." },
  { "en": "Apa Itu Cogging Pada Motor?", "id": "Rotor Terkunci Magnetik Stator." },
  { "en": "Apa Itu Skewing Rotor?", "id": "Memiringkan Alur Batang Rotor." },
  { "en": "Fungsi Skewing Rotor Adalah?", "id": "Mengurangi Dengung Dan Cogging." },
  { "en": "Apa Itu Double Cage Rotor?", "id": "Rotor Sangkar Ganda Torsi Tinggi." },
  { "en": "Apa Itu Deep Bar Rotor?", "id": "Batang Rotor Dalam Torsi Tinggi." },
  { "en": "Efek Kulit Pada Rotor Deep Bar?", "id": "Meningkatkan Resistansi Saat Start." },
  { "en": "Apa Itu Air Gap Eccentricity?", "id": "Celah Udara Tidak Rata." },
  { "en": "Akibat Air Gap Tidak Rata?", "id": "Getaran Dan Suara Berisik." },
  { "en": "Apa Itu Magnetic Saturation?", "id": "Inti Besi Jenuh Fluks Magnet." },
  { "en": "Apa Itu Solder Paste Inspection (SPI)?", "id": "Cek Volume Pasta Solder." },
  { "en": "Apa Itu Squeegee SMT?", "id": "Alat Perata Pasta Solder." },
  { "en": "Apa Itu Pick And Place Nozzle?", "id": "Ujung Penyedot Komponen SMD." },
  { "en": "Apa Itu Feeder Mesin SMT?", "id": "Pengumpan Gulungan Komponen." },
  { "en": "Apa Itu Component Reel?", "id": "Gulungan Pita Komponen SMD." },
  { "en": "Apa Itu Chip Mounter?", "id": "Mesin Pasang Komponen Cepat." },
  { "en": "Apa Itu Reflow Profile?", "id": "Grafik Suhu Waktu Oven." },
  { "en": "Tahap Preheat Reflow Adalah?", "id": "Pemanasan Awal PCB." },
  { "en": "Tahap Soaking Reflow Adalah?", "id": "Aktivasi Flux Pasta Solder." },
  { "en": "Tahap Reflow Peak Adalah?", "id": "Pencairan Timah Maksimal." },
  { "en": "Tahap Cooling Reflow Adalah?", "id": "Pendinginan Pembekuan Timah." },
  { "en": "Apa Itu Water Hammer PLTA?", "id": "Pukulan Air Pipa Pesat." },
  { "en": "Apa Itu Surge Tank PLTA?", "id": "Tangki Peredam Tekanan Air." },
  { "en": "Apa Itu Trash Rack PLTA?", "id": "Saringan Sampah Pintu Air." },
  { "en": "Apa Itu Penstock PLTA?", "id": "Pipa Pesat Penyalur Air." },
  { "en": "Apa Itu Draft Tube PLTA?", "id": "Pipa Buang Turbin Air." },
  { "en": "Fungsi Draft Tube Adalah?", "id": "Memulihkan Tekanan Buang Air." },
  { "en": "Apa Itu Governor PLTA?", "id": "Pengatur Debit Air Turbin." },
  { "en": "Apa Itu Run Of River PLTA?", "id": "PLTA Tanpa Bendungan Besar." },
  { "en": "Apa Itu Pumped Storage PLTA?", "id": "PLTA Bisa Memompa Balik." },
  { "en": "Kapan Pumped Storage Memompa?", "id": "Saat Beban Listrik Rendah." },
  { "en": "Apa Itu Guide Vane Turbin?", "id": "Sirip Pengarah Aliran Air." },
  { "en": "Apa Itu Spiral Case Turbin?", "id": "Rumah Keong Turbin Air." },
  { "en": "Apa Itu Surge Arrester Counter?", "id": "Menghitung Jumlah Sambaran Petir." },
  { "en": "Apa Itu Grading Capacitor CB?", "id": "Meratakan Tegangan Kontak Breaker." },
  { "en": "Apa Itu TRV (Transient Recovery Voltage)?", "id": "Tegangan Balik Saat Memutus Arus." },
  { "en": "Apa Itu RRRV Breaker?", "id": "Laju Kenaikan Tegangan Balik." },
  { "en": "Apa Itu Current Chopping?", "id": "Pemutusan Arus Sebelum Titik Nol." },
  { "en": "Current Chopping Sering Terjadi Pada?", "id": "Vacuum Circuit Breaker." },
  { "en": "Apa Itu Pre Insertion Resistor?", "id": "Resistor Peredam Inrush Breaker." },
  { "en": "Apa Itu GIS Partial Discharge?", "id": "Cacat Isolasi Gas SF6." },
  { "en": "Apa Itu Lock Out Box?", "id": "Kotak Kunci Gembok LOTO." },
  { "en": "Apa Itu Tag Out Label?", "id": "Label Peringatan Bahaya Perbaikan." },
  { "en": "Apa Itu HRC Rating Pakaian?", "id": "Kategori Resiko Bahaya Api." },
  { "en": "Apa Itu Insulating Mat Kelas 0?", "id": "Karpet Isolasi 1000 Volt." },
  { "en": "Apa Itu Insulating Gloves Kelas 2?", "id": "Sarung Tangan Tahan 17000 Volt." },
  { "en": "Wajib Tes Sarung Tangan Isolasi?", "id": "Tes Tiup Angin Sebelum Pakai." },
  { "en": "Apa Itu Voltage Detector Stick?", "id": "Tongkat Deteksi Tegangan Tinggi." },
  { "en": "Apa Itu Discharge Rod?", "id": "Tongkat Pembuang Muatan Sisa." },
  { "en": "Discharge Rod Wajib Dipakai Saat?", "id": "Setelah Mematikan Listrik HV." },
  { "en": "Apa Itu Safety Harness Listrik?", "id": "Sabuk Pengaman Panjat Tiang." },
  { "en": "Apa Itu Full Body Harness?", "id": "Sabuk Pengaman Seluruh Tubuh." },
  { "en": "Apa Itu Lanyard Absorber?", "id": "Tali Peredam Kejut Jatuh." },
  { "en": "Apa Itu Arc Flash Face Shield?", "id": "Pelindung Wajah Anti Ledakan." },
  { "en": "Apa Itu Safety Shoes Electrical?", "id": "Sepatu Safety Tanpa Besi." },
  { "en": "Simbol Sepatu Safety Listrik Adalah?", "id": "Simbol Ohm Atau EH." },
  { "en": "Kepanjangan EH Pada Sepatu?", "id": "Electrical Hazard." },
  { "en": "Apa Itu Burden Voltage Multimeter?", "id": "Tegangan Jatuh Saat Ukur Arus." },
  { "en": "Apa Itu Crest Factor Multimeter?", "id": "Kemampuan Ukur Puncak Gelombang." },
  { "en": "Apa Itu Bandwidth Multimeter?", "id": "Rentang Frekuensi Ukur AC." },
  { "en": "Apa Itu Count Pada Multimeter?", "id": "Jumlah Digit Tampilan Layar." },
  { "en": "Multimeter 2000 Counts Menampilkan Maksimal?", "id": "Angka 1999." },
  { "en": "Multimeter 6000 Counts Menampilkan Maksimal?", "id": "Angka 5999." },
  { "en": "Apa Itu Relative Mode (REL)?", "id": "Mengukur Selisih Nilai Referensi." },
  { "en": "Apa Itu Low Z Mode?", "id": "Impedansi Rendah Buang Ghost Voltage." },
  { "en": "Apa Itu Ghost Voltage?", "id": "Tegangan Semu Kabel Induksi." },
  { "en": "Apa Itu Non Contact Voltage (NCV)?", "id": "Deteksi Listrik Tanpa Sentuh." },
  { "en": "Apa Itu Analog Bar Graph?", "id": "Grafik Batang Di Layar Digital." },
  { "en": "Fungsi Analog Bar Graph?", "id": "Melihat Perubahan Sinyal Cepat." },
  { "en": "Apa Itu Duty Cycle Measurement?", "id": "Mengukur Persen Sinyal On." },
  { "en": "Apa Itu Pulse Width Measurement?", "id": "Mengukur Lebar Waktu Pulsa." },
  { "en": "Apa Itu Diode Test Mode?", "id": "Mengukur Tegangan Maju Dioda." },
  { "en": "Apa Itu Continuity Test Mode?", "id": "Bunyi Beep Jika Tersambung." },
  { "en": "Ambang Batas Continuity Test Biasanya?", "id": "Di Bawah 50 Ohm." },
  { "en": "Apa Itu Data Hold?", "id": "Membekukan Angka Di Layar." },
  { "en": "Apa Itu Auto Hold?", "id": "Membekukan Angka Saat Stabil." },
  { "en": "Apa Itu Min Max Average?", "id": "Rekam Nilai Terendah Tertinggi Rata Rata." },
  { "en": "Apa Itu Impulse Voltage Generator?", "id": "Pembangkit Tegangan Kejut Petir." },
  { "en": "Bentuk Gelombang Impuls Standar Adalah?", "id": "1,2 Garis Miring 50 Mikrodetik." },
  { "en": "Apa Itu Sphere Gap?", "id": "Celah Bola Ukur Tegangan." },
  { "en": "Apa Fungsi Sphere Gap?", "id": "Kalibrasi Tegangan Tinggi Impuls." },
  { "en": "Apa Itu Rod Gap Arrester?", "id": "Celah Batang Pelindung Sederhana." },
  { "en": "Apa Itu BIL Rating Trafo?", "id": "Basic Impulse Insulation Level." },
  { "en": "Uji BIL Mensimulasikan Apa?", "id": "Sambaran Petir Pada Peralatan." },
  { "en": "Apa Itu Withstand Voltage Test?", "id": "Uji Tahan Tegangan Frekuensi Kerja." },
  { "en": "Berapa Lama Durasi Withstand Test?", "id": "Biasanya 1 Menit." },
  { "en": "Apa Itu Relay Directional Overcurrent?", "id": "Proteksi Arus Lebih Berarah." },
  { "en": "Kode ANSI Relay Directional Adalah?", "id": "67." },
  { "en": "Relay Directional Membutuhkan Input Apa?", "id": "Arus Dan Tegangan Referensi." },
  { "en": "Apa Itu Relay Directional Power?", "id": "Proteksi Daya Balik Aktif." },
  { "en": "Kode ANSI Relay Power Directional?", "id": "32." },
  { "en": "Apa Itu Relay Frequency Rate Of Change?", "id": "Proteksi Perubahan Frekuensi Cepat." },
  { "en": "Kode ANSI Relay ROCOF Adalah?", "id": "81R." },
  { "en": "Apa Itu Vector Shift Relay?", "id": "Deteksi Pergeseran Fasa Tiba Tiba." },
  { "en": "Apa Itu Anti Pumping Relay?", "id": "Mencegah Breaker On Off Berulang." },
  { "en": "Kode ANSI Anti Pumping Adalah?", "id": "94." },
  { "en": "Apa Itu Coefficient Of Utilization (CU)?", "id": "Efisiensi Cahaya Ruangan." },
  { "en": "Apa Itu Maintenance Factor Lampu?", "id": "Faktor Penurunan Cahaya." },
  { "en": "Nilai Maintenance Factor Standar Adalah?", "id": "0,8." },
  { "en": "Apa Itu Unified Glare Rating (UGR)?", "id": "Indeks Silau Cahaya Ruangan." },
  { "en": "Batas UGR Ruang Kantor Adalah?", "id": "Di Bawah 19." },
  { "en": "Apa Itu Lux Meter Cosine Corrected?", "id": "Akurat Dari Sudut Miring." },
  { "en": "Apa Itu Polar Curve Lampu?", "id": "Grafik Sebaran Arah Cahaya." },
  { "en": "Apa Itu IES File?", "id": "Data Fotometrik Lampu Digital." },
  { "en": "Software Simulasi Pencahayaan Populer Adalah?", "id": "Dialux." },
  { "en": "Apa Itu Early Effect Transistor?", "id": "Modulasi Lebar Basis BJT." },
  { "en": "Apa Itu Secondary Breakdown Transistor?", "id": "Kerusakan Hotspot Lokal BJT." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Panas Menghasilkan Panas Berlebih." },
  { "en": "Komponen Paling Rentan Thermal Runaway?", "id": "Transistor Bipolar BJT." },
  { "en": "Apa Itu Electromigration?", "id": "Perpindahan Ion Logam Chip." },
  { "en": "Electromigration Terjadi Karena?", "id": "Kerapatan Arus Sangat Tinggi." },
  { "en": "Apa Itu Tin Whiskers?", "id": "Kumis Timah Tumbuh Sendiri." },
  { "en": "Bahaya Tin Whiskers Adalah?", "id": "Hubung Singkat Kaki Komponen." },
  { "en": "Apa Itu Purple Plague Emas?", "id": "Korosi Sambungan Emas Aluminium." },
  { "en": "Apa Itu Popcorn Effect IC?", "id": "IC Retak Saat Disolder." },
  { "en": "Penyebab Popcorn Effect Adalah?", "id": "Uap Air Dalam Kemasan IC." },
  { "en": "Apa Itu MSL (Moisture Sensitivity Level)?", "id": "Tingkat Sensitivitas Lembab IC." },
  { "en": "Apa Itu Desiccant Bag?", "id": "Kantong Pengering Kemasan Komponen." },
  { "en": "Apa Itu Humidity Indicator Card?", "id": "Kertas Indikator Kelembaban." },
  { "en": "Apa Itu Baking Komponen?", "id": "Memanaskan IC Sebelum Solder." },
  { "en": "Tujuan Baking Komponen Adalah?", "id": "Menguapkan Air Dalam IC." },
  { "en": "Apa Itu AC Coupling Osiloskop?", "id": "Memblokir Komponen DC Sinyal." },
  { "en": "Apa Itu DC Coupling Osiloskop?", "id": "Menampilkan AC Dan DC." },
  { "en": "Apa Itu Probe Ground Spring?", "id": "Ground Pendek Frekuensi Tinggi." },
  { "en": "Mengapa Kabel Ground Probe Panjang Buruk?", "id": "Menangkap Noise Induktansi Tinggi." },
  { "en": "Apa Itu Roll Mode Osiloskop?", "id": "Grafik Berjalan Lambat." },
  { "en": "Apa Itu Trigger Holdoff?", "id": "Jeda Waktu Sebelum Trigger." },
  { "en": "Apa Itu Persistence Display?", "id": "Jejak Sinyal Lama Tertinggal." },
  { "en": "Fungsi Persistence Adalah?", "id": "Melihat Glitch Jarang Muncul." },
  { "en": "Apa Itu Tegangan Equalizing Aki 12V?", "id": "14,4 Volt Sampai 15,5 Volt." },
  { "en": "Apa Itu Tegangan Gassing Aki?", "id": "Tegangan Mulai Keluar Gas." },
  { "en": "Bahaya Gas Hidrogen Aki Adalah?", "id": "Mudah Meledak." },
  { "en": "Apa Itu Desulfator Aki?", "id": "Pemecah Kristal Sulfat." },
  { "en": "Desulfator Menggunakan Teknik Apa?", "id": "Pulsa Frekuensi Tinggi." },
  { "en": "Apa Itu Battery Hydrometer?", "id": "Alat Ukur Berat Jenis Air." },
  { "en": "Apa Itu AGM Battery?", "id": "Absorbent Glass Mat." },
  { "en": "Elektrolit Baterai AGM Tersimpan Di?", "id": "Serat Kaca Spons." },
  { "en": "Apa Itu Gel Battery?", "id": "Elektrolit Dicampur Silika Gel." },
  { "en": "Kelebihan Baterai Gel Adalah?", "id": "Tahan Suhu Dan Getaran." },
  { "en": "Apa Itu Conductor Resistance Test?", "id": "Uji Tahanan Penghantar DC." },
  { "en": "Apa Itu HV Test Kabel?", "id": "High Voltage Test." },
  { "en": "Apa Itu Sheath Test Kabel?", "id": "Uji Kebocoran Kulit Kabel." },
  { "en": "Apa Itu Armor Continuity Test?", "id": "Uji Sambungan Kawat Baja." },
  { "en": "Apa Itu Phase Check Kabel?", "id": "Memastikan Urutan R S T." },
  { "en": "Apa Itu TDR (Time Domain Reflectometer)?", "id": "Pencari Lokasi Putus Kabel." },
  { "en": "Prinsip Kerja TDR Adalah?", "id": "Pantulan Pulsa Listrik." },
  { "en": "Apa Itu Cable Thumping?", "id": "Dentuman Suara Lokasi Gangguan." },
  { "en": "Apa Itu Dead Time Process Control?", "id": "Waktu Tunda Respon Sistem." },
  { "en": "Apa Itu Time Constant Tau?", "id": "Waktu Mencapai 63,2 Persen." },
  { "en": "Apa Itu Rise Time?", "id": "Waktu Naik 10 Ke 90 Persen." },
  { "en": "Apa Itu Settling Time?", "id": "Waktu Stabil Di Setpoint." },
  { "en": "Apa Itu Peak Time?", "id": "Waktu Mencapai Puncak Overshoot." },
  { "en": "Apa Itu Ramp Input?", "id": "Sinyal Naik Miring Konstan." },
  { "en": "Apa Itu Step Input?", "id": "Sinyal Naik Tegak Lurus." },
  { "en": "Apa Itu Impulse Input?", "id": "Sinyal Denyut Sesaat." },
  { "en": "Apa Itu Kurva P-V?", "id": "Hubungan Daya Aktif Dan Tegangan." },
  { "en": "Apa Itu Kurva Q-V?", "id": "Hubungan Daya Reaktif Dan Tegangan." },
  { "en": "Titik Kritis Kurva P-V Adalah?", "id": "Batas Kestabilan Tegangan." },
  { "en": "Apa Itu Voltage Collapse?", "id": "Kejatuhan Tegangan Sistem Total." },
  { "en": "Penyebab Voltage Collapse Adalah?", "id": "Kekurangan Daya Reaktif." },
  { "en": "Apa Itu Load Flow Analysis?", "id": "Analisis Aliran Daya." },
  { "en": "Apa Itu Short Circuit Analysis?", "id": "Analisis Arus Hubung Singkat." },
  { "en": "Apa Itu Swing Bus (Slack Bus)?", "id": "Bus Penyeimbang Daya Sistem." },
  { "en": "Apa Itu PV Bus?", "id": "Bus Pembangkit Tegangan Tetap." },
  { "en": "Apa Itu PQ Bus?", "id": "Bus Beban Daya Tetap." },
  { "en": "Apa Itu Pitch Factor Lilitan?", "id": "Rasio Kisar Lilitan Stator." },
  { "en": "Apa Itu Chording Atau Short Pitching?", "id": "Memperpendek Jarak Lilitan." },
  { "en": "Tujuan Short Pitching Adalah?", "id": "Mengurangi Harmonisa Dan Tembaga." },
  { "en": "Apa Itu Distribusi Fluks Sinusoidal?", "id": "Bentuk Gelombang Magnet Ideal." },
  { "en": "Apa Itu Fractional Slot Winding?", "id": "Jumlah Alur Per Kutub Pecahan." },
  { "en": "Apa Itu Concentrated Winding?", "id": "Lilitan Terpusat Pada Satu Gigi." },
  { "en": "Apa Itu Distributed Winding?", "id": "Lilitan Tersebar Di Banyak Alur." },
  { "en": "Motor BLDC Menggunakan Winding Tipe?", "id": "Concentrated Winding." },
  { "en": "Motor Induksi Menggunakan Winding Tipe?", "id": "Distributed Winding." },
  { "en": "Apa Itu Dew Point Test SF6?", "id": "Uji Titik Embun Kelembaban." },
  { "en": "Apa Itu Torque Boost Pada VFD?", "id": "Menambah Tegangan Frekuensi Rendah." },
  { "en": "Fungsi Torque Boost Adalah?", "id": "Meningkatkan Torsi Start Motor." },
  { "en": "Apa Itu Slip Compensation VFD?", "id": "Menjaga Kecepatan Saat Beban Naik." },
  { "en": "Apa Itu Carrier Frequency VFD?", "id": "Frekuensi Switching Transistor IGBT." },
  { "en": "Efek Carrier Frequency Tinggi Adalah?", "id": "Motor Senyap Inverter Panas." },
  { "en": "Efek Carrier Frequency Rendah Adalah?", "id": "Motor Berisik Inverter Dingin." },
  { "en": "Apa Itu DC Injection Braking?", "id": "Pengereman Suntik Arus DC." },
  { "en": "Kapan DC Injection Digunakan?", "id": "Menahan Motor Saat Berhenti." },
  { "en": "Apa Itu Coast To Stop?", "id": "Motor Berhenti Alami Tanpa Rem." },
  { "en": "Apa Itu Ramp To Stop?", "id": "Motor Berhenti Mengikuti Kurva Waktu." },
  { "en": "Apa Itu Flying Start VFD?", "id": "Menjalankan Motor Yang Sedang Berputar." },
  { "en": "Apa Itu Skip Frequency VFD?", "id": "Melompati Frekuensi Resonansi Mekanik." },
  { "en": "Apa Itu Jogging Atau Inching?", "id": "Menjalankan Motor Sesaat." },
  { "en": "Apa Itu Multi Speed Step?", "id": "Kecepatan Bertingkat Digital Input." },
  { "en": "Apa Itu PID Reverse Action?", "id": "Output Naik Jika Error Turun." },
  { "en": "Apa Itu PID Direct Action?", "id": "Output Naik Jika Error Naik." },
  { "en": "Aplikasi PID Reverse Action Adalah?", "id": "Pemanas Heater." },
  { "en": "Aplikasi PID Direct Action Adalah?", "id": "Pendingin Cooler." },
  { "en": "Apa Itu Interharmonics?", "id": "Frekuensi Bukan Kelipatan Bulat." },
  { "en": "Penyebab Interharmonics Adalah?", "id": "Cycloconverter Dan Tungku Busur." },
  { "en": "Apa Itu Voltage Notching?", "id": "Cacat Tegangan Akibat Komutasi SCR." },
  { "en": "Apa Itu Noise Electrical?", "id": "Sinyal Gangguan Frekuensi Tinggi." },
  { "en": "Apa Itu Common Mode Noise?", "id": "Gangguan Fasa Netral Ke Ground." },
  { "en": "Apa Itu Differential Mode Noise?", "id": "Gangguan Antara Fasa Dan Netral." },
  { "en": "Filter EMI Digunakan Untuk?", "id": "Meredam Noise Elektromagnetik." },
  { "en": "Apa Itu Impulse Withstand Voltage?", "id": "Tahan Tegangan Kejut Petir." },
  { "en": "Apa Itu Creepage Distance Isolator?", "id": "Jarak Rambat Permukaan." },
  { "en": "Apa Itu Clearance Distance Isolator?", "id": "Jarak Bebas Udara." },
  { "en": "Apa Itu Wet Flashover Voltage?", "id": "Tegangan Loncat Api Saat Basah." },
  { "en": "Apa Itu Dry Flashover Voltage?", "id": "Tegangan Loncat Api Saat Kering." },
  { "en": "Apa Itu Corona Ring?", "id": "Cincin Pemerata Medan Listrik." },
  { "en": "Fungsi Corona Ring Adalah?", "id": "Mencegah Pendaran Cahaya Korona." },
  { "en": "Apa Itu Insulator Glazing?", "id": "Lapisan Kaca Keramik Isolator." },
  { "en": "Fungsi Glazing Adalah?", "id": "Agar Kotoran Mudah Hilang." },
  { "en": "Apa Itu Pollution Severity Level?", "id": "Tingkat Polusi Lingkungan Isolator." },
  { "en": "Daerah Pantai Membutuhkan Isolator?", "id": "Tahan Garam Dan Korosi." },
  { "en": "Apa Itu Baterai NCA?", "id": "Nickel Cobalt Aluminum." },
  { "en": "Apa Itu Baterai LMO?", "id": "Lithium Manganese Oxide." },
  { "en": "Kelebihan Baterai LFP (LiFePO4)?", "id": "Sangat Aman Dan Awet." },
  { "en": "Kekurangan Baterai LFP?", "id": "Densitas Energi Lebih Rendah." },
  { "en": "Apa Itu Specific Energy Wh/kg?", "id": "Energi Per Berat Baterai." },
  { "en": "Apa Itu Energy Density Wh/L?", "id": "Energi Per Volume Baterai." },
  { "en": "Baterai Solid State Menggunakan?", "id": "Elektrolit Padat." },
  { "en": "Keunggulan Solid State Battery?", "id": "Tidak Mudah Terbakar." },
  { "en": "Apa Itu Battery Venting?", "id": "Pelepasan Gas Tekanan Tinggi." },
  { "en": "Apa Itu Battery Swelling (Kembung)?", "id": "Akumulasi Gas Dalam Sel." },
  { "en": "Penyebab Baterai Kembung Adalah?", "id": "Overcharge Atau Panas Berlebih." },
  { "en": "Apa Itu Relay Thermal Overload 49?", "id": "Proteksi Panas Berdasarkan Arus." },
  { "en": "Apa Itu Relay Earth Fault 64?", "id": "Proteksi Deteksi Arus Tanah." },
  { "en": "Apa Itu Relay Over Frequency 81O?", "id": "Frekuensi Di Atas Normal." },
  { "en": "Apa Itu Relay Under Frequency 81U?", "id": "Frekuensi Di Bawah Normal." },
  { "en": "Apa Itu Relay Phase Balance 46?", "id": "Proteksi Ketidakseimbangan Arus." },
  { "en": "Apa Itu Relay Lockout 86?", "id": "Mengunci Trip Wajib Reset Manual." },
  { "en": "Apa Itu Relay Voltage Balance 60?", "id": "Keseimbangan Tegangan Antar Fasa." },
  { "en": "Apa Itu Relay Directional Power 32?", "id": "Mencegah Daya Balik Generator." },
  { "en": "Apa Itu Relay Synchrocheck 25?", "id": "Cek Syarat Sinkronisasi Paralel." },
  { "en": "Apa Itu Relay Distance 21?", "id": "Ukur Impedansi Jarak Gangguan." },
  { "en": "Apa Itu Relay Differential 87?", "id": "Bandingkan Arus Masuk Keluar." },
  { "en": "Apa Itu Zone Of Protection?", "id": "Area Kerja Relay Tertentu." },
  { "en": "Apa Itu Overlapping Zone?", "id": "Area Perlindungan Tumpang Tindih." },
  { "en": "Tujuan Overlapping Zone Adalah?", "id": "Mencegah Blind Spot Proteksi." },
  { "en": "Apa Itu PLC Scan Cycle?", "id": "Baca Input Program Tulis Output." },
  { "en": "Waktu Scan PLC Biasanya?", "id": "Beberapa Milidetik." },
  { "en": "Apa Itu Watchdog Timer PLC?", "id": "Deteksi Jika Program Macet." },
  { "en": "Apa Itu Force I/O PLC?", "id": "Memaksa Status Input Output." },
  { "en": "Apa Itu Upload Program PLC?", "id": "Ambil Program Dari PLC." },
  { "en": "Apa Itu Download Program PLC?", "id": "Kirim Program Ke PLC." },
  { "en": "Apa Itu Online Edit PLC?", "id": "Ubah Program Saat Mesin Jalan." },
  { "en": "Apa Itu Rung Comment?", "id": "Catatan Penjelas Baris Program." },
  { "en": "Apa Itu Tag Name PLC?", "id": "Nama Variabel Alamat Memori." },
  { "en": "Apa Itu Uji Hipot DC?", "id": "Uji Tegangan Tinggi Arus Searah." },
  { "en": "Apa Itu Uji Hipot AC?", "id": "Uji Tegangan Tinggi Arus Bolak Balik." },
  { "en": "Kapan Hipot DC Digunakan?", "id": "Biasanya Untuk Kabel Panjang." },
  { "en": "Kapan Hipot AC Digunakan?", "id": "Untuk Panel Dan Busbar." },
  { "en": "Apa Itu Leakage Current Hipot?", "id": "Arus Bocor Saat Uji Tegangan." },
  { "en": "Apa Itu Ramp Test Hipot?", "id": "Naikkan Tegangan Bertahap." },
  { "en": "Apa Itu Flashover Detection?", "id": "Deteksi Loncat Api Tiba Tiba." },
  { "en": "Apa Itu IGBT Latch Up?", "id": "IGBT Gagal Mati Terus On." },
  { "en": "Penyebab IGBT Latch Up?", "id": "Arus Kolektor Terlalu Besar." },
  { "en": "Apa Itu MOSFET Body Diode?", "id": "Dioda Parasit Internal MOSFET." },
  { "en": "Apa Fungsi Body Diode?", "id": "Jalur Arus Balik Freewheeling." },
  { "en": "Apa Itu Gate Charge MOSFET?", "id": "Muatan Untuk Menyalakan Gate." },
  { "en": "Satuan Gate Charge Adalah?", "id": "Nano Coulomb." },
  { "en": "Apa Itu Miller Effect MOSFET?", "id": "Kapasitansi Balik Gate Drain." },
  { "en": "Efek Miller Menyebabkan Apa?", "id": "Switching Menjadi Lambat." },
  { "en": "Apa Itu Logic Level MOSFET?", "id": "Bisa Dipicu Tegangan 5V." },
  { "en": "MOSFET Biasa Butuh Vgs Berapa?", "id": "Minimal 10 Volt." },
  { "en": "Apa Itu Depletion Mode MOSFET?", "id": "Normally On Tanpa Tegangan Gate." },
  { "en": "Apa Itu Enhancement Mode MOSFET?", "id": "Normally Off Butuh Tegangan Gate." },
  { "en": "MOSFET Paling Umum Adalah Tipe?", "id": "Enhancement Mode." },
  { "en": "Apa Itu Ferrite Core Transformer?", "id": "Trafo Inti Serbuk Besi." },
  { "en": "Ferrite Core Digunakan Pada?", "id": "Frekuensi Tinggi SMPS." },
  { "en": "Apa Itu Laminasi Inti Besi?", "id": "Plat Tipis Tumpuk." },
  { "en": "Tujuan Laminasi Inti Trafo?", "id": "Mengurangi Arus Eddy." },
  { "en": "Apa Itu Grain Oriented Steel?", "id": "Baja Silikon Arah Butiran." },
  { "en": "Keunggulan Grain Oriented Adalah?", "id": "Permeabilitas Magnet Tinggi." },
  { "en": "Apa Itu Gauge Pressure?", "id": "Tekanan Relatif Terhadap Atmosfer." },
  { "en": "Apa Itu Absolute Pressure?", "id": "Tekanan Relatif Terhadap Vakum." },
  { "en": "Apa Itu Differential Pressure (DP)?", "id": "Selisih Antara Dua Tekanan." },
  { "en": "DP Transmitter Sering Digunakan Untuk?", "id": "Mengukur Level Dan Aliran." },
  { "en": "Apa Itu Manometer U-Tube?", "id": "Alat Ukur Tekanan Pipa U." },
  { "en": "Cairan Dalam Manometer Biasanya?", "id": "Air Raksa Atau Air." },
  { "en": "Apa Itu Dead Weight Tester?", "id": "Alat Kalibrasi Tekanan Presisi." },
  { "en": "Prinsip Dead Weight Tester Adalah?", "id": "Beban Pemberat Di Atas Piston." },
  { "en": "Apa Itu Bourdon Tube C-Type?", "id": "Sensor Tekanan Pipa Melengkung." },
  { "en": "Apa Itu Diaphragm Seal?", "id": "Membran Pelindung Sensor Tekanan." },
  { "en": "Kapan Diaphragm Seal Digunakan?", "id": "Cairan Korosif Atau Kental." },
  { "en": "Apa Itu Snubber Pressure Gauge?", "id": "Peredam Getaran Jarum Tekanan." },
  { "en": "Apa Itu Siphon Tube Pigtail?", "id": "Pipa Lingkar Peredam Panas." },
  { "en": "Fungsi Siphon Tube Adalah?", "id": "Melindungi Sensor Dari Uap Panas." },
  { "en": "Apa Itu Back EMF Motor DC?", "id": "Tegangan Lawan Saat Berputar." },
  { "en": "Back EMF Sebanding Dengan?", "id": "Kecepatan Putaran Motor." },
  { "en": "Saat Motor Diam Back EMF Bernilai?", "id": "0 Volt." },
  { "en": "Apa Itu Armature Resistance?", "id": "Hambatan Gulungan Jangkar Motor." },
  { "en": "Nilai Armature Resistance Biasanya?", "id": "Sangat Kecil." },
  { "en": "Apa Itu Field Weakening Motor DC?", "id": "Melemahkan Medan Agar Cepat." },
  { "en": "Resiko Field Weakening Adalah?", "id": "Torsi Menjadi Turun." },
  { "en": "Apa Itu Commutation Sparking?", "id": "Percikan Api Di Sikat Arang." },
  { "en": "Penyebab Sparking Komutator Adalah?", "id": "Sikat Aus Atau Posisi Salah." },
  { "en": "Apa Itu Interpole Pada Motor DC?", "id": "Kutub Bantu Mencegah Percikan." },
  { "en": "Apa Itu Ward Leonard System?", "id": "Kendali Kecepatan Motor DC Kuno." },
  { "en": "Apa Itu Universal Motor?", "id": "Motor AC DC Kecepatan Tinggi." },
  { "en": "Kelemahan Motor Universal Adalah?", "id": "Suara Bising Dan Sikat Aus." },
  { "en": "Relay Buchholz Dipasang Di Pipa?", "id": "Antara Tangki Dan Konservator." },
  { "en": "Gas Di Relay Buchholz Berasal Dari?", "id": "Uraian Minyak Akibat Panas." },
  { "en": "Apa Itu Jansen Tap Changer?", "id": "Mekanisme Pindah Tap Trafo." },
  { "en": "Apa Itu Diverter Switch OLTC?", "id": "Sakelar Pemindah Beban Cepat." },
  { "en": "Apa Itu Grading Ring Isolator?", "id": "Cincin Pemerata Medan Listrik." },
  { "en": "Apa Itu Post Insulator?", "id": "Isolator Duduk Tegak." },
  { "en": "Apa Itu Suspension Insulator?", "id": "Isolator Gantung Piringan." },
  { "en": "Bahan Isolator Piringan Biasanya?", "id": "Kaca Atau Keramik." },
  { "en": "Apa Itu String Efficiency Isolator?", "id": "Distribusi Tegangan Tiap Piringan." },
  { "en": "Piringan Isolator Paling Bawah Menahan?", "id": "Tegangan Paling Tinggi." },
  { "en": "Apa Itu Standing Wave Ratio (SWR)?", "id": "Rasio Gelombang Maju Pantul." },
  { "en": "Nilai SWR Ideal Adalah?", "id": "1 Banding 1." },
  { "en": "Apa Itu Return Loss?", "id": "Daya Yang Tidak Terpantul." },
  { "en": "Apa Itu Characteristic Impedance Coax?", "id": "Impedansi Kabel 50 Ohm." },
  { "en": "Apa Itu Velocity Factor Kabel?", "id": "Kecepatan Sinyal Dalam Kabel." },
  { "en": "Velocity Factor Kabel Coax RG58?", "id": "Sekitar 0,66 Kecepatan Cahaya." },
  { "en": "Apa Itu Balun (Balanced Unbalanced)?", "id": "Penyesuai Impedansi Antena." },
  { "en": "Balun 1:1 Digunakan Untuk?", "id": "Antena Dipole Ke Coax." },
  { "en": "Balun 4:1 Digunakan Untuk?", "id": "Antena Folded Dipole." },
  { "en": "Apa Itu Gamma Match?", "id": "Sistem Matching Impedansi Antena." },
  { "en": "Apa Itu Loading Coil Antena?", "id": "Lilitan Pemendek Ukuran Antena." },
  { "en": "Apa Itu Yagi Antenna Gain?", "id": "Penguatan Sinyal Searah." },
  { "en": "Elemen Paling Belakang Yagi Disebut?", "id": "Reflector." },
  { "en": "Elemen Paling Depan Yagi Disebut?", "id": "Director." },
  { "en": "Elemen Yang Diberi Kabel Disebut?", "id": "Driven Element." },
  { "en": "Apa Itu Ground Plane Antenna?", "id": "Antena Vertikal Dengan Radial." },
  { "en": "Sudut Radial Ground Plane Standar?", "id": "45 Derajat Ke Bawah." },
  { "en": "Apa Itu Silicon Controlled Switch (SCS)?", "id": "Thyristor Dengan Dua Gate." },
  { "en": "Apa Itu Gate Turn Off (GTO)?", "id": "Thyristor Bisa Dimatikan Gate." },
  { "en": "Arus Gate Mematikan GTO Adalah?", "id": "Sangat Besar Negatif." },
  { "en": "Apa Itu Quadrac?", "id": "Triac Dan Diac Terintegrasi." },
  { "en": "Apa Itu Sidac?", "id": "Dioda Sakelar Tegangan Tinggi." },
  { "en": "Sidac Sering Digunakan Untuk?", "id": "Pemicu Lampu Strobo." },
  { "en": "Apa Itu Tunnel Diode?", "id": "Dioda Resistansi Negatif." },
  { "en": "Aplikasi Tunnel Diode Adalah?", "id": "Osilator Gelombang Mikro." },
  { "en": "Apa Itu Gunn Diode?", "id": "Dioda Osilator Microwave." },
  { "en": "Apa Itu PIN Diode?", "id": "Dioda Sakelar Frekuensi Tinggi." },
  { "en": "Apa Itu Step Recovery Diode?", "id": "Pembangkit Pulsa Sangat Cepat." },
  { "en": "Apa Itu Varicap Diode?", "id": "Kapasitor Variabel Tegangan." },
  { "en": "Aplikasi Varicap Adalah?", "id": "Tuning Radio Digital." },
  { "en": "Apa Itu Phototransistor?", "id": "Transistor Peka Cahaya." },
  { "en": "Sensitivitas Phototransistor Dibanding Photodiode?", "id": "Phototransistor Lebih Sensitif." },
  { "en": "Kecepatan Photodiode Dibanding Phototransistor?", "id": "Photodiode Lebih Cepat." },
  { "en": "Apa Itu Opto Triac Zero Cross?", "id": "Pemicu Saat Tegangan Nol." },
  { "en": "Apa Itu Opto Triac Random Phase?", "id": "Pemicu Sembarang Waktu." },
  { "en": "Random Phase Opto Digunakan Untuk?", "id": "Dimmer Lampu." },
  { "en": "Zero Cross Opto Digunakan Untuk?", "id": "Sakelar Solid State Relay." },
  { "en": "Apa Itu Thermopile?", "id": "Susunan Seri Thermocouple." },
  { "en": "Thermopile Digunakan Pada?", "id": "Sensor Suhu Inframerah." },
  { "en": "Apa Itu Bolometer?", "id": "Sensor Daya Gelombang Mikro." },
  { "en": "Apa Itu Strain Gauge Rosette?", "id": "Susunan Strain Gauge Berbagai Arah." },
  { "en": "Apa Itu Dummy Gauge?", "id": "Kompensasi Suhu Strain Gauge." },
  { "en": "Apa Itu Wheatstone Bridge Quarter?", "id": "Menggunakan 1 Strain Gauge." },
  { "en": "Apa Itu Wheatstone Bridge Half?", "id": "Menggunakan 2 Strain Gauge." },
  { "en": "Apa Itu Wheatstone Bridge Full?", "id": "Menggunakan 4 Strain Gauge." },
  { "en": "Mana Paling Sensitif Quarter Atau Full?", "id": "Full Bridge Paling Sensitif." },
  { "en": "Apa Itu Load Cell S-Type?", "id": "Sensor Berat Tarik Tekan." },
  { "en": "Apa Itu Load Cell Shear Beam?", "id": "Sensor Berat Lantai." },
  { "en": "Apa Itu Load Cell Single Point?", "id": "Sensor Timbangan Duduk." },
  { "en": "Apa Itu Excitation Voltage Load Cell?", "id": "Tegangan Suplai Jembatan Sensor." },
  { "en": "Nilai Excitation Voltage Biasanya?", "id": "5 Volt Sampai 10 Volt." },
  { "en": "Apa Itu mV/V Rating Load Cell?", "id": "Sensitivitas Output Per Volt." },
  { "en": "Jika Rating 2mV/V Suplai 10V Maka?", "id": "Output Maksimal 20 miliVolt." },
  { "en": "Apa Itu Creep Error Load Cell?", "id": "Perubahan Nilai Saat Beban Diam." },
  { "en": "Apa Itu Tara (Tare) Timbangan?", "id": "Mengenolkan Berat Wadah." },
  { "en": "Apa Kepanjangan HASL (Hot Air Solder Leveling)?", "id": "Perataan Timah Udara Panas." },
  { "en": "Apa Itu ENIG (Electroless Nickel Immersion Gold)?", "id": "Pelapis Akhir PCB Emas." },
  { "en": "Kelebihan ENIG Dibanding HASL Adalah?", "id": "Permukaan Lebih Rata Dan Awet." },
  { "en": "Apa Itu OSP (Organic Solderability Preservative)?", "id": "Pelapis Anti Karat Organik Tipis." },
  { "en": "Apa Itu Via Tenting Pada PCB?", "id": "Via Tertutup Solder Mask." },
  { "en": "Apa Itu Via Plugging?", "id": "Via Diisi Penuh Resin." },
  { "en": "Apa Itu Via In Pad?", "id": "Lubang Via Di Telapak Komponen." },
  { "en": "Teknologi Via In Pad Wajib Untuk?", "id": "Komponen BGA Kaki Rapat." },
  { "en": "Apa Itu Mouse Bites PCB?", "id": "Lubang Kecil Pemisah Panel." },
  { "en": "Apa Itu PCB Stiffener?", "id": "Penguat Mekanik PCB Fleksibel." },
  { "en": "Apa Itu Flex PCB?", "id": "PCB Bahan Plastik Lentur." },
  { "en": "Apa Itu Rigid Flex PCB?", "id": "Gabungan Papan Kaku Dan Lentur." },
  { "en": "Apa Itu Impedance Control PCB?", "id": "Mengatur Lebar Jalur Sesuai Impedansi." },
  { "en": "Kapan Impedance Control Diperlukan?", "id": "Sinyal Frekuensi Tinggi RF." },
  { "en": "Apa Itu Rogers PCB Material?", "id": "Bahan PCB Frekuensi Tinggi." },
  { "en": "Apa Itu Aluminium PCB?", "id": "PCB Dengan Pendingin Aluminium." },
  { "en": "Aluminium PCB Banyak Digunakan Untuk?", "id": "Lampu LED High Power." },
  { "en": "Apa Itu Blind Buried Vias?", "id": "Via Tidak Tembus Seluruh Lapisan." },
  { "en": "Apa Itu Annular Ring PCB?", "id": "Cincin Tembaga Sekeliling Lubang." },
  { "en": "Apa Itu Drill Hit?", "id": "Titik Bor Pada PCB." },
  { "en": "Apa Itu Polarization Antena Vertikal?", "id": "Gelombang Tegak Lurus Bumi." },
  { "en": "Apa Itu Polarization Antena Horizontal?", "id": "Gelombang Sejajar Dengan Bumi." },
  { "en": "Apa Itu Circular Polarization?", "id": "Gelombang Berputar Spiral." },
  { "en": "Kapan Menggunakan Circular Polarization?", "id": "Komunikasi Satelit Dan Drone." },
  { "en": "Apa Itu LHCP (Left Hand Circular Polarization)?", "id": "Polarisasi Putar Kiri." },
  { "en": "Apa Itu RHCP (Right Hand Circular Polarization)?", "id": "Polarisasi Putar Kanan." },
  { "en": "Apa Itu Fresnel Zone?", "id": "Area Ellips Jalur Sinyal Radio." },
  { "en": "Hambatan Di Fresnel Zone Menyebabkan?", "id": "Pelemahan Sinyal Multipath." },
  { "en": "Minimal Area Fresnel Zone Bersih Adalah?", "id": "60 Persen." },
  { "en": "Apa Itu Isotropic Radiator?", "id": "Antena Ideal Memancar Segala Arah." },
  { "en": "Gain Antena Isotropik Adalah?", "id": "0 dBi." },
  { "en": "Apa Itu Front To Back Ratio?", "id": "Perbandingan Sinyal Depan Belakang." },
  { "en": "Apa Itu Sidelobe Antena?", "id": "Pancaran Samping Yang Tidak Diinginkan." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Banyak Antena Kirim Terima." },
  { "en": "Teknologi MIMO Meningkatkan Apa?", "id": "Kecepatan Dan Kapasitas Data." },
  { "en": "Apa Itu Diversity Antenna?", "id": "Dua Antena Memilih Sinyal Terbaik." },
  { "en": "Apa Itu Beamforming?", "id": "Mengarahkan Sinyal Ke Pengguna." },
  { "en": "Apa Itu Phased Array Antenna?", "id": "Antena Dengan Pengarah Elektronik." },
  { "en": "Apa Itu VSWR Meter?", "id": "Alat Ukur Pantulan Sinyal." },
  { "en": "Jarum VSWR Di Angka 1 Artinya?", "id": "Sangat Sempurna Tanpa Pantulan." },
  { "en": "Jarum VSWR Di Angka 3 Artinya?", "id": "Buruk Banyak Pantulan." },
  { "en": "Apa Itu Antenna Tuner?", "id": "Alat Penyesuai Impedansi Antena." },
  { "en": "Apa Itu Coaxial Switch?", "id": "Sakelar Pemindah Antena." },
  { "en": "Apa Itu Dummy Load RF?", "id": "Resistor Beban Pengganti Antena." },
  { "en": "Impedansi Dummy Load Standar Adalah?", "id": "50 Ohm." },
  { "en": "Kode ANSI Relay 24 Adalah?", "id": "Volts Per Hertz Overexcitation." },
  { "en": "Kode ANSI Relay 32 Adalah?", "id": "Reverse Power Daya Balik." },
  { "en": "Kode ANSI Relay 40 Adalah?", "id": "Loss Of Field Hilang Medan." },
  { "en": "Kode ANSI Relay 51V Adalah?", "id": "Voltage Controlled Overcurrent." },
  { "en": "Kode ANSI Relay 64R Adalah?", "id": "Rotor Earth Fault." },
  { "en": "Kode ANSI Relay 87G Adalah?", "id": "Generator Differential Relay." },
  { "en": "Kode ANSI Relay 87T Adalah?", "id": "Transformer Differential Relay." },
  { "en": "Apa Itu Negative Sequence Current?", "id": "Arus Akibat Beban Tidak Seimbang." },
  { "en": "Negative Sequence Memanaskan Bagian?", "id": "Rotor Generator." },
  { "en": "Apa Itu Inadvertent Energization?", "id": "Generator Mati Tiba Tiba Bertegangan." },
  { "en": "Apa Itu Pole Slipping?", "id": "Rotor Hilang Sinkronisasi Dengan Stator." },
  { "en": "Apa Itu Shaft Current Protection?", "id": "Proteksi Arus Bocor Poros." },
  { "en": "Apa Itu Vibration Protection?", "id": "Proteksi Getaran Berlebih Mesin." },
  { "en": "Apa Itu Flip Flop JK?", "id": "Flip Flop Universal." },
  { "en": "Jika J=1 Dan K=1 Outputnya?", "id": "Toggle Berubah Nilai." },
  { "en": "Jika J=0 Dan K=0 Outputnya?", "id": "Hold Menahan Nilai." },
  { "en": "Apa Itu Flip Flop D (Data)?", "id": "Menyimpan Data Sesuai Input." },
  { "en": "Apa Itu Flip Flop T (Toggle)?", "id": "Berubah Nilai Setiap Clock." },
  { "en": "Apa Itu Master Slave Flip Flop?", "id": "Dua Flip Flop Bertingkat." },
  { "en": "Apa Itu Edge Triggered?", "id": "Aktif Saat Tepi Sinyal." },
  { "en": "Apa Itu Level Triggered?", "id": "Aktif Saat Level Tegangan." },
  { "en": "Apa Itu Asynchronous Counter?", "id": "Output Memicu Clock Berikutnya." },
  { "en": "Kelemahan Asynchronous Counter Adalah?", "id": "Waktu Tunda Akumulatif Propagation." },
  { "en": "Apa Itu Synchronous Counter?", "id": "Semua Flip Flop Clock Bersamaan." },
  { "en": "Apa Itu Ring Counter?", "id": "Data Bergeser Memutar Lingkaran." },
  { "en": "Apa Itu Johnson Counter?", "id": "Ring Counter Dengan Inversi." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "Chip Logika Bisa Diprogram Ulang." },
  { "en": "Bahasa Pemrograman FPGA Adalah?", "id": "VHDL Atau Verilog." },
  { "en": "Apa Itu CPLD?", "id": "Complex Programmable Logic Device." },
  { "en": "Apa Itu ASIC?", "id": "Application Specific Integrated Circuit." },
  { "en": "Bedanya FPGA Dan ASIC?", "id": "ASIC Tidak Bisa Diprogram Ulang." },
  { "en": "Apa Itu LUT (Look Up Table)?", "id": "Blok Logika Dasar FPGA." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Mikroprosesor Khusus Matematika Sinyal." },
  { "en": "Kelebihan DSP Adalah?", "id": "Cepat Menghitung Perkalian Penjumlahan." },
  { "en": "Apa Itu MAC Instruction?", "id": "Multiply Accumulate." },
  { "en": "Apa Itu Floating Point Unit (FPU)?", "id": "Unit Hitung Bilangan Koma." },
  { "en": "Apa Itu Fixed Point Arithmetic?", "id": "Hitungan Bilangan Bulat Cepat." },
  { "en": "Apa Itu Pipeline Processing?", "id": "Memproses Banyak Instruksi Bersamaan." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Transfer Memori Tanpa CPU." },
  { "en": "Apa Itu Interrupt Vector Table?", "id": "Daftar Alamat Fungsi Interupsi." },
  { "en": "Apa Itu Stack Pointer?", "id": "Penunjuk Memori Tumpukan Data." },
  { "en": "Apa Itu Heap Memory?", "id": "Memori Alokasi Dinamis." },
  { "en": "Apa Itu Firmware Over The Air (FOTA)?", "id": "Update Software Nirkabel." },
  { "en": "Apa Itu Bootloader Arduino?", "id": "Program Awal Upload Sketch." },
  { "en": "Apa Itu Fuse Bit AVR?", "id": "Konfigurasi Hardware Mikrokontroler." },
  { "en": "Salah Setting Fuse Bit Menyebabkan?", "id": "Chip Terkunci Atau Mati." },
  { "en": "Apa Itu Brown Out Detection (BOD)?", "id": "Reset Saat Tegangan Drop." },
  { "en": "Apa Itu JTAG Interface?", "id": "Port Debugging Dan Testing." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Debugging ARM Dengan 2 Kabel." },
  { "en": "Kabel SWD Terdiri Dari?", "id": "SWDIO Dan SWCLK." },
  { "en": "Apa Itu VSWR Sempurna?", "id": "Nilai 1 Banding 1." },
  { "en": "Apa Itu Return Loss Tinggi?", "id": "Kualitas Matching Bagus." },
  { "en": "Apa Itu Insertion Loss?", "id": "Rugi Daya Lewat Komponen." },
  { "en": "Apa Itu Mismatch Loss?", "id": "Rugi Akibat Impedansi Beda." },
  { "en": "Apa Itu Reflection Coefficient?", "id": "Koefisien Pantulan Sinyal." },
  { "en": "Nilai Impedansi Karakteristik Vakum?", "id": "377 Ohm." },
  { "en": "Apa Itu Smith Chart Tengah?", "id": "Impedansi Matching 50 Ohm." },
  { "en": "Apa Itu Quarter Wave Transformer?", "id": "Penyesuai Impedansi 1 Per 4 Gelombang." },
  { "en": "Apa Itu Stub Tuning?", "id": "Potongan Kabel Penyesuai Impedansi." },
  { "en": "Apa Itu Skin Depth Tembaga?", "id": "Sangat Tipis Di Frekuensi Tinggi." },
  { "en": "Apa Itu Konektor Type 1 EV?", "id": "Konektor J1772 5 Pin." },
  { "en": "Apa Itu Konektor Type 2 EV?", "id": "Konektor Mennekes 7 Pin." },
  { "en": "Apa Itu Konektor CCS 2?", "id": "Combo AC Dan DC Eropa." },
  { "en": "Apa Itu Konektor CHAdeMO?", "id": "DC Fast Charging Jepang." },
  { "en": "Apa Itu Konektor GB/T?", "id": "Standar Charging China." },
  { "en": "Apa Fungsi Pin Control Pilot?", "id": "Komunikasi Data Mobil Charger." },
  { "en": "Apa Fungsi Pin Proximity Pilot?", "id": "Deteksi Kabel Tercolok Aman." },
  { "en": "Sinyal Control Pilot Berupa?", "id": "Gelombang Kotak PWM 1 kHz." },
  { "en": "Apa Itu V2L (Vehicle To Load)?", "id": "Mobil Jadi Power Bank AC." },
  { "en": "Apa Itu V2H (Vehicle To Home)?", "id": "Mobil Suplai Listrik Rumah." },
  { "en": "Apa Itu Osilator Kristal (XTAL)?", "id": "Pembangkit Frekuensi Efek Piezoelektrik." },
  { "en": "Apa Itu Load Capacitance Kristal?", "id": "Kapasitor Beban Agar Stabil." },
  { "en": "Satuan Akurasi Kristal Adalah?", "id": "PPM Parts Per Million." },
  { "en": "Apa Itu TCXO?", "id": "Osilator Kompensasi Suhu." },
  { "en": "Apa Itu VCXO?", "id": "Osilator Kendali Tegangan." },
  { "en": "Apa Itu OCXO?", "id": "Osilator Oven Sangat Stabil." },
  { "en": "Apa Itu MEMS Oscillator?", "id": "Osilator Chip Silikon Mikro." },
  { "en": "Apa Itu Ceramic Resonator?", "id": "Pengganti Kristal Murah." },
  { "en": "Akurasi Ceramic Resonator Adalah?", "id": "Lebih Rendah Dari Kristal." },
  { "en": "Apa Itu RTC DS3231?", "id": "Jam Digital TCXO Akurat." },
  { "en": "Apa Itu EtherNet/IP?", "id": "Industrial Protocol Over Ethernet." },
  { "en": "Apa Itu CIP (Common Industrial Protocol)?", "id": "Standar Data Rockwell Automation." },
  { "en": "Apa Itu PROFINET RT?", "id": "Real Time Communication." },
  { "en": "Apa Itu PROFINET IRT?", "id": "Isochronous Real Time." },
  { "en": "Kabel Ethernet Industri Harus?", "id": "Shielded Dan Tahan Noise." },
  { "en": "Konektor Ethernet Industri M12?", "id": "Konektor Bulat Tahan Air." },
  { "en": "Apa Itu DLR (Device Level Ring)?", "id": "Topologi Ring Redundan Ethernet." },
  { "en": "Apa Itu Stratix Switch?", "id": "Switch Managed Allen Bradley." },
  { "en": "Apa Itu Multicast Traffic?", "id": "Satu Pengirim Banyak Penerima." },
  { "en": "Apa Itu IGMP Snooping?", "id": "Mengatur Lalu Lintas Multicast." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Tegangan Ujung Terima Naik." },
  { "en": "Penyebab Efek Ferranti Adalah?", "id": "Kapasitansi Saluran Beban Ringan." },
  { "en": "Cara Mengatasi Efek Ferranti?", "id": "Pasang Shunt Reactor." },
  { "en": "Apa Itu Shunt Capacitor?", "id": "Menaikkan Tegangan Drop." },
  { "en": "Apa Itu Series Capacitor?", "id": "Mengurangi Reaktansi Saluran." },
  { "en": "Apa Itu Bundle Conductor?", "id": "Beberapa Kabel Per Fasa." },
  { "en": "Manfaat Bundle Conductor Adalah?", "id": "Mengurangi Corona Dan Induktansi." },
  { "en": "Apa Itu Transposisi SUTET?", "id": "Menukar Posisi Kabel Fasa." },
  { "en": "Tujuan Transposisi Adalah?", "id": "Menyeimbangkan Impedansi Fasa." },
  { "en": "Apa Itu Spacer Damper?", "id": "Pemisah Kabel Dan Peredam." },
  { "en": "Kode ANSI 50 Adalah?", "id": "Instantaneous Overcurrent Relay." },
  { "en": "Kode ANSI 51 Adalah?", "id": "Time Delay Overcurrent Relay." },
  { "en": "Kode ANSI 50N Adalah?", "id": "Instantaneous Earth Fault." },
  { "en": "Kode ANSI 51N Adalah?", "id": "Time Delay Earth Fault." },
  { "en": "Kode ANSI 27 Adalah?", "id": "Under Voltage Relay." },
  { "en": "Kode ANSI 59 Adalah?", "id": "Over Voltage Relay." },
  { "en": "Kode ANSI 81 Adalah?", "id": "Frequency Relay." },
  { "en": "Kode ANSI 87 Adalah?", "id": "Differential Protection Relay." },
  { "en": "Kode ANSI 63 Adalah?", "id": "Pressure Switch Buchholz." },
  { "en": "Kode ANSI 96 Adalah?", "id": "Buchholz Relay Trip." },
  { "en": "Apa Itu Hot Spot Panel Surya?", "id": "Sel Panas Akibat Bayangan." },
  { "en": "Penyebab Hot Spot Adalah?", "id": "Arus Terhambat Di Sel Gelap." },
  { "en": "Fungsi Bypass Diode Adalah?", "id": "Mengalirkan Arus Lewat Sel Gelap." },
  { "en": "Letak Bypass Diode Ada Di?", "id": "Junction Box Panel Surya." },
  { "en": "Fungsi Blocking Diode Adalah?", "id": "Cegah Arus Balik Malam Hari." },
  { "en": "Apa Itu MPPT Tracking Efficiency?", "id": "Kemampuan Ikuti Perubahan Cuaca." },
  { "en": "Nilai Efisiensi MPPT Bagus?", "id": "Di Atas 99 Persen." },
  { "en": "Apa Itu String Inverter?", "id": "Inverter Untuk Seri Panel Banyak." },
  { "en": "Apa Itu Micro Inverter?", "id": "Inverter Untuk Satu Panel." },
  { "en": "Keunggulan Micro Inverter Adalah?", "id": "Tidak Terpengaruh Bayangan Panel Lain." },
  { "en": "Berapa Nilai Resistor Pull Up I2C?", "id": "4,7k Sampai 10k Ohm." },
  { "en": "Berapa Nilai Kapasitor Decoupling IC?", "id": "100 Nano Farad." },
  { "en": "Berapa Nilai Kapasitor Kristal Arduino?", "id": "22 Pico Farad." },
  { "en": "Apa Itu LDO Regulator?", "id": "Low Dropout Linear Regulator." },
  { "en": "Apa Itu AMS1117-3.3?", "id": "LDO Regulator 3,3 Volt." },
  { "en": "Apa Itu LM7805 CV?", "id": "Regulator 5 Volt 1,5 Ampere." },
  { "en": "Apa Itu TO-220 Package?", "id": "Kemasan Transistor Pendingin Logam." },
  { "en": "Apa Itu TO-92 Package?", "id": "Kemasan Transistor Plastik Kecil." },
  { "en": "Apa Itu SMD 0805?", "id": "Ukuran Komponen Tempel Sedang." },
  { "en": "Apa Itu SMD 0603?", "id": "Ukuran Komponen Tempel Kecil." },
  { "en": "Apa Itu SMD 0402?", "id": "Ukuran Komponen Tempel Sangat Kecil." },
  { "en": "Slip Pada Motor Sinkron Adalah?", "id": "0 Persen." },
  { "en": "Slip Pada Motor Induksi Normal?", "id": "2 Sampai 5 Persen." },
  { "en": "Apa Itu Motor Reluktansi?", "id": "Rotor Besi Tanpa Magnet." },
  { "en": "Apa Itu Switched Reluctance Motor?", "id": "Motor Torsi Tinggi Tanpa Magnet." },
  { "en": "Keunggulan Motor Reluktansi Adalah?", "id": "Murah Dan Tahan Panas." },
  { "en": "Apa Itu Motor Histeresis?", "id": "Motor Sinkron Rotor Baja Keras." },
  { "en": "Aplikasi Motor Histeresis Adalah?", "id": "Pemutar Piringan Hitam Jadul." },
  { "en": "LOTO Warna Merah Artinya?", "id": "Gembok Bahaya Personal." },
  { "en": "LOTO Warna Kuning Artinya?", "id": "Gembok Transisi Shift." },
  { "en": "LOTO Warna Biru Artinya?", "id": "Gembok Supervisor Grup." },
  { "en": "Apa Itu Arc Flash Boundary?", "id": "Batas Jarak Bahaya Luka Bakar." },
  { "en": "Apa Itu Incident Energy?", "id": "Energi Panas Ledakan Listrik." },
  { "en": "Apa Itu Cal/cm2?", "id": "Satuan Energi Panas Arc Flash." },
  { "en": "Kategori APD Level 1 Tahan?", "id": "4 Kalori Per cm Persegi." },
  { "en": "Kategori APD Level 4 Tahan?", "id": "40 Kalori Per cm Persegi." },
  { "en": "APD Level 4 Biasanya Berbentuk?", "id": "Baju Astronot Lengkap." }



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
