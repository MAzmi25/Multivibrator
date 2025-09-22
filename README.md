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


    { "en": "Apa Itu Multivibrator?", "id": "Sirkuit Elektronik Penghasil Gelombang Non-Sinusoidal." },
    { "en": "Apa Tiga Jenis Dasar Multivibrator?", "id": "Astabil, Monostabil, Dan Bistabil." },
    { "en": "Bagaimana Karakteristik Multivibrator Astabil?", "id": "Tidak Memiliki Keadaan Stabil Sama Sekali." },
    { "en": "Apa Yang Dihasilkan Oleh Multivibrator Astabil?", "id": "Gelombang Kotak Atau Pulsa Berkelanjutan." },
    { "en": "Nama Lain Multivibrator Astabil?", "id": "Free-Running Multivibrator." },
    { "en": "Bagaimana Karakteristik Multivibrator Monostabil?", "id": "Memiliki Satu Keadaan Stabil." },
    { "en": "Berapa Banyak Keadaan Kuasi-Stabil Yang Dimiliki Monostabil?", "id": "Satu Keadaan Kuasi-Stabil." },
    { "en": "Apa Yang Dibutuhkan Monostabil Untuk Berubah Keadaan?", "id": "Sebuah Sinyal Pemicu Eksternal." },
    { "en": "Nama Lain Multivibrator Monostabil?", "id": "One-Shot Multivibrator." },
    { "en": "Bagaimana Karakteristik Multivibrator Bistabil?", "id": "Memiliki Dua Keadaan Stabil." },
    { "en": "Bagaimana Bistabil Berpindah Antar Keadaan?", "id": "Membutuhkan Sinyal Pemicu Eksternal." },
    { "en": "Nama Lain Multivibrator Bistabil?", "id": "Flip-Flop." },
    { "en": "Apa Komponen Utama Dalam Multivibrator Klasik?", "id": "Transistor, Resistor, Dan Kapasitor." },
    { "en": "Apa Peran Kapasitor Dalam Multivibrator?", "id": "Sebagai Elemen Pewaktu (Timing Element)." },
    { "en": "Apa Peran Resistor Dalam Multivibrator?", "id": "Mengontrol Laju Pengisian Dan Pengosongan Kapasitor." },
    { "en": "Apa Itu Keadaan Stabil?", "id": "Keadaan Di Mana Sirkuit Tetap Tanpa Pemicu." },
    { "en": "Apa Itu Keadaan Kuasi-Stabil?", "id": "Keadaan Sementara Sebelum Kembali Ke Keadaan Stabil." },
    { "en": "Multivibrator Astabil Digunakan Sebagai Apa?", "id": "Osilator, Penghasil Clock, Atau Timer." },
    { "en": "Multivibrator Monostabil Digunakan Sebagai Apa?", "id": "Penghasil Pulsa Tunggal, Timer, Atau Pembentuk Pulsa." },
    { "en": "Multivibrator Bistabil Digunakan Sebagai Apa?", "id": "Elemen Memori, Pencacah (Counter), Atau Pembagi Frekuensi." },
    { "en": "Apa Yang Menentukan Frekuensi Osilasi Astabil?", "id": "Nilai Resistor Dan Kapasitor (RC)." },
    { "en": "Apa Yang Menentukan Durasi Pulsa Monostabil?", "id": "Nilai Resistor Dan Kapasitor (RC)." },
    { "en": "Berapa Banyak Pemicu Yang Dibutuhkan Bistabil Untuk Satu Siklus?", "id": "Dua Sinyal Pemicu." },
    { "en": "Apa Itu Umpan Balik Positif?", "id": "Sebagian Output Dikembalikan Untuk Memperkuat Perubahan." },
    { "en": "Mekanisme Apa Yang Mendasari Operasi Multivibrator?", "id": "Umpan Balik Positif." },
    { "en": "Apa Yang Terjadi Pada Transistor Dalam Multivibrator?", "id": "Bekerja Sebagai Saklar (Cut-off Dan Saturasi)." },
    { "en": "Apa Itu Keadaan Cut-off Transistor?", "id": "Transistor Mati, Tidak Ada Arus Kolektor." },
    { "en": "Apa Itu Keadaan Saturasi Transistor?", "id": "Transistor Hidup Penuh, Arus Kolektor Maksimal." },
    { "en": "Multivibrator Adalah Kelas Osilator Apa?", "id": "Osilator Relaksasi." },
    { "en": "Mengapa Disebut Osilator Relaksasi?", "id": "Bentuk Gelombangnya Dihasilkan Oleh Pengisian/Pengosongan RC." },
    { "en": "Sirkuit Terpadu (IC) Populer Untuk Multivibrator?", "id": "IC 555 Timer." },
    { "en": "IC 555 Bisa Dikonfigurasi Sebagai Multivibrator Apa Saja?", "id": "Astabil Dan Monostabil." },
    { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Rasio Waktu 'ON' Terhadap Total Periode." },
    { "en": "Bagaimana Siklus Kerja Astabil Berbasis Transistor Simetris?", "id": "Lima Puluh Persen (50%)." },
    { "en": "Bisakah Siklus Kerja Astabil IC 555 Kurang Dari 50%?", "id": "Tidak Secara Langsung, Perlu Modifikasi." },
    { "en": "Apa Itu Pemicu (Trigger)?", "id": "Sinyal Yang Memulai Perubahan Keadaan." },
    { "en": "Apa Sifat Pemicu Untuk Monostabil?", "id": "Pulsa Sesaat." },
    { "en": "Berapa Banyak Output Yang Dimiliki Flip-Flop?", "id": "Dua Output (Q Dan Q-bar)." },
    { "en": "Bagaimana Hubungan Antara Output Q Dan Q-bar?", "id": "Selalu Dalam Keadaan Logika Berlawanan." },
    { "en": "Apa Itu 'State' Dalam Konteks Digital?", "id": "Tingkat Tegangan Output (Tinggi Atau Rendah)." },
    { "en": "Multivibrator Menghasilkan Gelombang Analog Atau Digital?", "id": "Umumnya Dianggap Sinyal Digital." },
    { "en": "Apa Bentuk Gelombang Output Ideal Multivibrator?", "id": "Gelombang Kotak Sempurna." },
    { "en": "Mengapa Gelombang Output Praktis Tidak Sempurna?", "id": "Karena Waktu Naik (Rise Time) Dan Turun (Fall Time)." },
    { "en": "Apa Yang Mempengaruhi Waktu Naik Dan Turun?", "id": "Kapasitansi Parasitik Dalam Rangkaian." },
    { "en": "Apa Itu 'Commutating Capacitors'?", "id": "Kapasitor Kopling Dalam Multivibrator Transistor." },
    { "en": "Apa Fungsi Kapasitor Kopling?", "id": "Menyampaikan Perubahan Cepat Dari Satu Sisi Ke Sisi Lain." },
    { "en": "Rangkaian Multivibrator Berbasis Op-Amp Menggunakan Apa?", "id": "Op-Amp Sebagai Komparator." },
    { "en": "Apa Keuntungan Multivibrator Berbasis Op-Amp?", "id": "Desain Lebih Mudah, Kinerja Lebih Baik." },
    { "en": "Apa Itu Histeresis Dalam Konteks Komparator?", "id": "Perbedaan Antara Tegangan Ambang Atas Dan Bawah." },
    { "en": "Pemicu Schmitt Adalah Jenis Multivibrator Apa?", "id": "Multivibrator Bistabil." },
    { "en": "Apa Fungsi Pemicu Schmitt?", "id": "Mengubah Sinyal Analog Berisik Menjadi Digital Bersih." },
    { "en": "Apa Itu 'Noise Immunity'?", "id": "Kemampuan Sirkuit Untuk Menolak Gangguan Derau." },
    { "en": "Bagaimana Histeresis Meningkatkan 'Noise Immunity'?", "id": "Mencegah Pemicuan Palsu Akibat Derau Kecil." },
    { "en": "Apa Komponen Yang Menciptakan Umpan Balik Positif Di Pemicu Schmitt?", "id": "Jaringan Pembagi Tegangan Ke Input Non-Pembalik." },
    { "en": "Apa Itu 'Upper Threshold Voltage' (V_UT)?", "id": "Tegangan Ambang Untuk Transisi Output Ke Rendah." },
    { "en": "Apa Itu 'Lower Threshold Voltage' (V_LT)?", "id": "Tegangan Ambang Untuk Transisi Output Ke Tinggi." },
    { "en": "Lebar Histeresis Dihitung Bagaimana?", "id": "V_UT Dikurangi V_LT." },
    { "en": "Dalam Astabil, Kapasitor Diisi Melalui Apa?", "id": "Resistor Pewaktu." },
    { "en": "Dalam Astabil, Kapasitor Dikosongkan Melalui Apa?", "id": "Resistor Pewaktu Yang Berbeda Atau Sama." },
    { "en": "Tegangan Kapasitor Di Astabil Berosilasi Antara Apa?", "id": "Dua Tingkat Tegangan Ambang." },
    { "en": "Apa Itu Periode (T) Gelombang?", "id": "Waktu Untuk Menyelesaikan Satu Siklus Penuh." },
    { "en": "Apa Itu Frekuensi (f) Gelombang?", "id": "Jumlah Siklus Per Detik (f = 1/T)." },
    { "en": "Apa Satuan Frekuensi?", "id": "Hertz (Hz)." },
    { "en": "Bagaimana Mengubah Frekuensi Multivibrator Astabil?", "id": "Mengubah Nilai R Atau C." },
    { "en": "Bagaimana Mengubah Durasi Pulsa Monostabil?", "id": "Mengubah Nilai R Atau C." },
    { "en": "Pulsa Output Monostabil Berakhir Ketika Apa?", "id": "Ketika Tegangan Kapasitor Mencapai Ambang Batas." },
    { "en": "Setelah Pulsa Berakhir, Monostabil Kembali Ke Keadaan Apa?", "id": "Kembali Ke Keadaan Stabilnya." },
    { "en": "Apakah Monostabil Bisa Dipicu Kembali Saat Pulsa Aktif?", "id": "Tergantung Jenisnya (Retriggerable Atau Non-Retriggerable)." },
    { "en": "Apa Itu 'Retriggerable Monostable'?", "id": "Durasi Pulsa Diperpanjang Jika Dipicu Kembali." },
    { "en": "Apa Itu 'Edge Triggering'?", "id": "Perubahan Keadaan Dipicu Oleh Sisi Naik/Turun Pulsa." },
    { "en": "Apa Itu 'Level Triggering'?", "id": "Perubahan Keadaan Terjadi Selama Level Tegangan Aktif." },
    { "en": "Flip-Flop Adalah Blok Bangunan Dasar Dari Apa?", "id": "Sirkuit Logika Sekuensial." },
    { "en": "Contoh Sirkuit Logika Sekuensial?", "id": "Register, Pencacah, Dan Memori." },
    { "en": "Apa Beda Logika Kombinasional Dan Sekuensial?", "id": "Sekuensial Memiliki Memori, Kombinasional Tidak." },
    { "en": "Flip-Flop Tipe Apa Yang Paling Dasar?", "id": "SR Latch (Set-Reset)." },
    { "en": "Apa Kondisi Terlarang Pada SR Latch?", "id": "Ketika Input S Dan R Keduanya Tinggi." },
    { "en": "Flip-Flop Apa Yang Mengatasi Masalah SR Latch?", "id": "JK Flip-Flop Atau D Flip-Flop." },
    { "en": "Bagaimana Perilaku JK Flip-Flop Saat J Dan K Tinggi?", "id": "Keadaan Outputnya Berubah (Toggle)." },
    { "en": "Apa Fungsi D Flip-Flop?", "id": "Menyimpan Data Satu Bit (Data Latch)." },
    { "en": "Apa Fungsi T Flip-Flop?", "id": "Sebagai Pembagi Frekuensi Dua (Toggle)." },
    { "en": "Apa Itu 'Clock' Dalam Sirkuit Digital?", "id": "Sinyal Pulsa Untuk Sinkronisasi Operasi." },
    { "en": "Sinyal Clock Dihasilkan Oleh Apa?", "id": "Multivibrator Astabil." },
    { "en": "Multivibrator Termasuk Sirkuit Linear Atau Non-Linear?", "id": "Sirkuit Non-Linear." },
    { "en": "Mengapa Disebut Non-Linear?", "id": "Karena Menggunakan Komponen Yang Bekerja Di Daerah Non-Linear." },
    { "en": "Apa Itu 'Rise Time'?", "id": "Waktu Yang Dibutuhkan Sinyal Naik Dari 10% Ke 90%." },
    { "en": "Apa Itu 'Fall Time'?", "id": "Waktu Yang Dibutuhkan Sinyal Turun Dari 90% Ke 10%." },
    { "en": "'Rise Time' Dan 'Fall Time' Yang Baik Bagaimana?", "id": "Secepat Mungkin (Sangat Kecil)." },
    { "en": "Apa Yang Terjadi Jika Umpan Balik Positif Terlalu Kuat?", "id": "Menghasilkan Osilasi Yang Stabil." },
    { "en": "Apa Peran Umpan Balik Negatif Dalam Osilator?", "id": "Untuk Menstabilkan Amplitudo Osilasi." },
    { "en": "Apakah Multivibrator Astabil Membutuhkan Umpan Balik Negatif?", "id": "Tidak, Bekerja Berdasarkan Umpan Balik Positif." },
    { "en": "Apa Itu 'Collector-Coupled Multivibrator'?", "id": "Multivibrator Transistor Klasik." },
    { "en": "Apa Yang Terjadi Pada Tegangan Kolektor Saat Transistor 'ON'?", "id": "Tegangan Kolektor Menjadi Rendah ( mendekati nol)." },
    { "en": "Apa Yang Terjadi Pada Tegangan Kolektor Saat Transistor 'OFF'?", "id": "Tegangan Kolektor Menjadi Tinggi (mendekati Vcc)." },
    { "en": "Bagaimana Kapasitor Diisi Dalam Astabil BJT?", "id": "Melalui Resistor Kolektor Dan Basis Transistor Lain." },
    { "en": "Tegangan Apa Yang Menyebabkan Transistor Berpindah Keadaan?", "id": "Tegangan Pada Basisnya." },
    { "en": "Sinyal Apa Yang Bisa Dikonversi Oleh Pemicu Schmitt?", "id": "Gelombang Sinus Menjadi Gelombang Kotak." },
    { "en": "Multivibrator Astabil Adalah Generator Sinyal Jenis Apa?", "id": "Generator Gelombang Kotak." },
    { "en": "Apakah Multivibrator Bisa Dibuat Dengan Gerbang Logika?", "id": "Ya, Menggunakan Inverter, NAND, Atau NOR." },
    { "en": "Multivibrator Astabil Gerbang Logika Menggunakan Komponen Apa?", "id": "Inverter (NOT Gates) Dan Jaringan RC." },
    { "en": "Apa Fungsi Pin 2 Pada IC 555?", "id": "Sebagai Input Pemicu (Trigger)." },
    { "en": "Apa Fungsi Pin 6 Pada IC 555?", "id": "Sebagai Input Ambang (Threshold)." },
    { "en": "Apa Fungsi Pin 7 Pada IC 555?", "id": "Untuk Mengosongkan Kapasitor Pewaktu (Discharge)." },
    { "en": "Apa Fungsi Pin 3 Pada IC 555?", "id": "Sebagai Terminal Output." },
    { "en": "Pada Mode Monostabil, Pemicu Diberikan Ke Pin Mana?", "id": "Pin 2 (Trigger)." },
    { "en": "Tegangan Ambang Internal IC 555 Berapa?", "id": "Dua Pertiga Dari Vcc." },
    { "en": "Tegangan Pemicu Internal IC 555 Berapa?", "id": "Sepertiga Dari Vcc." },
    { "en": "Jaringan Internal Apa Yang Menetapkan Tegangan Ambang?", "id": "Pembagi Tegangan Tiga Resistor." },
    { "en": "Komponen Internal Utama IC 555 Adalah?", "id": "Dua Komparator, Satu Flip-Flop, Satu Transistor." },
    { "en": "Apa Yang Terjadi Jika Tegangan Pin 2 Kurang Dari 1/3 Vcc?", "id": "Flip-Flop Di-Set, Output Menjadi Tinggi." },
    { "en": "Apa Yang Terjadi Jika Tegangan Pin 6 Lebih Dari 2/3 Vcc?", "id": "Flip-Flop Di-Reset, Output Menjadi Rendah." },
    { "en": "Apa Fungsi Pin 4 Pada IC 555?", "id": "Sebagai Input Reset." },
    { "en": "Bagaimana Cara Kerja Pin Reset?", "id": "Jika Rendah, Mereset Timer Terlepas Dari Input Lain." },
    { "en": "Jika Pin Reset Tidak Digunakan, Dihubungkan Ke Mana?", "id": "Ke Vcc Untuk Mencegah Reset Palsu." },
    { "en": "Apa Fungsi Pin 5 Pada IC 555?", "id": "Sebagai Tegangan Kontrol (Control Voltage)." },
    { "en": "Untuk Apa Pin Tegangan Kontrol Digunakan?", "id": "Untuk Mengubah Tegangan Ambang Secara Eksternal." },
    { "en": "Jika Pin 5 Tidak Digunakan, Dihubungkan Ke Mana?", "id": "Ke Ground Melalui Kapasitor Kecil." },
    { "en": "Tujuan Kapasitor Di Pin 5?", "id": "Mengurangi Gangguan Derau Pada Pembagi Tegangan." },
    { "en": "Rumus Durasi Pulsa Monostabil IC 555?", "id": "T = 1.1 * R * C." },
    { "en": "Rumus Frekuensi Astabil IC 555?", "id": "f = 1.44 / ((Ra + 2Rb) * C)." },
    { "en": "Dalam Astabil IC 555, Pengisian Kapasitor Melalui Resistor Mana?", "id": "Melalui Ra Dan Rb." },
    { "en": "Dalam Astabil IC 555, Pengosongan Kapasitor Melalui Resistor Mana?", "id": "Hanya Melalui Rb." },
    { "en": "Apa Itu 'Mark Time' (T_high) Di Astabil 555?", "id": "Durasi Saat Output Berlogika Tinggi." },
    { "en": "Apa Itu 'Space Time' (T_low) Di Astabil 555?", "id": "Durasi Saat Output Berlogika Rendah." },
    { "en": "Rumus T_high Untuk Astabil 555?", "id": "T_high = 0.693 * (Ra + Rb) * C." },
    { "en": "Rumus T_low Untuk Astabil 555?", "id": "T_low = 0.693 * Rb * C." },
    { "en": "Bisakah T_high Lebih Pendek Dari T_low Di Astabil 555 Standar?", "id": "Tidak, T_high Selalu Lebih Panjang." },
    { "en": "Bagaimana Cara Mendapatkan Siklus Kerja 50% Dengan IC 555?", "id": "Menambahkan Dioda Paralel Dengan Resistor Rb." },
    { "en": "Apa Output Dari Multivibrator Bistabil?", "id": "Level Tegangan DC Stabil (Tinggi/Rendah)." },
    { "en": "Apa Output Dari Multivibrator Astabil?", "id": "Gelombang Kotak Kontinu." },
    { "en": "Apa Output Dari Multivibrator Monostabil?", "id": "Satu Pulsa Dengan Durasi Tertentu." },
    { "en": "Multivibrator Mana Yang Membutuhkan Sinyal Clock?", "id": "Flip-Flop Sinkron." },
    { "en": "Apa Itu Flip-Flop Sinkron?", "id": "Perubahan Keadaan Terjadi Hanya Pada Tepi Clock." },
    { "en": "Apa Itu Latch?", "id": "Flip-Flop Asinkron (Level-Triggered)." },
    { "en": "Apa Kelemahan Latch?", "id": "Output Bisa Terus Berubah Selama Sinyal Enable Aktif." },
    { "en": "Apa Peran Clock Dalam Sistem Digital?", "id": "Mensinkronkan Semua Operasi." },
    { "en": "Gelombang Clock Yang Baik Memiliki Tepi Bagaimana?", "id": "Tepi Naik Dan Turun Yang Sangat Curam." },
    { "en": "Bagaimana Cara Membuat Pembagi Frekuensi?", "id": "Menggunakan T Flip-Flop Atau JK Flip-Flop." },
    { "en": "Jika Frekuensi Input T Flip-Flop 10 KHz, Frekuensi Outputnya?", "id": "Lima Kilo Hertz (5 KHz)." },
    { "en": "Bagaimana Cara Membuat Pencacah Biner (Binary Counter)?", "id": "Dengan Menghubungkan Beberapa Flip-Flop Secara Berantai." },
    { "en": "Apa Itu 'Ripple Counter'?", "id": "Pencacah Asinkron, Output Satu Flip-Flop Memicu Berikutnya." },
    { "en": "Apa Kelemahan 'Ripple Counter'?", "id": "Adanya Penundaan Propagasi (Propagation Delay)." },
    { "en": "Apa Itu Pencacah Sinkron?", "id": "Semua Flip-Flop Dipicu Oleh Sinyal Clock Yang Sama." },
    { "en": "Apa Keuntungan Pencacah Sinkron?", "id": "Lebih Cepat Dan Tidak Ada Masalah Penundaan." },
    { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Menyimpan Dan Menggeser Data Bit Demi Bit." },
    { "en": "Register Geser Dibuat Dari Apa?", "id": "Rangkaian Flip-Flop Tipe D." },
    { "en": "Apa Itu 'Loading' Data Ke Register?", "id": "Memasukkan Data Awal Ke Dalam Register." },
    { "en": "Multivibrator Mana Yang Paling Dekat Dengan Konsep Memori?", "id": "Multivibrator Bistabil (Flip-Flop)." },
    { "en": "Berapa Bit Informasi Yang Disimpan Satu Flip-Flop?", "id": "Satu Bit (0 Atau 1)." },
    { "en": "Apa Yang Terjadi Pada Keadaan Flip-Flop Saat Daya Dimatikan?", "id": "Keadaannya Hilang (Memori Volatil)." },
    { "en": "Apa Itu 'Race Condition'?", "id": "Kondisi Tak Tentu Akibat Penundaan Sinyal." },
    { "en": "Flip-Flop Master-Slave Dirancang Untuk Mencegah Apa?", "id": "Mencegah 'Race Condition'." },
    { "en": "Bagaimana Cara Kerja Flip-Flop Master-Slave?", "id": "Menggunakan Dua Latch (Master Dan Slave)." },
    { "en": "Kapan Master Menerima Data?", "id": "Saat Tepi Clock Naik." },
    { "en": "Kapan Slave Mengeluarkan Data?", "id": "Saat Tepi Clock Turun." },
    { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Minimum Data Harus Stabil Sebelum Clock." },
    { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Minimum Data Harus Stabil Setelah Clock." },
    { "en": "Apa Yang Terjadi Jika 'Setup' Atau 'Hold Time' Dilanggar?", "id": "Operasi Flip-Flop Menjadi Tidak Dapat Diprediksi." },
    { "en": "Apa Itu 'Metastability'?", "id": "Keadaan Tak Tentu Di Mana Output Bukan 0 Atau 1." },
    { "en": "Kapan Metastabilitas Bisa Terjadi?", "id": "Ketika 'Setup' Atau 'Hold Time' Dilanggar." },
    { "en": "Apa Sumber Osilasi Pada Multivibrator Astabil?", "id": "Pengisian Dan Pengosongan Kapasitor Secara Berulang." },
    { "en": "Apa Sumber Stabilitas Pada Multivibrator Bistabil?", "id": "Dua Loop Umpan Balik Positif Yang Saling Mengunci." },
    { "en": "Apa Yang Memulai Transisi Dalam Monostabil?", "id": "Pemicu Eksternal Yang Mengganggu Keadaan Stabil." },
    { "en": "Tegangan Output Multivibrator Biasanya Berayun Antara Apa?", "id": "Vcc (Tinggi) Dan Ground (Rendah)." },
    { "en": "Apa Itu Multivibrator Emitor-Terkopel (Emitter-Coupled)?", "id": "Jenis Multivibrator Berkecepatan Tinggi." },
    { "en": "Apa Keuntungan Multivibrator Emitor-Terkopel?", "id": "Transistor Tidak Masuk Ke Saturasi Penuh." },
    { "en": "Mengapa Menghindari Saturasi Penting Untuk Kecepatan?", "id": "Karena Keluar Dari Saturasi Membutuhkan Waktu." },
    { "en": "Apa Itu 'Storage Time' Transistor?", "id": "Waktu Yang Dibutuhkan Untuk Keluar Dari Saturasi." },
    { "en": "Apakah IC 555 Bisa Menjadi Bistabil?", "id": "Ya, Dengan Menggunakan Pin Trigger Dan Reset." },
    { "en": "Bagaimana Cara Membuat Latch Dengan IC 555?", "id": "Pin 2 Sebagai Set, Pin 4 Sebagai Reset." },
    { "en": "Apa Batasan Tegangan Catu Daya IC 555?", "id": "Biasanya Antara 5 Hingga 15 Volt." },
    { "en": "Apa Itu Versi CMOS Dari IC 555?", "id": "7555 Atau LMC555." },
    { "en": "Apa Keuntungan Versi CMOS?", "id": "Konsumsi Daya Rendah, Dapat Bekerja Di Tegangan Rendah." },
    { "en": "Apa Itu 'Debouncing'?", "id": "Menghilangkan Pantulan Sinyal Dari Saklar Mekanis." },
    { "en": "Multivibrator Apa Yang Bisa Untuk 'Debouncing'?", "id": "Monostabil Atau Bistabil (SR Latch)." },
    { "en": "Bagaimana Monostabil Melakukan 'Debouncing'?", "id": "Menghasilkan Satu Pulsa Bersih Dari Pantulan Pertama." },
    { "en": "Apa Itu 'Glitches'?", "id": "Pulsa Palsu Berdurasi Sangat Pendek." },
    { "en": "Pemicu Schmitt Bisa Menghilangkan 'Glitches'?", "id": "Bisa, Jika Amplitudo Glitch Di Bawah Histeresis." },
    { "en": "Apa Itu Osilator Kristal?", "id": "Osilator Dengan Stabilitas Frekuensi Sangat Tinggi." },
    { "en": "Apakah Osilator Kristal Termasuk Multivibrator?", "id": "Tidak, Itu Adalah Osilator Resonansi." },
    { "en": "Perbedaan Utama Dengan Osilator Relaksasi?", "id": "Osilator Resonansi Menggunakan Komponen Resonansi (Kristal/LC)." },
    { "en": "Akurasi Frekuensi Mana Yang Lebih Baik?", "id": "Osilator Resonansi Jauh Lebih Akurat." },
    { "en": "Apa Itu 'Jitter'?", "id": "Variasi Kecil Dari Periode Ke Periode Sinyal Clock." },
    { "en": "Osilator Relaksasi Memiliki 'Jitter' Bagaimana?", "id": "Cenderung Memiliki Jitter Yang Lebih Tinggi." },
    { "en": "Multivibrator Mana Yang Tidak Memerlukan Kapasitor Pewaktu?", "id": "Multivibrator Bistabil (Flip-Flop)." },
    { "en": "Mengapa Bistabil Tidak Perlu Kapasitor?", "id": "Keadaannya Ditentukan Oleh Pemicu, Bukan Waktu RC." },
    { "en": "Apa Itu 'Power-On Reset'?", "id": "Sirkuit Yang Mereset Sistem Saat Daya Dinyalakan." },
    { "en": "Rangkaian Apa Yang Bisa Untuk 'Power-On Reset'?", "id": "Rangkaian RC Sederhana Atau Monostabil." },
    { "en": "Bagaimana Sirkuit RC Membuat Sinyal Reset?", "id": "Menghasilkan Pulsa Sesaat Saat Kapasitor Mengisi." },
    { "en": "Apa Itu Osilator Gelang (Ring Oscillator)?", "id": "Astabil Dibuat Dari Inverter Ganjil Berantai." },
    { "en": "Frekuensi Osilator Gelang Ditentukan Oleh Apa?", "id": "Jumlah Inverter Dan Penundaan Gerbang." },
    { "en": "Apa Itu 'Propagation Delay'?", "id": "Waktu Tunda Sinyal Melewati Sebuah Gerbang." },
    { "en": "Apa Itu 'Waveform'?", "id": "Bentuk Grafis Dari Sinyal Terhadap Waktu." },
    { "en": "Apa Itu Amplitudo?", "id": "Nilai Puncak Dari Suatu Sinyal." },
    { "en": "Tegangan Amplitudo Multivibrator Ditentukan Oleh Apa?", "id": "Tegangan Catu Daya (Vcc)." },
    { "en": "Apa Itu 'Time Base'?", "id": "Sinyal Clock Akurat Sebagai Referensi Waktu." },
    { "en": "Multivibrator Astabil Bisa Digunakan Sebagai 'Time Base'?", "id": "Ya, Tapi Akurasinya Tidak Terlalu Tinggi." },
    { "en": "Apa Itu Regenerasi Dalam Multivibrator?", "id": "Proses Cepat Umpan Balik Positif Menguatkan Diri." },
    { "en": "Transisi Keadaan Dalam Multivibrator Terjadi Secara?", "id": "Sangat Cepat Karena Proses Regenerasi." },
    { "en": "Apa Itu 'Recovery Time' Monostabil?", "id": "Waktu Agar Kapasitor Pulih Setelah Pulsa Selesai." },
    { "en": "Apa Yang Terjadi Jika Monostabil Dipicu Sebelum 'Recovery Time'?", "id": "Durasi Pulsa Berikutnya Mungkin Tidak Akurat." },
    { "en": "Apa Itu 'Sinking Current'?", "id": "Arus Yang Mengalir Masuk Ke Terminal Output." },
    { "en": "Apa Itu 'Sourcing Current'?", "id": "Arus Yang Mengalir Keluar Dari Terminal Output." },
    { "en": "Output IC 555 Bisa 'Sink' Atau 'Source' Arus?", "id": "Bisa Keduanya, Cukup Kuat." },
    { "en": "Berapa Arus Output Maksimum IC 555?", "id": "Sekitar Dua Ratus Miliampere (200mA)." },
    { "en": "Kemampuan Ini Memungkinkan IC 555 Mengerjakan Apa?", "id": "Mengemudikan Beban Kecil Secara Langsung (LED/Relay)." },
    { "en": "Apa Itu 'Pull-up Resistor'?", "id": "Resistor Yang Menghubungkan Jalur Ke Vcc." },
    { "en": "Apa Fungsi 'Pull-up Resistor'?", "id": "Memastikan Level Logika Tinggi Saat Tidak Aktif." },
    { "en": "Apa Itu 'Pull-down Resistor'?", "id": "Resistor Yang Menghubungkan Jalur Ke Ground." },
    { "en": "Apa Fungsi 'Pull-down Resistor'?", "id": "Memastikan Level Logika Rendah Saat Tidak Aktif." },
    { "en": "Apa Itu Output 'Open Collector'?", "id": "Output Yang Hanya Bisa Menarik Ke Level Rendah." },
    { "en": "Output 'Open Collector' Membutuhkan Apa?", "id": "Resistor Pull-up Eksternal." },
    { "en": "Apa Itu Logika 'Wired-AND'?", "id": "Menghubungkan Beberapa Output Open Collector Bersama." },
    { "en": "Apakah Pin 7 IC 555 Adalah 'Open Collector'?", "id": "Ya, Kolektor Transistor Internal." },
    { "en": "Apa Itu Keluarga Logika TTL?", "id": "Transistor-Transistor Logic." },
    { "en": "Berapa Level Tegangan Logika TTL?", "id": "Nol Hingga Lima Volt." },
    { "en": "Apa Itu Keluarga Logika CMOS?", "id": "Complementary Metal-Oxide-Semiconductor." },
    { "en": "Apa Keunggulan Logika CMOS?", "id": "Konsumsi Daya Sangat Rendah." },
    { "en": "Bagaimana Cara Mengubah Amplitudo Output Multivibrator?", "id": "Mengubah Tegangan Catu Daya (Vcc)." },
    { "en": "Apa Yang Terjadi Pada Frekuensi Astabil Jika Vcc Diubah?", "id": "Idealnya Tidak Berubah (Independen)." },
    { "en": "Mengapa Frekuensi Astabil 555 Independen Dari Vcc?", "id": "Karena Tegangan Ambang Adalah Rasio Dari Vcc." },
    { "en": "Apa Itu 'Voltage Controlled Oscillator' (VCO)?", "id": "Frekuensi Osilasi Dikontrol Oleh Tegangan." },
    { "en": "Bagaimana Membuat VCO Dengan IC 555?", "id": "Memberikan Tegangan Kontrol Ke Pin 5." },
    { "en": "Apa Itu 'Pulse Width Modulation' (PWM)?", "id": "Memodulasi Lebar Pulsa Sesuai Sinyal Informasi." },
    { "en": "Bagaimana Membuat Sinyal PWM Dengan IC 555?", "id": "Menggunakan Mode Astabil Dan Mengontrol Pin 5." },
    { "en": "Apa Itu 'Pulse Position Modulation' (PPM)?", "id": "Memodulasi Posisi Pulsa Sesuai Sinyal Informasi." },
    { "en": "Aplikasi PWM?", "id": "Kontrol Kecepatan Motor, Peredupan Lampu LED." },
    { "en": "Multivibrator Astabil Simetris Dibuat Dengan Komponen Apa?", "id": "Dua Transistor Dan Dua Pasang RC Yang Identik." },
    { "en": "Apa Yang Terjadi Jika Komponen RC Tidak Identik?", "id": "Siklus Kerja Tidak Akan 50%." },
    { "en": "Apa Peran Dioda Kopling Dalam Beberapa Desain?", "id": "Mempercepat Waktu Pemulihan." },
    { "en": "Apa Itu 'Blocking Oscillator'?", "id": "Jenis Osilator Relaksasi Menggunakan Transformator." },
    { "en": "Apa Keuntungan 'Blocking Oscillator'?", "id": "Menghasilkan Pulsa Output Yang Sangat Kuat." },
    { "en": "Apa Itu Multivibrator Terintegrasi?", "id": "Multivibrator Yang Dibuat Dalam Satu Chip IC." },
    { "en": "Contoh Multivibrator Terintegrasi?", "id": "IC 74121 (Monostabil), IC 74123 (Dual Monostabil)." },
    { "en": "Apa Keuntungan Menggunakan IC Khusus?", "id": "Jumlah Komponen Eksternal Lebih Sedikit." },
    { "en": "Apa Itu 'Missing Pulse Detector'?", "id": "Sirkuit Yang Mendeteksi Jika Pulsa Hilang." },
    { "en": "Bagaimana Cara Membuat 'Missing Pulse Detector'?", "id": "Menggunakan Monostabil 'Retriggerable'." },
    { "en": "Apa Itu 'Watchdog Timer'?", "id": "Timer Yang Mereset Sistem Jika Macet." },
    { "en": "Prinsip Kerja 'Watchdog Timer'?", "id": "Monostabil Yang Harus Dipicu Ulang Secara Berkala." },
    { "en": "Jika Sistem Macet, Apa Yang Terjadi Pada 'Watchdog'?", "id": "Pulsa Monostabil Berakhir Dan Mereset Sistem." },
    { "en": "Bagaimana Pengaruh Suhu Terhadap Multivibrator?", "id": "Dapat Mempengaruhi Frekuensi Atau Durasi Pulsa." },
    { "en": "Komponen Apa Yang Paling Sensitif Terhadap Suhu?", "id": "Kapasitor Dan Semikonduktor (Transistor)." },
    { "en": "Bagaimana Cara Menstabilkan Frekuensi Terhadap Suhu?", "id": "Menggunakan Komponen Dengan Koefisien Suhu Rendah." },
    { "en": "Berapa Banyak Keadaan Stabil Yang Dimiliki Osilator?", "id": "Nol." },
    { "en": "Berapa Banyak Keadaan Stabil Yang Dimiliki Memori?", "id": "Dua Atau Lebih." },
    { "en": "Multivibrator Mana Yang Paling Sederhana?", "id": "Biasanya Astabil Berbasis Dua Transistor." },
    { "en": "Apa Itu 'Loading Effect'?", "id": "Pengaruh Beban Terhadap Kinerja Rangkaian." },
    { "en": "Bagaimana 'Loading' Mempengaruhi Osilator?", "id": "Dapat Mengubah Frekuensi Atau Menghentikan Osilasi." },
    { "en": "Bagaimana Cara Mengisolasi Osilator Dari Beban?", "id": "Menggunakan Rangkaian Penyangga (Buffer)." },
    { "en": "Contoh Rangkaian Penyangga?", "id": "Pengikut Tegangan (Voltage Follower)." },
    { "en": "Apa Itu 'Fan-out'?", "id": "Jumlah Maksimum Input Gerbang Yang Bisa Digerakkan." },
    { "en": "Apa Itu 'Fan-in'?", "id": "Jumlah Input Pada Sebuah Gerbang Logika." },
    { "en": "Apakah Multivibrator Bisa Dibuat Dengan Tabung Vakum?", "id": "Ya, Itu Adalah Desain Awalnya." },
    { "en": "Siapa Yang Menemukan Multivibrator?", "id": "Henri Abraham Dan Eugene Bloch." },
    { "en": "Kapan Multivibrator Pertama Kali Dibuat?", "id": "Selama Perang Dunia Pertama." },
    { "en": "Apa Itu 'Flip-Flop' Berbasis Gerbang NAND?", "id": "SR Latch Yang Dibuat Dari Dua Gerbang NAND." },
    { "en": "Apa Itu 'Flip-Flop' Berbasis Gerbang NOR?", "id": "SR Latch Yang Dibuat Dari Dua Gerbang NOR." },
    { "en": "Apa Perbedaan Antara Keduanya?", "id": "Perilaku Input Aktifnya (Aktif Rendah/Tinggi)." },
    { "en": "Apa Itu Logika Aktif Rendah (Active-Low)?", "id": "Sinyal Dianggap Aktif Saat Berlogika Rendah." },
    { "en": "SR Latch NAND Adalah Aktif Apa?", "id": "Aktif Rendah." },
    { "en": "SR Latch NOR Adalah Aktif Apa?", "id": "Aktif Tinggi." },
    { "en": "Apa Itu 'Clocked SR Flip-Flop'?", "id": "SR Latch Dengan Input Enable Tambahan (Clock)." },
    { "en": "Apa Kegunaan Input Enable?", "id": "Mengontrol Kapan Latch Menerima Data." },
    { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Selama Enable Aktif." },
    { "en": "D Latch Sering Disebut Apa?", "id": "Latch Transparan." },
    { "en": "Bagaimana Cara Membuat T Flip-Flop Dari JK Flip-Flop?", "id": "Hubungkan Input J Dan K Bersama." },
    { "en": "Bagaimana Cara Membuat D Flip-Flop Dari JK Flip-Flop?", "id": "Hubungkan J Ke D, K Ke Invers D." },
    { "en": "Apa Itu 'Asynchronous Input' Pada Flip-Flop?", "id": "Input Yang Bekerja Independen Dari Clock." },
    { "en": "Contoh 'Asynchronous Input'?", "id": "Preset (Set) Dan Clear (Reset)." },
    { "en": "Apa Fungsi Input Preset?", "id": "Memaksa Output Q Menjadi Tinggi." },
    { "en": "Apa Fungsi Input Clear?", "id": "Memaksa Output Q Menjadi Rendah." },
    { "en": "Mengapa Input Asinkron Berguna?", "id": "Untuk Mengatur Keadaan Awal Sistem." },
    { "en": "Dalam IC Pencacah, Input Clear Disebut Apa?", "id": "Master Reset (MR)." },
    { "en": "Apa Itu 'Frequency Synthesizer'?", "id": "Sirkuit Yang Menghasilkan Berbagai Frekuensi Dari Satu Referensi." },
    { "en": "Komponen Kunci 'Frequency Synthesizer'?", "id": "VCO, Phase Detector, Dan Pembagi Frekuensi." },
    { "en": "Pembagi Frekuensi Dalam 'Synthesizer' Dibangun Dari Apa?", "id": "Rangkaian Flip-Flop." },
    { "en": "Apa Itu 'Phase-Locked Loop' (PLL)?", "id": "Sistem Umpan Balik Yang Mengunci Fasa VCO." },
    { "en": "Di Mana PLL Digunakan?", "id": "Radio, Komunikasi, Komputer." },
    { "en": "Apakah Multivibrator Dapat Menghasilkan Gelombang Segitiga?", "id": "Ya, Dengan Menambahkan Sirkuit Integrator." },
    { "en": "Bagaimana Cara Membuat Generator Fungsi?", "id": "Menggabungkan Astabil (Kotak) Dan Integrator (Segitiga/Sinus)." },
    { "en": "Apa Yang Terjadi Jika Resistor Pewaktu Terlalu Kecil?", "id": "Arus Terlalu Besar, Merusak Transistor." },
    { "en": "Apa Yang Terjadi Jika Resistor Pewaktu Terlalu Besar?", "id": "Arus Bocor Menjadi Signifikan, Operasi Tidak Akurat." },
    { "en": "Apa Yang Terjadi Jika Kapasitor Pewaktu Terlalu Kecil?", "id": "Kapasitansi Parasitik Mendominasi, Frekuensi Tidak Stabil." },
    { "en": "Apa Yang Terjadi Jika Kapasitor Pewaktu Terlalu Besar?", "id": "Arus Bocor Kapasitor Menyebabkan Kesalahan Pewaktuan." },
    { "en": "Rentang Praktis Untuk Komponen Pewaktu?", "id": "Resistor Kilo-Ohm Hingga Mega-Ohm, Kapasitor Pikofarad Hingga Mikrofarad." },
    { "en": "Apa Itu 'Breadboard'?", "id": "Papan Untuk Membuat Prototipe Sirkuit Sementara." },
    { "en": "Mengapa 'Breadboard' Berguna Untuk Multivibrator?", "id": "Mudah Untuk Mencoba Berbagai Nilai R Dan C." },
    { "en": "Apa Itu Osiloskop?", "id": "Alat Untuk Melihat Bentuk Gelombang Sinyal." },
    { "en": "Apa Yang Diukur Dengan Osiloskop?", "id": "Tegangan, Periode, Frekuensi, Siklus Kerja." },
    { "en": "Bagaimana Tampilan Output Astabil Di Osiloskop?", "id": "Gelombang Kotak Yang Berulang." },
    { "en": "Bagaimana Tampilan Output Monostabil Di Osiloskop?", "id": "Satu Pulsa Muncul Setelah Pemicu." },
    { "en": "Bagaimana Tampilan Output Bistabil Di Osiloskop?", "id": "Level Tegangan Berubah Saat Dipicu." },
    { "en": "Apa Itu 'Duty Cycle'?", "id": "Persentase Waktu ON Dalam Satu Periode." },
    { "en": "Rumus Siklus Kerja?", "id": "Siklus Kerja = (T_on / T_total) * 100%." },
    { "en": "Siklus Kerja 50% Berarti Apa?", "id": "Waktu ON Sama Dengan Waktu OFF." },
    { "en": "Bentuk Gelombang Dengan Siklus Kerja 50%?", "id": "Gelombang Kotak Simetris." },
    { "en": "Bisakah Siklus Kerja Menjadi 100%?", "id": "Tidak, Itu Menjadi Sinyal DC Konstan." },
    { "en": "Bisakah Siklus Kerja Menjadi 0%?", "id": "Tidak, Itu Juga Sinyal DC Konstan." },
    { "en": "Apa Itu 'Pulse Train'?", "id": "Serangkaian Pulsa Yang Berulang." },
    { "en": "Multivibrator Astabil Adalah Generator 'Pulse Train'?", "id": "Ya, Benar." },
    { "en": "Apa Perbedaan Pulsa Dan Gelombang Kotak?", "id": "Pulsa Biasanya Memiliki Siklus Kerja Rendah." },
    { "en": "Kapan Sebuah Transistor Dikatakan Jenuh (Saturated)?", "id": "Ketika Arus Basis Cukup Untuk Arus Kolektor Maksimal." },
    { "en": "Kapan Sebuah Transistor Dikatakan Terputus (Cut-off)?", "id": "Ketika Tegangan Basis-Emitor Terlalu Rendah." },
    { "en": "Daerah Kerja Transistor Untuk Saklar?", "id": "Saturasi Dan Cut-off." },
    { "en": "Daerah Kerja Transistor Untuk Penguat?", "id": "Daerah Aktif." },
    { "en": "Apakah Multivibrator Menggunakan Daerah Aktif?", "id": "Hanya Secara Singkat Selama Transisi." },
    { "en": "Mayoritas Waktu Transistor Berada Di Keadaan Apa?", "id": "Sepenuhnya ON Atau Sepenuhnya OFF." },
    { "en": "Apa Itu 'Cross-Coupling'?", "id": "Output Satu Tahap Dihubungkan Ke Input Tahap Lain." },
    { "en": "Jenis Kopling Apa Yang Digunakan Di Astabil Klasik?", "id": "Kopling Kapasitif (AC Coupling)." },
    { "en": "Jenis Kopling Apa Yang Digunakan Di Bistabil Klasik?", "id": "Kopling Langsung (DC Coupling)." },
    { "en": "Mengapa Bistabil Menggunakan Kopling DC?", "id": "Untuk Mempertahankan Keadaan Stabil Secara Permanen." },
    { "en": "Apa Itu 'Timing Resistor'?", "id": "Resistor Yang Menentukan Konstanta Waktu RC." },
    { "en": "Apa Itu 'Timing Capacitor'?", "id": "Kapasitor Yang Menentukan Konstanta Waktu RC." },
    { "en": "Tegangan Pada Kapasitor Mengikuti Kurva Apa?", "id": "Kurva Pengisian Dan Pengosongan Eksponensial." },
    { "en": "Rumus Umum Pengisian Kapasitor?", "id": "V(t) = V_final * (1 - e^(-t/RC))." },
    { "en": "Rumus Umum Pengosongan Kapasitor?", "id": "V(t) = V_initial * e^(-t/RC)." },
    { "en": "Apa Itu 'Threshold'?", "id": "Level Tegangan Yang Memicu Suatu Aksi." },
    { "en": "Di IC 555, 'Threshold' Digunakan Untuk Apa?", "id": "Mengakhiri Waktu Pengisian Dan Mereset Output." },
    { "en": "Di IC 555, 'Trigger' Digunakan Untuk Apa?", "id": "Memulai Waktu Pengisian Dan Men-set Output." },
    { "en": "Apa Itu 'Set' Dalam Konteks Flip-Flop?", "id": "Membuat Output Q Menjadi 1." },
    { "en": "Apa Itu 'Reset' Dalam Konteks Flip-Flop?", "id": "Membuat Output Q Menjadi 0 (Clear)." },
    { "en": "Apa Itu 'Toggle' Dalam Konteks Flip-Flop?", "id": "Mengubah Output Ke Keadaan Berlawanan." },
    { "en": "Flip-Flop Mana Yang Bisa 'Toggle'?", "id": "JK Flip-Flop Dan T Flip-Flop." },
    { "en": "Apa Itu 'One-Hot' Encoding?", "id": "Representasi Di Mana Hanya Satu Bit Aktif." },
    { "en": "Bagaimana 'Ring Counter' Dibuat?", "id": "Register Geser Dengan Output Terhubung Kembali Ke Input." },
    { "en": "Apa Itu 'Johnson Counter'?", "id": "Register Geser Dengan Output Invers Terhubung Ke Input." },
    { "en": "Berapa Banyak Keadaan Unik 'Johnson Counter' 4-Bit?", "id": "Delapan Keadaan (2N)." },
    { "en": "Apa Keuntungan 'Johnson Counter'?", "id": "Menghasilkan Urutan Yang Bebas 'Glitch'." },
    { "en": "Apa Itu 'State Machine' (Mesin Keadaan)?", "id": "Model Komputasi Berdasarkan Keadaan Dan Transisi." },
    { "en": "Blok Bangunan 'State Machine'?", "id": "Flip-Flop (Untuk Menyimpan Keadaan) Dan Logika Kombinasional." },
    { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi Grafis Dari Sebuah 'State Machine'." },
    { "en": "Lingkaran Dalam Diagram Keadaan Mewakili Apa?", "id": "Keadaan (State)." },
    { "en": "Panah Dalam Diagram Keadaan Mewakili Apa?", "id": "Transisi Antar Keadaan." },
    { "en": "Apa Itu 'Sequential Logic'?", "id": "Logika Di Mana Output Bergantung Pada Input Saat Ini Dan Sebelumnya." },
    { "en": "Mengapa Disebut Sekuensial?", "id": "Karena Bergantung Pada Urutan (Sequence) Input." },
    { "en": "Multivibrator Bistabil Adalah Jantung Dari 'Sequential Logic'?", "id": "Ya, Benar." },
    { "en": "Apa Itu 'Combinational Logic'?", "id": "Output Hanya Bergantung Pada Input Saat Ini." },
    { "en": "Contoh 'Combinational Logic'?", "id": "Gerbang AND/OR/NOT, Encoder, Decoder, Multiplexer." },
    { "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
    { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengirim Satu Input Ke Salah Satu Dari Banyak Output." },
    { "en": "Apa Itu Memori RAM?", "id": "Random Access Memory." },
    { "en": "Sel Memori RAM Statis (SRAM) Dibuat Dari Apa?", "id": "Flip-Flop (Biasanya 6 Transistor)." },
    { "en": "Mengapa SRAM Disebut Statis?", "id": "Tidak Perlu Di-refresh Selama Daya Ada." },
    { "en": "Sel Memori RAM Dinamis (DRAM) Dibuat Dari Apa?", "id": "Satu Transistor Dan Satu Kapasitor." },
    { "en": "Mengapa DRAM Disebut Dinamis?", "id": "Muatan Kapasitor Harus Di-refresh Secara Berkala." },
    { "en": "Mana Yang Lebih Cepat, SRAM Atau DRAM?", "id": "SRAM Lebih Cepat." },
    { "en": "Mana Yang Lebih Padat, SRAM Atau DRAM?", "id": "DRAM Jauh Lebih Padat." },
    { "en": "Memori Cache CPU Menggunakan Apa?", "id": "SRAM." },
    { "en": "Memori Utama Komputer Menggunakan Apa?", "id": "DRAM." },
    { "en": "Apa Itu 'Positive Edge-Triggered'?", "id": "Flip-Flop Berubah Keadaan Saat Tepi Naik Clock." },
    { "en": "Apa Itu 'Negative Edge-Triggered'?", "id": "Flip-Flop Berubah Keadaan Saat Tepi Turun Clock." },
    { "en": "Bagaimana Simbol Clock 'Edge-Triggered'?", "id": "Segitiga Kecil Di Input Clock." },
    { "en": "Bagaimana Membedakan Tepi Positif Dan Negatif?", "id": "Lingkaran Kecil Menandakan Tepi Negatif." },
    { "en": "Apa Itu 'Clock Skew'?", "id": "Perbedaan Waktu Kedatangan Clock Di Titik Berbeda." },
    { "en": "Apa Masalah Yang Disebabkan 'Clock Skew'?", "id": "Dapat Menyebabkan Pelanggaran 'Setup' Dan 'Hold Time'." },
    { "en": "Apa Itu 'Clock Distribution Network'?", "id": "Jaringan Untuk Mendistribusikan Clock Dengan Skew Minimal." },
    { "en": "Frekuensi Operasi Astabil Dipengaruhi Oleh Apa?", "id": "Toleransi Nilai R Dan C." },
    { "en": "Bagaimana Cara Mendapatkan Frekuensi Yang Tepat?", "id": "Menggunakan Resistor Variabel (Trimpot) Untuk Penyetelan." },
    { "en": "Apa Itu 'Linear Ramp Generator'?", "id": "Sirkuit Yang Menghasilkan Gelombang Gigi Gergaji." },
    { "en": "Bagaimana Cara Membuatnya?", "id": "Mengisi Kapasitor Dengan Sumber Arus Konstan." },
    { "en": "Penguat Transkonduktansi Bisa Digunakan Sebagai Apa?", "id": "Sebagai Sumber Arus Konstan." },
    { "en": "Apa Itu 'Missing Pulse Detector'?", "id": "Mendeteksi Jika Sebuah Pulsa Dalam Urutan Hilang." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Menggunakan Monostabil yang terus-menerus di-reset oleh pulsa." },
    { "en": "Apa Itu 'Frequency-to-Voltage Converter'?", "id": "Mengubah Frekuensi Input Menjadi Tegangan DC Output." },
    { "en": "Bagaimana Prinsip Kerjanya?", "id": "Monostabil Memicu Pulsa, Outputnya Disaring Lolos-Rendah." },
    { "en": "Apa Itu 'Voltage-to-Frequency Converter'?", "id": "Mengubah Tegangan DC Input Menjadi Frekuensi Output." },
    { "en": "VCO Adalah 'Voltage-to-Frequency Converter'?", "id": "Ya, Benar." },
    { "en": "Apa Itu 'Digital-to-Analog Converter' (DAC)?", "id": "Mengubah Data Digital Menjadi Sinyal Analog." },
    { "en": "Apa Itu 'Analog-to-Digital Converter' (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Data Digital." },
    { "en": "Tipe ADC Apa Yang Menggunakan Komparator?", "id": "Flash ADC, Successive Approximation ADC." },
    { "en": "Apa Itu 'Flash ADC'?", "id": "ADC Sangat Cepat Menggunakan Banyak Komparator." },
    { "en": "Apa Itu 'Dual-Slope ADC'?", "id": "ADC Lambat Tapi Sangat Akurat." },
    { "en": "Prinsip 'Dual-Slope' Menggunakan Apa?", "id": "Pengisian Dan Pengosongan Integrator." },
    { "en": "Apa Hubungan Multivibrator Dengan Dunia Digital?", "id": "Mereka Adalah Blok Bangunan Fundamentalnya." },
    { "en": "Sinyal Clock Adalah Jantung Dari Sistem Digital?", "id": "Ya, Mendikte Laju Semua Operasi." },
    { "en": "Dari Mana Sinyal Clock Berasal?", "id": "Dari Osilator (Seringkali Multivibrator Astabil)." },
    { "en": "Apa Yang Dimaksud Dengan Sirkuit Regeneratif?", "id": "Sirkuit Dengan Umpan Balik Positif." },
    { "en": "Multivibrator Adalah Contoh Sirkuit Regeneratif?", "id": "Ya, Ketiga Jenisnya." },
    { "en": "Apa Itu 'Square Wave'?", "id": "Gelombang Kotak." },
    { "en": "Apa Itu 'Rectangular Wave'?", "id": "Gelombang Persegi Panjang (Siklus Kerja Bukan 50%)." },
    { "en": "Output Astabil IC 555 Adalah 'Square' Atau 'Rectangular'?", "id": "Biasanya 'Rectangular' (Persegi Panjang)." },
    { "en": "Apa Itu 'Pulse Stretcher'?", "id": "Memperpanjang Durasi Pulsa Yang Pendek." },
    { "en": "Multivibrator Apa Yang Digunakan Sebagai 'Pulse Stretcher'?", "id": "Monostabil." },
    { "en": "Apa Itu 'Dark-activated Switch'?", "id": "Saklar Yang Aktif Saat Gelap." },
    { "en": "Bagaimana Cara Membuatnya?", "id": "Menggunakan LDR Untuk Memicu Monostabil Atau Bistabil." },
    { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistansinya Berubah Tergantung Intensitas Cahaya." },
    { "en": "Apa Peran Transistor Q1 Dalam Astabil BJT?", "id": "Bertindak Sebagai Saklar Untuk Mengontrol Q2." },
    { "en": "Apa Peran Transistor Q2 Dalam Astabil BJT?", "id": "Bertindak Sebagai Saklar Untuk Mengontrol Q1." },
    { "en": "Kondisi Apa Yang Terjadi Saat Q1 'ON'?", "id": "Q2 Akan 'OFF' Karena Basisnya Ditarik Rendah." },
    { "en": "Kondisi Apa Yang Terjadi Saat Q1 'OFF'?", "id": "Q2 Akan 'ON' Setelah Kapasitor Mengisi Basisnya." },
    { "en": "Proses Ini Berulang Terus Menerus?", "id": "Ya, Menciptakan Osilasi." },
    { "en": "Berapa Waktu Yang Dibutuhkan Kapasitor Untuk Mengisi?", "id": "Tergantung Pada Konstanta Waktu RC." },
    { "en": "Tegangan Berapa Yang Diperlukan Untuk Menyalakan Transistor BJT?", "id": "Sekitar Nol Koma Tujuh Volt (0.7V) Di Basis." },
    { "en": "Apa Itu LED Flasher?", "id": "Sirkuit Sederhana Untuk Membuat LED Berkedip." },
    { "en": "LED Flasher Adalah Aplikasi Dari Multivibrator Apa?", "id": "Multivibrator Astabil." },
    { "en": "Bagaimana Siklus Kerja Mempengaruhi Kecerahan LED?", "id": "Siklus Kerja Lebih Tinggi, LED Tampak Lebih Terang." },
    { "en": "Apa Itu 'Persistence of Vision'?", "id": "Efek Mata Manusia Menahan Gambar Sesaat." },
    { "en": "Mengapa LED Berkedip Cepat Terlihat Menyala Terus?", "id": "Karena Efek 'Persistence of Vision'." },
    { "en": "PWM Mengandalkan Efek Ini?", "id": "Ya, Untuk Mengontrol Daya Rata-rata." },
    { "en": "Apa Itu 'Time-Delay Relay'?", "id": "Relay Yang Aktif Setelah Penundaan Waktu." },
    { "en": "Multivibrator Apa Yang Digunakan Untuk 'Time-Delay Relay'?", "id": "Monostabil." },
    { "en": "Durasi Penundaan Ditentukan Oleh Apa?", "id": "Durasi Pulsa Output Monostabil." },
    { "en": "Apa Itu 'Touch Switch'?", "id": "Saklar Yang Diaktifkan Oleh Sentuhan." },
    { "en": "Bagaimana 'Touch Switch' Dibuat Dengan Multivibrator?", "id": "Sentuhan Memicu Monostabil Atau Mengubah Keadaan Bistabil." },
    { "en": "Mengapa Sentuhan Bisa Memicu Sirkuit?", "id": "Tubuh Manusia Bertindak Sebagai Antena Pengambil Derau." },
    { "en": "Apa Itu Sirkuit 'Divide-by-N'?", "id": "Sirkuit Yang Membagi Frekuensi Input Dengan N." },
    { "en": "Bagaimana Cara Membuat Sirkuit 'Divide-by-N'?", "id": "Menggunakan Pencacah (Counter) Dan Logika Tambahan." },
    { "en": "Berapa Frekuensi Maksimum Operasi IC 555?", "id": "Sekitar Lima Ratus Kilo Hertz (500 KHz)." },
    { "en": "Berapa Frekuensi Maksimum Operasi Flip-Flop TTL?", "id": "Beberapa Puluh Mega Hertz (MHz)." },
    { "en": "Berapa Frekuensi Maksimum Operasi Flip-Flop ECL?", "id": "Ratusan Mega Hertz Hingga Giga Hertz (GHz)." },
    { "en": "Apa Itu ECL (Emitter-Coupled Logic)?", "id": "Keluarga Logika Berkecepatan Sangat Tinggi." },
    { "en": "Apa Kerugian ECL?", "id": "Konsumsi Daya Sangat Tinggi." },
    { "en": "Apa Itu Gerbang Logika?", "id": "Blok Bangunan Elektronik Untuk Operasi Logika Boolean." },
    { "en": "Contoh Gerbang Logika?", "id": "AND, OR, NOT, NAND, NOR, XOR." },
    { "en": "Gerbang Apa Yang Disebut Gerbang Universal?", "id": "NAND Dan NOR." },
    { "en": "Mengapa Disebut Universal?", "id": "Karena Semua Fungsi Logika Lain Bisa Dibuat Darinya." },
    { "en": "Bagaimana Membuat Inverter Dari Gerbang NAND?", "id": "Hubungkan Kedua Inputnya Bersama." },
    { "en": "Bagaimana Membuat Latch Dari Gerbang NAND?", "id": "Dengan Menyilangkan Dua Gerbang NAND." },
    { "en": "Apakah Mungkin Membuat Osilator Dari Gerbang NAND?", "id": "Ya, Mirip Dengan Osilator Gelang Inverter." },
    { "en": "Apa Itu 'Metronome' Elektronik?", "id": "Alat Yang Menghasilkan Denyut Audio Berirama." },
    { "en": "Metronome Elektronik Menggunakan Multivibrator Apa?", "id": "Astabil Untuk Menghasilkan Denyut." },
    { "en": "Bagaimana Mengatur Tempo Metronome?", "id": "Mengubah Frekuensi Astabil Dengan Potensiometer." },
    { "en": "Apa Itu Generator Nada (Tone Generator)?", "id": "Sirkuit Untuk Menghasilkan Frekuensi Audio." },
    { "en": "Generator Nada Sederhana Menggunakan Apa?", "id": "Multivibrator Astabil Yang Frekuensinya Di Rentang Audio." },
    { "en": "Rentang Frekuensi Audio Manusia?", "id": "Dua Puluh Hertz Hingga Dua Puluh Kilo Hertz." },
    { "en": "Apa Itu 'Decoupling Capacitor'?", "id": "Kapasitor Untuk Menstabilkan Tegangan Catu Daya." },
    { "en": "Di Mana 'Decoupling Capacitor' Diletakkan?", "id": "Sangat Dekat Dengan Pin Catu Daya IC." },
    { "en": "Mengapa 'Decoupling' Penting Untuk Sirkuit Digital?", "id": "Sirkuit Digital Menarik Arus Dalam Pulsa Cepat." },
    { "en": "Pulsa Arus Cepat Menyebabkan Apa?", "id": "Penurunan Tegangan Sesaat ('Voltage Droop')." },
    { "en": "Apa Itu 'Bypass Capacitor'?", "id": "Istilah Lain Untuk 'Decoupling Capacitor'." },
    { "en": "Nilai Kapasitor 'Decoupling' Yang Umum?", "id": "Nol Koma Satu Mikrofarad (0.1 µF)." },
    { "en": "Jenis Kapasitor Apa Yang Baik Untuk 'Decoupling'?", "id": "Kapasitor Keramik." },
    { "en": "Mengapa Keramik?", "id": "Memiliki Induktansi Parasitik Yang Rendah." },
    { "en": "Apa Itu 'Logic Probe'?", "id": "Alat Uji Sederhana Untuk Melihat Level Logika." },
    { "en": "Indikator Apa Yang Ada Di 'Logic Probe'?", "id": "LED Untuk Logika Tinggi, Rendah, Dan Pulsa." },
    { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Canggih Untuk Menganalisis Banyak Sinyal Digital." },
    { "en": "Perbedaan Utama Dengan Osiloskop?", "id": "Menganalisis Domain Logika (1/0), Bukan Analog." },
    { "en": "Apa Yang Terjadi Pada Durasi Monostabil Jika Kapasitor Bocor?", "id": "Durasinya Akan Lebih Pendek Dari Perhitungan." },
    { "en": "Apa Itu 'Soak Time' Kapasitor?", "id": "Waktu Yang Dibutuhkan Kapasitor Untuk Terisi Penuh." },
    { "en": "Apa Itu 'Dielectric Absorption'?", "id": "Efek Kapasitor 'Mengingat' Tegangan Sebelumnya." },
    { "en": "Efek Ini Mempengaruhi Aplikasi Apa?", "id": "Aplikasi Pewaktuan Dan 'Sample and Hold' Presisi." },
    { "en": "Apa Jenis Kapasitor Terbaik Untuk Pewaktuan?", "id": "Kapasitor Film (Polystyrene, Polypropylene)." },
    { "en": "Mengapa Kapasitor Elektrolit Kurang Baik Untuk Pewaktuan?", "id": "Memiliki Toleransi Besar Dan Arus Bocor Tinggi." },
    { "en": "Apa Itu 'Temperature Coefficient' Resistor?", "id": "Seberapa Besar Nilai Resistansi Berubah Dengan Suhu." },
    { "en": "Apa Itu 'Voltage Controlled Amplifier' (VCA)?", "id": "Penguatan Dikontrol Oleh Tegangan Eksternal." },
    { "en": "Apa Itu 'Analog Switch'?", "id": "Saklar Elektronik Untuk Melewatkan Sinyal Analog." },
    { "en": "Contoh IC 'Analog Switch'?", "id": "IC Seri 4066." },
    { "en": "'Analog Switch' Dikontrol Oleh Sinyal Apa?", "id": "Sinyal Logika Digital." },
    { "en": "Apa Itu 'Synchronizer'?", "id": "Sirkuit Untuk Mensinkronkan Sinyal Asinkron Dengan Clock." },
    { "en": "Bagaimana Cara Membuat 'Synchronizer' Sederhana?", "id": "Menggunakan Dua Flip-Flop Secara Berantai." },
    { "en": "Masalah Apa Yang Dihadapi 'Synchronizer'?", "id": "Risiko Metastabilitas." },
    { "en": "Apa Itu 'Mean Time Between Failures' (MTBF)?", "id": "Waktu Rata-rata Sebelum Terjadi Kegagalan." },
    { "en": "MTBF Metastabilitas Bergantung Pada Apa?", "id": "Frekuensi Clock Dan Frekuensi Data Asinkron." },
    { "en": "Apa Itu 'Finite State Machine' (FSM)?", "id": "Nama Lain Untuk 'State Machine'." },
    { "en": "Dua Jenis FSM?", "id": "Moore Dan Mealy." },
    { "en": "Perbedaan FSM Moore Dan Mealy?", "id": "Bagaimana Output Dihasilkan." },
    { "en": "Output FSM Moore Bergantung Pada Apa?", "id": "Hanya Pada Keadaan Saat Ini (Current State)." },
    { "en": "Output FSM Mealy Bergantung Pada Apa?", "id": "Keadaan Saat Ini Dan Input Saat Ini." },
    { "en": "Mana Yang Biasanya Lebih Cepat?", "id": "Mesin Mealy." },
    { "en": "Apa Itu 'One-Shot'?", "id": "Istilah Populer Untuk Multivibrator Monostabil." },
    { "en": "Mengapa Multivibrator Penting Dalam Sejarah Komputer?", "id": "Flip-Flop Adalah Dasar Komputer Digital Pertama." },
    { "en": "Komputer ENIAC Menggunakan Apa?", "id": "Ribuan Flip-Flop Berbasis Tabung Vakum." },
    { "en": "Kapan Transistor Ditemukan?", "id": "Tahun 1947 Di Bell Labs." },
    { "en": "Kapan Sirkuit Terpadu (IC) Ditemukan?", "id": "Akhir Tahun 1950-an." },
    { "en": "Apa Dampak IC Pada Desain Multivibrator?", "id": "Membuatnya Jauh Lebih Kecil, Murah, Dan Andal." },
    { "en": "Apa Itu Hukum Moore?", "id": "Jumlah Transistor Pada Chip Berlipat Ganda Setiap Dua Tahun." },
    { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Yang Bisa Diprogram Untuk Mengimplementasikan Sirkuit Digital." },
    { "en": "Bisakah Multivibrator Diimplementasikan Di FPGA?", "id": "Ya, Dengan Mendeskripsikannya Menggunakan Bahasa Hardware." },
    { "en": "Contoh Bahasa Deskripsi Hardware (HDL)?", "id": "VHDL Dan Verilog." },
    { "en": "Apa Itu 'Latching'?", "id": "Proses Menyimpan Sebuah Nilai." },
    { "en": "Multivibrator Mana Yang Melakukan 'Latching'?", "id": "Bistabil (Latch Atau Flip-Flop)." },
    { "en": "Apa Itu 'Astable'?", "id": "Kata Latin Yang Berarti Tidak Stabil." },
    { "en": "Apa Itu 'Mono'?", "id": "Kata Latin Yang Berarti Satu." },
    { "en": "Apa Itu 'Bi'?", "id": "Kata Latin Yang Berarti Dua." },
    { "en": "Jadi, Monostabil Berarti Apa?", "id": "Memiliki Satu Keadaan Stabil." },
    { "en": "Apa Itu 'Quasi-Stable'?", "id": "Tampak Stabil Tetapi Hanya Untuk Sementara." },
    { "en": "Keadaan Monostabil Yang Sementara Disebut Apa?", "id": "Keadaan Kuasi-Stabil." },
    { "en": "Apa Yang Membawa Monostabil Kembali Ke Keadaan Stabil?", "id": "Proses Pengisian/Pengosongan Kapasitor." },
    { "en": "Apa Yang Menjaga Bistabil Dalam Satu Keadaan?", "id": "Loop Umpan Balik Positif Yang Mengunci Diri." },
    { "en": "Apa Perlu Energi Untuk Menjaga Keadaan Bistabil?", "id": "Ya, Catu Daya Harus Tetap Menyala." },
    { "en": "Apa Itu Memori Non-Volatil?", "id": "Memori Yang Menyimpan Data Tanpa Catu Daya." },
    { "en": "Contoh Memori Non-Volatil?", "id": "Flash Drive, SSD, ROM." },
    { "en": "Apakah Flip-Flop Termasuk Memori Non-Volatil?", "id": "Tidak, Itu Adalah Memori Volatil." },
    { "en": "Apa Itu 'Power-Up State' Flip-Flop?", "id": "Keadaan Awal Saat Daya Dinyalakan." },
    { "en": "Apakah 'Power-Up State' Dapat Diprediksi?", "id": "Tidak Selalu, Bisa Acak." },
    { "en": "Mengapa Perlu Rangkaian Reset?", "id": "Untuk Memastikan Keadaan Awal Yang Diketahui." },
    { "en": "Apa Itu Osilator Terkendali Tegangan?", "id": "VCO (Voltage-Controlled Oscillator)." },
    { "en": "Bagaimana Pin 5 IC 555 Mengontrol Frekuensi?", "id": "Mengubah Level Tegangan Ambang Atas (2/3 Vcc)." },
    { "en": "Jika Tegangan Pin 5 Naik, Frekuensi Astabil Bagaimana?", "id": "Frekuensi Akan Menurun." },
    { "en": "Jika Tegangan Pin 5 Turun, Frekuensi Astabil Bagaimana?", "id": "Frekuensi Akan Meningkat." },
    { "en": "Rentang Tegangan Kontrol Pin 5?", "id": "Dari Mendekati Ground Hingga Vcc." },
    { "en": "Apa Itu 'Frequency Modulation' (FM)?", "id": "Memodulasi Frekuensi Gelombang Pembawa." },
    { "en": "VCO Adalah Komponen Kunci Dalam Modulasi Apa?", "id": "Modulasi Frekuensi (FM)." },
    { "en": "Apa Itu 'Frequency Shift Keying' (FSK)?", "id": "Merepresentasikan Data Biner Dengan Dua Frekuensi Berbeda." },
    { "en": "Bagaimana Membuat Generator FSK Sederhana?", "id": "Menggunakan VCO Dan Mengalihkan Tegangan Kontrolnya." },
    { "en": "Apa Itu Modem Awal (Acoustic Coupler)?", "id": "Menggunakan FSK Untuk Mengirim Data Melalui Telepon." },
    { "en": "Apa Itu 'Settling Time' Osilator?", "id": "Waktu Yang Dibutuhkan Frekuensi Untuk Stabil." },
    { "en": "Apa Itu 'Phase Noise'?", "id": "Derau Dalam Fasa Sinyal Osilator." },
    { "en": "Apa Dampak 'Phase Noise'?", "id": "Membatasi Kinerja Sistem Komunikasi." },
    { "en": "Apa Itu 'Harmonics'?", "id": "Komponen Frekuensi Kelipatan Dari Frekuensi Fundamental." },
    { "en": "Apakah Gelombang Kotak Memiliki Harmonisa?", "id": "Ya, Memiliki Banyak Harmonisa Ganjil." },
    { "en": "Bagaimana Amplitudo Harmonisa Gelombang Kotak?", "id": "Amplitudonya Berkurang Seiring Kenaikan Frekuensi." },
    { "en": "Deret Apa Yang Mendeskripsikan Harmonisa Ini?", "id": "Deret Fourier." },
    { "en": "Gelombang Segitiga Memiliki Harmonisa Apa?", "id": "Juga Hanya Harmonisa Ganjil." },
    { "en": "Bagaimana Harmonisa Gelombang Segitiga Dibanding Kotak?", "id": "Amplitudonya Turun Lebih Cepat." },
    { "en": "Gelombang Apa Yang Tidak Memiliki Harmonisa?", "id": "Gelombang Sinus Murni." },
    { "en": "Bagaimana Cara Mengubah Gelombang Kotak Menjadi Sinus?", "id": "Menggunakan Filter Lolos-Rendah Yang Baik." },
    { "en": "Fungsi Filter Lolos-Rendah Dalam Hal Ini?", "id": "Menghilangkan Semua Komponen Harmonisa." },
    { "en": "Apa Itu 'Overshoot' dan 'Ringing'?", "id": "Lonjakan Dan Osilasi Pada Tepi Pulsa." },
    { "en": "Penyebab 'Overshoot'?", "id": "Induktansi Dan Kapasitansi Parasitik Di Jalur Sinyal." },
    { "en": "Bagaimana Cara Mengurangi 'Overshoot'?", "id": "Menggunakan Teknik Terminasi Yang Tepat." },
    { "en": "Apa Itu Terminasi Seri?", "id": "Menambahkan Resistor Seri Di Dekat Sumber." },
    { "en": "Apa Itu Terminasi Paralel?", "id": "Menambahkan Resistor Paralel Di Ujung Jalur." },
    { "en": "Apa Itu 'Ground Plane'?", "id": "Lapisan Tembaga Besar Di PCB Yang Terhubung Ke Ground." },
    { "en": "Manfaat 'Ground Plane'?", "id": "Mengurangi Derau Dan Memberikan Jalur Kembali Arus Yang Baik." },
    { "en": "Apa Itu 'Crosstalk'?", "id": "Interferensi Antara Jalur Sinyal Yang Berdekatan." },
    { "en": "Penyebab 'Crosstalk'?", "id": "Kopling Kapasitif Dan Induktif Antar Jalur." },
    { "en": "Bagaimana Mengurangi 'Crosstalk'?", "id": "Menjaga Jarak Antar Jalur, Menggunakan 'Ground Plane'." },
    { "en": "Apa Itu 'Logic Family'?", "id": "Sekelompok IC Digital Dengan Karakteristik Serupa." },
    { "en": "Karakteristik Apa Yang Sama?", "id": "Tegangan Catu Daya, Level Logika, Kecepatan." },
    { "en": "Bisakah Keluarga Logika Berbeda Dihubungkan Langsung?", "id": "Tidak Selalu, Mungkin Perlu Sirkuit Antarmuka." },
    { "en": "Apa Itu 'Level Shifter'?", "id": "Sirkuit Untuk Mengubah Level Tegangan Logika." },
    { "en": "Apa Itu 'H-Bridge'?", "id": "Sirkuit Untuk Mengontrol Arah Putaran Motor DC." },
    { "en": "Bagaimana 'H-Bridge' Dikontrol?", "id": "Menggunakan Sinyal PWM Dari Multivibrator Astabil." },
    { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Yang Berputar Dalam Langkah-langkah Diskrit." },
    { "en": "Bagaimana Mengontrol 'Stepper Motor'?", "id": "Dengan Memberikan Urutan Pulsa Ke Lilitannya." },
    { "en": "Urutan Pulsa Ini Dihasilkan Oleh Apa?", "id": "Sirkuit Logika Sekuensial (Pencacah/State Machine)." },
    { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Dengan Umpan Balik Posisi Internal." },
    { "en": "Bagaimana Mengontrol 'Servo Motor'?", "id": "Dengan Mengubah Lebar Pulsa Sinyal Kontrol." },
    { "en": "Sinyal Kontrol Servo Dihasilkan Oleh Apa?", "id": "Multivibrator Monostabil Atau Astabil (Mode PWM)." },
    { "en": "Lebar Pulsa Tipikal Untuk Servo?", "id": "Satu Hingga Dua Milidetik." },
    { "en": "Apa Itu 'Dead Time'?", "id": "Penundaan Kecil Untuk Mencegah Hubung Singkat." },
    { "en": "Di Mana 'Dead Time' Diperlukan?", "id": "Di 'H-Bridge' Dan Sirkuit 'Half-Bridge'." },
    { "en": "Apa Itu 'Shoot-through'?", "id": "Kondisi Hubung Singkat Saat Dua Saklar Vertikal Menyala." },
    { "en": "Bagaimana Cara Menghasilkan 'Dead Time'?", "id": "Menggunakan Logika Khusus Atau Sirkuit RC." },
    { "en": "Apa Itu 'Bang-Bang Controller'?", "id": "Sistem Kontrol Sederhana Dengan Dua Keadaan (ON/OFF)." },
    { "en": "Contoh 'Bang-Bang Controller'?", "id": "Termostat Pada AC Atau Kulkas." },
    { "en": "Apakah Komparator Dengan Histeresis Adalah 'Bang-Bang Controller'?", "id": "Ya, Benar." },
    { "en": "Apa Itu 'Schottky Diode'?", "id": "Dioda Dengan Jatuh Tegangan Maju Rendah." },
    { "en": "Apa Keuntungan 'Schottky Diode'?", "id": "Waktu Pemulihan Sangat Cepat." },
    { "en": "Di Mana 'Schottky Diode' Digunakan?", "id": "Logika Kecepatan Tinggi, Catu Daya Switching." },
    { "en": "Apa Itu 'Schottky Transistor'?", "id": "Transistor BJT Dengan Dioda Schottky Internal." },
    { "en": "Tujuan Dioda Schottky Di Transistor?", "id": "Mencegah Transistor Masuk Ke Saturasi Penuh." },
    { "en": "Apa Hasilnya?", "id": "Waktu Switching Menjadi Jauh Lebih Cepat." },
    { "en": "Keluarga Logika TTL Mana Yang Menggunakan Ini?", "id": "Schottky TTL (Seri 74S) Dan Low-power Schottky (74LS)." },
    { "en": "Apa Itu 'Optocoupler'?", "id": "Mengirim Sinyal Menggunakan Cahaya Untuk Isolasi Listrik." },
    { "en": "Komponen Internal 'Optocoupler'?", "id": "Sebuah LED Dan Sebuah Fototransistor." },
    { "en": "Apa Tujuan Isolasi Listrik?", "id": "Keselamatan Dan Mencegah 'Ground Loop'." },
    { "en": "Apa Itu 'Solid State Relay' (SSR)?", "id": "Relay Elektronik Tanpa Bagian Bergerak." },
    { "en": "Bagaimana SSR Bekerja?", "id": "Menggunakan 'Optocoupler' Dan TRIAC Atau MOSFET." },
    { "en": "Apa Keuntungan SSR Dibanding Relay Mekanis?", "id": "Lebih Cepat, Tahan Lama, Tidak Berisik." },
    { "en": "Apa Itu Sirkuit 'Crowbar'?", "id": "Sirkuit Proteksi Tegangan Berlebih." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Membuat Hubung Singkat Disengaja Untuk Membakar Sekring." },
    { "en": "Komponen Kunci Sirkuit 'Crowbar'?", "id": "SCR (Silicon-Controlled Rectifier)." },
    { "en": "Apa Itu 'Current Limiter'?", "id": "Sirkuit Yang Membatasi Arus Output Maksimum." },
    { "en": "Apa Itu 'Foldback Current Limiting'?", "id": "Mengurangi Arus Dan Tegangan Saat Terjadi Beban Berlebih." },
    { "en": "Apa Itu 'Thermal Shutdown'?", "id": "Fitur Proteksi Yang Mematikan IC Jika Terlalu Panas." },
    { "en": "Banyak IC Regulator Dan Op-Amp Memiliki Fitur Ini?", "id": "Ya, Sebagai Fitur Keamanan Standar." },
    { "en": "Apa Itu 'Latch-up' Dalam CMOS?", "id": "Kondisi Hubung Singkat Internal Yang Merusak." },
    { "en": "Penyebab 'Latch-up'?", "id": "Tegangan Input/Output Melebihi Batas Catu Daya." },
    { "en": "Bagaimana Mencegah 'Latch-up'?", "id": "Desain Sirkuit Yang Baik Dan Proteksi Input." },
    { "en": "Apa Itu 'Gated Oscillator'?", "id": "Osilator Yang Bisa Dihidupkan Dan Dimatikan." },
    { "en": "Bagaimana Cara Membuat 'Gated Oscillator'?", "id": "Menambahkan Gerbang Logika Di Jalur Umpan Balik Astabil." },
    { "en": "Apa Itu 'Duty Cycle Controller'?", "id": "Sirkuit Untuk Mengatur Siklus Kerja Secara Variabel." },
    { "en": "Bagaimana Caranya Dengan IC 555?", "id": "Menggunakan Dioda Dan Potensiometer Pada Jalur Pengisian/Pengosongan." },
    { "en": "Apa Itu 'Pulse-Skipping Modulation'?", "id": "Metode Kontrol Daya Dengan Melewatkan Beberapa Pulsa." },
    { "en": "Apa Itu 'Sequential Timer'?", "id": "Timer Yang Mengaktifkan Beberapa Output Secara Berurutan." },
    { "en": "Bagaimana Cara Membuat 'Sequential Timer'?", "id": "Menggabungkan Astabil (Clock) Dan Pencacah/Decoder." },
    { "en": "Apa Itu 'Cascading Timers'?", "id": "Output Satu Timer Memicu Timer Berikutnya." },
    { "en": "Tujuan 'Cascading Timers'?", "id": "Membuat Penundaan Waktu Yang Sangat Panjang." },
    { "en": "Apa Itu 'Frequency Doubler'?", "id": "Sirkuit Yang Menggandakan Frekuensi Input." },
    { "en": "Bagaimana Cara Membuat 'Frequency Doubler' Sederhana?", "id": "Menggunakan Gerbang XOR Dan Penundaan RC." },
    { "en": "Apa Itu 'Phase Detector'?", "id": "Sirkuit Yang Outputnya Bergantung Beda Fasa Dua Input." },
    { "en": "Gerbang Logika Apa Yang Bisa Jadi 'Phase Detector'?", "id": "Gerbang XOR." },
    { "en": "Apa Itu 'Lock Range' PLL?", "id": "Rentang Frekuensi Di Mana PLL Bisa Tetap Terkunci." },
    { "en": "Apa Itu 'Capture Range' PLL?", "id": "Rentang Frekuensi Di Mana PLL Bisa Mulai Mengunci." },
    { "en": "Mana Yang Lebih Lebar, 'Lock Range' Atau 'Capture Range'?", "id": "'Lock Range' Selalu Lebih Lebar." },
    { "en": "Filter Loop Dalam PLL Bertipe Apa?", "id": "Filter Lolos-Rendah." },
    { "en": "Fungsi Filter Loop?", "id": "Menghilangkan Komponen AC Dari Output Phase Detector." },
    { "en": "Apa Itu 'Jitter' Dalam Konteks Sinyal Digital?", "id": "Penyimpangan Tepi Pulsa Dari Posisi Idealnya." },
    { "en": "Apa Dampak 'Jitter'?", "id": "Menyebabkan Kesalahan Bit Dalam Komunikasi Data." },
    { "en": "Apa Itu 'Clock Recovery Circuit'?", "id": "Mengekstrak Sinyal Clock Dari Aliran Data." },
    { "en": "Mengapa 'Clock Recovery' Penting?", "id": "Data Serial Asinkron Tidak Memiliki Jalur Clock Terpisah." },
    { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Untuk Deteksi Kesalahan Sederhana." },
    { "en": "Apa Itu 'Checksum'?", "id": "Metode Deteksi Kesalahan Yang Lebih Kuat." },
    { "en": "Apa Itu CRC (Cyclic Redundancy Check)?", "id": "Metode Deteksi Kesalahan Yang Sangat Andal." },
    { "en": "CRC Diimplementasikan Menggunakan Apa?", "id": "Register Geser Dengan Umpan Balik (LFSR)." },
    { "en": "Apa Itu LFSR (Linear Feedback Shift Register)?", "id": "Register Geser Penghasil Urutan Pseudo-Acak." },
    { "en": "Apa Itu 'Pseudo-Random'?", "id": "Tampak Acak Tetapi Sebenarnya Deterministik Dan Berulang." },
    { "en": "Aplikasi LFSR?", "id": "CRC, Pengujian Sirkuit, Enkripsi Aliran." },
    { "en": "Apa Itu 'White Noise'?", "id": "Derau Acak Dengan Kepadatan Spektral Datar." },
    { "en": "Apa Itu 'Pink Noise'?", "id": "Derau Dengan Kepadatan Spektral Menurun 3dB Per Oktaf." },
    { "en": "Generator Derau Digital Menggunakan Apa?", "id": "LFSR." },
    { "en": "Apa Itu 'Glitch' Pada Output Decoder?", "id": "Pulsa Palsu Sesaat Saat Input Berubah." },
    { "en": "Penyebab 'Glitch' Decoder?", "id": "Penundaan Yang Berbeda Pada Jalur Logika Internal." },
    { "en": "Bagaimana Mengatasi 'Glitch'?", "id": "Menggunakan 'Strobing' Atau Mendaftarkan Output." },
    { "en": "Apa Itu 'Strobing'?", "id": "Mencuplik Output Hanya Setelah Stabil." },
    { "en": "Apa Itu 'Metastability Hardening'?", "id": "Teknik Desain Untuk Mengurangi Peluang Kegagalan Metastabil." },
    { "en": "Contoh Tekniknya?", "id": "Menggunakan Synchronizer Multi-Tahap." },
    { "en": "Apa Itu 'Timing Analysis'?", "id": "Menganalisis Waktu Penundaan Dalam Sirkuit Digital." },
    { "en": "Tujuan 'Timing Analysis'?", "id": "Memastikan Sirkuit Akan Bekerja Pada Frekuensi Clock Target." },
    { "en": "Apa Itu 'Critical Path'?", "id": "Jalur Logika Dengan Penundaan Terpanjang." },
    { "en": "Frekuensi Maksimum Sirkuit Ditentukan Oleh Apa?", "id": "Penundaan Pada 'Critical Path'." },
    { "en": "Apa Itu 'Hold Violation'?", "id": "Ketika Data Berubah Terlalu Cepat Setelah Clock." },
    { "en": "Apa Itu 'Setup Violation'?", "id": "Ketika Data Berubah Terlalu Dekat Sebelum Clock." },
    { "en": "Mana Yang Lebih Sulit Diperbaiki, 'Setup' Atau 'Hold'?", "id": "'Hold Violation' Seringkali Lebih Sulit." },
    { "en": "Bagaimana Cara Memperbaiki 'Setup Violation'?", "id": "Mengurangi Logika, Menggunakan Gerbang Lebih Cepat, Menurunkan Clock." },
    { "en": "Bagaimana Cara Memperbaiki 'Hold Violation'?", "id": "Menambahkan Penyangga (Buffer) Untuk Menambah Penundaan." },
    { "en": "Apa Itu 'Clock Gating'?", "id": "Teknik Untuk Mematikan Clock Ke Bagian Sirkuit." },
    { "en": "Tujuan 'Clock Gating'?", "id": "Mengurangi Konsumsi Daya." },
    { "en": "Apa Risiko 'Clock Gating'?", "id": "Dapat Memperkenalkan 'Glitch' Jika Tidak Dirancang Dengan Baik." },
    { "en": "Apa Itu 'Power Gating'?", "id": "Mematikan Catu Daya Ke Blok Sirkuit Yang Tidak Aktif." },
    { "en": "Apa Itu 'Asynchronous Design'?", "id": "Desain Sirkuit Digital Tanpa Clock Global." },
    { "en": "Apa Keuntungan Desain Asinkron?", "id": "Potensi Konsumsi Daya Rendah Dan Kecepatan Rata-rata Tinggi." },
    { "en": "Apa Kerugian Desain Asinkron?", "id": "Jauh Lebih Sulit Untuk Dirancang Dan Diuji." },
    { "en": "Dunia Digital Sebagian Besar Sinkron Atau Asinkron?", "id": "Sebagian Besar Adalah Desain Sinkron." },
    { "en": "Apa Itu 'Handshaking'?", "id": "Protokol Sinyal Untuk Komunikasi Asinkron." },
    { "en": "Contoh Sinyal 'Handshaking'?", "id": "Request (REQ) Dan Acknowledge (ACK)." },
    { "en": "Apa Itu 'FIFO'?", "id": "First-In, First-Out Buffer." },
    { "en": "Fungsi 'FIFO'?", "id": "Menjembatani Antara Dua Domain Clock Yang Berbeda." },
    { "en": "Apa Itu 'Elasticity Buffer'?", "id": "Nama Lain Untuk FIFO." },
    { "en": "Bagaimana Memilih Nilai R Dan C?", "id": "Pilih C Terlebih Dahulu, Lalu Hitung R." },
    { "en": "Mengapa Memilih C Terlebih Dahulu?", "id": "Ketersediaan Nilai Kapasitor Lebih Terbatas." },
    { "en": "Apa Itu Kapasitor Tantalum?", "id": "Jenis Kapasitor Elektrolit Dengan Kinerja Baik." },
    { "en": "Keunggulan Kapasitor Tantalum?", "id": "Ukuran Kecil, Stabilitas Baik, ESR Rendah." },
    { "en": "Kelemahan Kapasitor Tantalum?", "id": "Sensitif Terhadap Pemasangan Polaritas Terbalik." },
    { "en": "Apa Itu 'Inrush Current Limiter'?", "id": "Sirkuit Untuk Membatasi Arus Saat Dinyalakan." },
    { "en": "Contoh 'Inrush Current Limiter'?", "id": "Termistor NTC (Negative Temperature Coefficient)." },
    { "en": "Bagaimana Cara Kerja Termistor NTC?", "id": "Resistansi Awal Tinggi, Menurun Saat Panas." },
    { "en": "Apa Itu 'Voltage Doubler'?", "id": "Sirkuit Untuk Menggandakan Tegangan DC." },
    { "en": "Bagaimana Cara Kerja 'Voltage Doubler'?", "id": "Menggunakan Dioda Dan Kapasitor (Charge Pump)." },
    { "en": "Apa Itu 'Charge Pump'?", "id": "Sirkuit Yang Menggunakan Kapasitor Untuk Menaikkan Tegangan." },
    { "en": "IC 555 Bisa Menjadi 'Charge Pump' Sederhana?", "id": "Ya, Untuk Menghasilkan Tegangan Negatif Kecil." },
    { "en": "Apa Itu 'Missing Cycle Detector'?", "id": "Nama Lain Untuk 'Missing Pulse Detector'." },
    { "en": "Apa Itu 'Time-Out' Dalam Perangkat Lunak?", "id": "Batas Waktu Untuk Sebuah Operasi." },
    { "en": "Bagaimana 'Time-Out' Diimplementasikan Dalam Hardware?", "id": "Menggunakan Timer Monostabil ('Watchdog')." },
    { "en": "Apa Itu 'Hysteresis'?", "id": "Ketergantungan Output Pada Sejarah Input." },
    { "en": "Di Mana Saja 'Hysteresis' Ditemukan?", "id": "Pemicu Schmitt, Magnetisme, Termostat." },
    { "en": "Manfaat 'Hysteresis'?", "id": "Memberikan Stabilitas Dan Kekebalan Terhadap Derau." },
    { "en": "Output Pemicu Schmitt Berayun Antara Apa?", "id": "Tegangan Saturasi Positif Dan Negatif Op-Amp." },
    { "en": "Apa Itu 'Window Comparator'?", "id": "Sirkuit Yang Mendeteksi Jika Tegangan Berada Dalam Rentang." },
    { "en": "Bagaimana Membuat 'Window Comparator'?", "id": "Menggunakan Dua Komparator Dan Gerbang Logika." },
    { "en": "Apa Itu 'Bar Graph Display Driver'?", "id": "IC Untuk Mengerakkan Tampilan LED Bar." },
    { "en": "Contoh IC nya?", "id": "LM3914." },
    { "en": "Prinsip Kerja LM3914?", "id": "Menggunakan Rangkaian Komparator Internal." },
    { "en": "Apa Itu 'Relaxation Oscillator'?", "id": "Osilator Yang Kerjanya Berdasarkan Pengisian/Pengosongan RC." },
    { "en": "Contoh Osilator Relaksasi?", "id": "Multivibrator Astabil." },
    { "en": "Apa Itu 'Feedback Oscillator'?", "id": "Osilator Yang Menggunakan Jaringan Umpan Balik Penentu Fasa." },
    { "en": "Contoh 'Feedback Oscillator'?", "id": "Osilator Jembatan Wien, Colpitts, Hartley." },
    { "en": "Osilator Ini Menghasilkan Gelombang Apa?", "id": "Gelombang Sinusoidal." },
    { "en": "Ringkasan: Multivibrator Astabil Memiliki Berapa Keadaan Stabil?", "id": "Nol." },
    { "en": "Ringkasan: Multivibrator Monostabil Memiliki Berapa Keadaan Stabil?", "id": "Satu." },
    { "en": "Ringkasan: Multivibrator Bistabil Memiliki Berapa Keadaan Stabil?", "id": "Dua." },
    { "en": "Ringkasan: Nama Lain Astabil?", "id": "'Free-Running'." },
    { "en": "Ringkasan: Nama Lain Monostabil?", "id": "'One-Shot'." },
    { "en": "Ringkasan: Nama Lain Bistabil?", "id": "'Flip-Flop' Atau 'Latch'." },
    { "en": "Ringkasan: Astabil Digunakan Untuk?", "id": "Menghasilkan Clock Atau Sinyal Periodik." },
    { "en": "Ringkasan: Monostabil Digunakan Untuk?", "id": "Pewaktuan Atau Menghasilkan Pulsa Tunggal." },
    { "en": "Ringkasan: Bistabil Digunakan Untuk?", "id": "Menyimpan Informasi (Memori)." },
    { "en": "Ringkasan: Prinsip Kerja Dasar?", "id": "Umpan Balik Positif Dan Jaringan Pewaktu RC." },
    { "en": "Apa Itu 'Digital Electronics'?", "id": "Bidang Elektronik Yang Bekerja Dengan Sinyal Diskrit." },
    { "en": "Apa Itu 'Analog Electronics'?", "id": "Bidang Elektronik Yang Bekerja Dengan Sinyal Kontinu." },
    { "en": "Multivibrator Adalah Jembatan Antara Keduanya?", "id": "Ya, Menggunakan Komponen Analog Untuk Membuat Sinyal Digital." },
    { "en": "Apa Itu 'Squaring Circuit'?", "id": "Sirkuit Yang Mengubah Sinyal (Misal: Sinus) Menjadi Kotak." },
    { "en": "Contoh 'Squaring Circuit'?", "id": "Pemicu Schmitt Atau Komparator." },
    { "en": "Apa Itu 'Clock Domain Crossing' (CDC)?", "id": "Mentransfer Data Antara Bagian Dengan Clock Berbeda." },
    { "en": "Apa Tantangan Dalam CDC?", "id": "Mencegah Metastabilitas." },
    { "en": "Sirkuit Apa Yang Digunakan Untuk CDC?", "id": "FIFO Asinkron Atau Synchronizer Dua-Tahap." },
    { "en": "Apa Itu 'Gray Code'?", "id": "Urutan Biner Di Mana Hanya Satu Bit Berubah." },
    { "en": "Mengapa 'Gray Code' Berguna?", "id": "Mencegah Kesalahan Pembacaan Pada Sensor Posisi." },
    { "en": "Bagaimana Pencacah 'Gray Code' Dibuat?", "id": "Menggunakan Flip-Flop Dan Logika XOR." },
    { "en": "Apa Itu 'Latency'?", "id": "Waktu Tunda Total Dari Input Ke Output." },
    { "en": "Apa Itu 'Throughput'?", "id": "Jumlah Data Yang Dapat Diproses Per Satuan Waktu." },
    { "en": "Apa Itu 'Pipelining'?", "id": "Teknik Untuk Meningkatkan 'Throughput' Dengan Membagi Tugas." },
    { "en": "Register Apa Yang Digunakan Dalam 'Pipelining'?", "id": "Register Antar Tahap (Berbasis Flip-Flop)." },
    { "en": "Apa Itu 'Microcontroller'?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Apa Saja Bagian Dari 'Microcontroller'?", "id": "CPU, Memori (RAM/Flash), Dan Periferal (Timer/ADC)." },
    { "en": "Timer Internal 'Microcontroller' Dibangun Dari Apa?", "id": "Pencacah (Counter) Berbasis Flip-Flop." },
    { "en": "Bagaimana 'Microcontroller' Menghasilkan Sinyal PWM?", "id": "Menggunakan Modul Timer Khusus." },
    { "en": "Apakah 'Microcontroller' Bisa Menggantikan Fungsi IC 555?", "id": "Ya, Untuk Banyak Aplikasi." },
    { "en": "Kapan Menggunakan IC 555 Daripada 'Microcontroller'?", "id": "Untuk Tugas Sangat Sederhana, Murah, Dan Cepat." },
    { "en": "Kapan Menggunakan 'Microcontroller'?", "id": "Ketika Dibutuhkan Fleksibilitas Dan Logika Kompleks." },
    { "en": "Apa Itu 'System on a Chip' (SoC)?", "id": "Sistem Komputer Lengkap Dalam Satu IC." },
    { "en": "Contoh SoC?", "id": "Prosesor Smartphone." },
    { "en": "Osilator Apa Yang Digunakan Di SoC?", "id": "Osilator Kristal Presisi Tinggi Dan PLL Internal." },
    { "en": "Apa Itu 'Debounce Delay'?", "id": "Waktu Tunggu Untuk Mengabaikan Pantulan Saklar." },
    { "en": "Bagaimana 'Debouncing' Dilakukan Di Perangkat Lunak?", "id": "Dengan Menunggu Sesaat Setelah Input Pertama Terdeteksi." },
    { "en": "Apa Itu 'Interrupt'?", "id": "Sinyal Yang Menginterupsi Eksekusi Normal Prosesor." },
    { "en": "Timer Bisa Menghasilkan 'Interrupt'?", "id": "Ya, Ketika Mencapai Nilai Tertentu." },
    { "en": "Apa Itu 'Real-Time Clock' (RTC)?", "id": "Sirkuit Untuk Melacak Waktu (Jam, Kalender)." },
    { "en": "Bagaimana RTC Tetap Berjalan Saat Daya Mati?", "id": "Menggunakan Baterai Cadangan Kecil." },
    { "en": "Osilator Apa Yang Digunakan RTC?", "id": "Osilator Kristal 32.768 KHz." },
    { "en": "Mengapa Frekuensi Tersebut Dipilih?", "id": "Mudah Dibagi Untuk Mendapatkan Denyut Satu Detik." },
    { "en": "32768 Adalah Pangkat Berapa Dari Dua?", "id": "Dua Pangkat Lima Belas (2^15)." },
    { "en": "Membagi Frekuensi Ini 15 Kali Menghasilkan Apa?", "id": "Satu Hertz (1 Hz)." },
    { "en": "Pembagian Frekuensi Dilakukan Oleh Apa?", "id": "Pencacah Biner 15-Bit." },
    { "en": "Apa Itu 'Time Constant'?", "id": "Ukuran Waktu Respon Rangkaian RC Atau RL." },
    { "en": "Dalam Multivibrator, 'Time Constant' Menentukan Apa?", "id": "Frekuensi Osilasi Atau Durasi Pulsa." },
    { "en": "Apa Itu 'Steady State'?", "id": "Kondisi Sirkuit Setelah Semua Transien Selesai." },
    { "en": "Keadaan Stabil Bistabil Adalah 'Steady State'?", "id": "Ya." },
    { "en": "Apakah Astabil Pernah Mencapai 'Steady State'?", "id": "Tidak, Selalu Dalam Keadaan Transien." },
    { "en": "Apa Itu 'Transient State'?", "id": "Keadaan Sementara Saat Sirkuit Berubah." },
    { "en": "Seluruh Operasi Monostabil Adalah 'Transient State'?", "id": "Ya, Setelah Dipicu Dan Sebelum Kembali Stabil." },
    { "en": "Sirkuit Logika Mana Yang Menyimpan Keadaan?", "id": "Sirkuit Sekuensial." },
    { "en": "Elemen Penyimpan Keadaan Adalah?", "id": "Flip-Flop Atau Latch." },
    { "en": "Apa Itu 'Clock Domain'?", "id": "Bagian Sirkuit Yang Beroperasi Dengan Clock Sama." },
    { "en": "Masalah Terjadi Ketika Data Melintasi Apa?", "id": "Batas Antar 'Clock Domain'." },
    { "en": "Apa Itu 'Power Supply Rejection Ratio' (PSRR)?", "id": "Kemampuan Sirkuit Menolak Derau Dari Catu Daya." },
    { "en": "PSRR Yang Baik Bernilai Bagaimana?", "id": "Tinggi (Dalam Satuan dB)." },
    { "en": "Bagaimana Meningkatkan PSRR?", "id": "Menggunakan 'Decoupling Capacitor' Yang Efektif." },
    { "en": "Apa Itu 'Electromagnetic Interference' (EMI)?", "id": "Gangguan Yang Disebabkan Oleh Radiasi Elektromagnetik." },
    { "en": "Sirkuit Digital Cepat Adalah Sumber EMI?", "id": "Ya, Karena Tepi Sinyal Yang Curam." },
    { "en": "Bagaimana Mengurangi EMI?", "id": "Layout PCB Yang Baik, Grounding, Shielding." },
    { "en": "Apa Itu 'Shielding'?", "id": "Membungkus Sirkuit Dengan Konduktor (Faraday Cage)." },
    { "en": "Apa Itu 'PCB'?", "id": "Printed Circuit Board." },
    { "en": "Apa Fungsi PCB?", "id": "Menghubungkan Komponen Elektronik Secara Fisik Dan Elektrik." },
    { "en": "Apa Itu 'Trace' Pada PCB?", "id": "Jalur Tembaga Yang Bertindak Sebagai Kawat." },
    { "en": "'Trace' Memiliki Resistansi, Kapasitansi, Dan Induktansi?", "id": "Ya, Terutama Pada Frekuensi Tinggi." },
    { "en": "Apa Itu 'Transmission Line'?", "id": "Struktur Untuk Memandu Gelombang Elektromagnetik." },
    { "en": "Kapan 'Trace' PCB Harus Dianggap 'Transmission Line'?", "id": "Ketika Panjangnya Sebanding Dengan Panjang Gelombang Sinyal." },
    { "en": "Apa Itu 'Impedance Matching'?", "id": "Menyesuaikan Impedansi Untuk Mencegah Refleksi." },
    { "en": "Refleksi Menyebabkan Apa Pada Sinyal Digital?", "id": "'Ringing' Dan Distorsi." },
    { "en": "Apa Itu 'Termination Resistor'?", "id": "Resistor Untuk 'Impedance Matching'." },
    { "en": "Di Mana Diletakkan?", "id": "Di Ujung Jalur Transmisi." },
    { "en": "Apa Itu 'Differential Signaling'?", "id": "Mengirim Sinyal Menggunakan Sepasang Kawat." },
    { "en": "Bagaimana Sinyal Pada Pasangan Kawat Tersebut?", "id": "Sama Besar Tetapi Berlawanan Polaritas." },
    { "en": "Apa Keuntungan 'Differential Signaling'?", "id": "Kekebalan Terhadap Derau Sangat Baik." },
    { "en": "Contoh Penggunaan 'Differential Signaling'?", "id": "USB, Ethernet, HDMI, LVDS." },
    { "en": "LVDS Adalah Singkatan Dari?", "id": "Low-Voltage Differential Signaling." },
    { "en": "Apa Itu 'Eye Diagram'?", "id": "Tampilan Osiloskop Untuk Menganalisis Kualitas Sinyal Digital." },
    { "en": "Bukaan Mata Yang Lebar Menandakan Apa?", "id": "Sinyal Berkualitas Baik Dengan Jitter Rendah." },
    { "en": "Apa Itu 'Bit Error Rate' (BER)?", "id": "Rasio Jumlah Bit Salah Terhadap Total Bit." },
    { "en": "BER Yang Baik Bernilai Bagaimana?", "id": "Sangat Rendah." },
    { "en": "Apakah Multivibrator Bisa Dibuat Dengan Komponen Mekanis?", "id": "Ya, Menggunakan Relay Dan Saklar (Desain Sangat Awal)." },
    { "en": "Apa Itu 'Bouncing' Pada Saklar?", "id": "Kontak Mekanis Yang Memantul Sebelum Stabil." },
    { "en": "Sirkuit 'Debouncer' Menggunakan Multivibrator Apa?", "id": "SR Latch Atau Monostabil." },
    { "en": "Mengapa LED Berkedip Pada Astabil BJT?", "id": "Karena Terhubung Ke Kolektor Salah Satu Transistor." },
    { "en": "Kapan LED Menyala?", "id": "Ketika Transistor Tersebut 'OFF' (Kolektor Tinggi)." },
    { "en": "Kapan LED Mati?", "id": "Ketika Transistor Tersebut 'ON' (Kolektor Rendah)." },
    { "en": "Frekuensi Kedipan Ditentukan Oleh Apa?", "id": "Konstanta Waktu Dari Kedua Jaringan RC." }




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
