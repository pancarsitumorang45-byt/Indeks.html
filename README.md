<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List & Quiz Interaktif SDLC</title>
    <!-- Memuat Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Memuat Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Font Awesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen flex flex-col antialiased selection:bg-indigo-100 selection:text-indigo-700">

    <!-- Header & Navigasi Utama -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-sm">
        <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between py-4 gap-4">
                <!-- Brand / Judul -->
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-xl bg-indigo-600 text-white flex items-center justify-center shadow-md shadow-indigo-200 shrink-0">
                        <i class="fa-solid fa-layer-group text-lg"></i>
                    </div>
                    <div>
                        <h1 class="text-lg sm:text-xl font-bold text-slate-900 leading-tight">Aplikasi SDLC & To-Do List</h1>
                        <p class="text-xs text-slate-500">Implementasi Siklus Hidup Pengembangan Perangkat Lunak</p>
                    </div>
                </div>

                <!-- Tab Navigasi -->
                <nav class="flex bg-slate-100 p-1 rounded-xl self-start sm:self-auto w-full sm:w-auto" aria-label="Navigasi Utama">
                    <button id="tab-todo" onclick="switchTab('todo')" class="flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 bg-white text-indigo-600 shadow-sm">
                        <i class="fa-solid fa-list-check"></i>
                        <span>Aplikasi To-Do</span>
                    </button>
                    <button id="tab-quiz" onclick="switchTab('quiz')" class="flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-600 hover:text-slate-900">
                        <i class="fa-solid fa-brain"></i>
                        <span>Quiz SDLC</span>
                        <span id="quiz-badge" class="bg-indigo-100 text-indigo-700 text-xs px-2 py-0.5 rounded-full font-bold">8 Soal</span>
                    </button>
                </nav>
            </div>
        </div>
    </header>

    <!-- Notifikasi Toast (Pengganti alert()) -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- Konten Utama -->
    <main class="flex-grow max-w-5xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- ========================================================= -->
        <!-- BAGIAN 1: APLIKASI TO-DO LIST & MATERI SDLC              -->
        <!-- ========================================================= -->
        <section id="section-todo" class="space-y-8">
            
            <!-- Kartu Utama Aplikasi To-Do List -->
            <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
                <div class="p-6 sm:p-8 border-b border-slate-100 bg-gradient-to-r from-indigo-50/70 via-white to-blue-50/50">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                        <div>
                            <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-indigo-100 text-indigo-700 mb-2">
                                <i class="fa-solid fa-database text-[10px]"></i> Penyimpanan LocalStorage Aktif
                            </span>
                            <h2 class="text-xl sm:text-2xl font-bold text-slate-900">Manajemen Daftar Tugas</h2>
                            <p class="text-sm text-slate-600 mt-1">Catat tugas Anda, tandai selesai, dan data otomatis tersimpan di memori browser.</p>
                        </div>
                        
                        <!-- Statistik Ringkas -->
                        <div class="flex items-center gap-3 bg-white px-4 py-3 rounded-xl border border-slate-200/80 shadow-xs shrink-0">
                            <div class="text-center px-2">
                                <p id="stat-total" class="text-lg font-bold text-slate-900">0</p>
                                <p class="text-[11px] font-medium text-slate-500 uppercase tracking-wider">Total</p>
                            </div>
                            <div class="h-8 w-px bg-slate-200"></div>
                            <div class="text-center px-2">
                                <p id="stat-pending" class="text-lg font-bold text-amber-600">0</p>
                                <p class="text-[11px] font-medium text-slate-500 uppercase tracking-wider">Belum</p>
                            </div>
                            <div class="h-8 w-px bg-slate-200"></div>
                            <div class="text-center px-2">
                                <p id="stat-done" class="text-lg font-bold text-emerald-600">0</p>
                                <p class="text-[11px] font-medium text-slate-500 uppercase tracking-wider">Selesai</p>
                            </div>
                        </div>
                    </div>

                    <!-- Form Input Tugas (Tahap Analisis & Desain UI) -->
                    <form id="todo-form" class="mt-6 flex flex-col sm:flex-row gap-3">
                        <div class="relative flex-grow">
                            <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none text-slate-400">
                                <i class="fa-solid fa-pen-to-square"></i>
                            </div>
                            <input 
                                type="text" 
                                id="task-input" 
                                maxlength="100"
                                placeholder="Masukkan nama tugas baru (maks. 100 karakter)..." 
                                class="w-full pl-11 pr-4 py-3.5 bg-white border border-slate-300 rounded-xl text-sm text-slate-800 placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all"
                                required
                            >
                        </div>
                        <button 
                            type="submit" 
                            class="px-6 py-3.5 bg-indigo-600 hover:bg-indigo-700 active:bg-indigo-800 text-white font-semibold text-sm rounded-xl shadow-sm shadow-indigo-200 transition-all flex items-center justify-center gap-2 shrink-0 cursor-pointer"
                        >
                            <i class="fa-solid fa-plus"></i>
                            <span>Tambah Tugas</span>
                        </button>
                    </form>
                </div>

                <!-- Filter & Aksi Daftar -->
                <div class="px-6 sm:px-8 py-3.5 bg-slate-50/80 border-b border-slate-200/80 flex flex-wrap items-center justify-between gap-3">
                    <div class="flex items-center gap-1.5" role="group" aria-label="Filter Tugas">
                        <button onclick="setFilter('all')" id="filter-all" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-indigo-600 text-white transition-colors">Semua</button>
                        <button onclick="setFilter('pending')" id="filter-pending" class="px-3 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:bg-slate-200/70 transition-colors">Belum Selesai</button>
                        <button onclick="setFilter('done')" id="filter-done" class="px-3 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:bg-slate-200/70 transition-colors">Selesai</button>
                    </div>
                    
                    <button onclick="clearCompletedTasks()" id="btn-clear-done" class="text-xs font-medium text-rose-600 hover:text-rose-700 hover:bg-rose-50 px-3 py-1.5 rounded-lg transition-colors hidden items-center gap-1.5">
                        <i class="fa-solid fa-broom"></i>
                        <span>Hapus yang Selesai</span>
                    </button>
                </div>

                <!-- Daftar Tugas -->
                <div class="p-6 sm:p-8">
                    <ul id="task-list" class="space-y-3">
                        <!-- Item tugas akan dirender di sini melalui JavaScript -->
                    </ul>

                    <!-- Tampilan Kosong -->
                    <div id="empty-state" class="text-center py-12 px-4 hidden">
                        <div class="w-16 h-16 bg-slate-100 text-slate-400 rounded-2xl flex items-center justify-center mx-auto mb-4">
                            <i class="fa-regular fa-clipboard text-2xl"></i>
                        </div>
                        <h3 class="text-base font-semibold text-slate-800">Tidak ada tugas</h3>
                        <p class="text-sm text-slate-500 mt-1 max-w-sm mx-auto">Tambahkan tugas baru menggunakan form di atas atau ubah filter tampilan daftar tugas Anda.</p>
                    </div>
                </div>
            </div>

            <!-- Panduan & Referensi Tahapan SDLC (Sesuai Dokumen) -->
            <div class="bg-white rounded-2xl border border-slate-200 shadow-sm p-6 sm:p-8">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 pb-5 border-b border-slate-100">
                    <div>
                        <h2 class="text-lg sm:text-xl font-bold text-slate-900 flex items-center gap-2">
                            <i class="fa-solid fa-book-open text-indigo-600"></i>
                            <span>Panduan Tahapan SDLC Aplikasi Ini</span>
                        </h2>
                        <p class="text-sm text-slate-500 mt-1">Ringkasan materi pembuatan aplikasi To-Do List berdasarkan siklus hidup pengembangan perangkat lunak.</p>
                    </div>
                    <button onclick="switchTab('quiz')" class="self-start sm:self-auto text-xs font-semibold text-indigo-600 hover:text-indigo-700 bg-indigo-50 hover:bg-indigo-100 px-3.5 py-2 rounded-xl transition-colors flex items-center gap-1.5">
                        <span>Uji Pemahaman di Quiz</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>

                <!-- Grid 6 Tahapan SDLC -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mt-6">
                    <!-- Tahap 1 -->
                    <div class="p-4 rounded-xl bg-slate-50 border border-slate-200/70 hover:border-indigo-200 transition-colors">
                        <div class="flex items-center gap-2.5 mb-2">
                            <span class="w-7 h-7 rounded-lg bg-indigo-600 text-white text-xs font-bold flex items-center justify-center">1</span>
                            <h3 class="font-bold text-slate-900 text-sm">Analisis Kebutuhan</h3>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">
                            Mengidentifikasi tujuan (mencatat & menandai tugas selesai), kebutuhan teknis web (HTML, CSS, JS), serta kebutuhan fungsional (form input, daftar, tombol selesai).
                        </p>
                    </div>

                    <!-- Tahap 2 -->
                    <div class="p-4 rounded-xl bg-slate-50 border border-slate-200/70 hover:border-indigo-200 transition-colors">
                        <div class="flex items-center gap-2.5 mb-2">
                            <span class="w-7 h-7 rounded-lg bg-indigo-600 text-white text-xs font-bold flex items-center justify-center">2</span>
                            <h3 class="font-bold text-slate-900 text-sm">Desain Sistem</h3>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">
                            Merancang UI (sketsa form & daftar), arsitektur sistem menggunakan memori browser (<strong>LocalStorage</strong>), dan diagram alur kerja pengguna.
                        </p>
                    </div>

                    <!-- Tahap 3 -->
                    <div class="p-4 rounded-xl bg-slate-50 border border-slate-200/70 hover:border-indigo-200 transition-colors">
                        <div class="flex items-center gap-2.5 mb-2">
                            <span class="w-7 h-7 rounded-lg bg-indigo-600 text-white text-xs font-bold flex items-center justify-center">3</span>
                            <h3 class="font-bold text-slate-900 text-sm">Pengembangan</h3>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">
                            Menulis kode dan mengintegrasikan komponen logika struktur data <code>Task</code> (nama tugas & status <code>isDone</code>) untuk tambah, tampilkan, dan tandai selesai.
                        </p>
                    </div>

                    <!-- Tahap 4 -->
                    <div class="p-4 rounded-xl bg-slate-50 border border-slate-200/70 hover:border-indigo-200 transition-colors">
                        <div class="flex items-center gap-2.5 mb-2">
                            <span class="w-7 h-7 rounded-lg bg-indigo-600 text-white text-xs font-bold flex items-center justify-center">4</span>
                            <h3 class="font-bold text-slate-900 text-sm">Pengujian</h3>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">
                            <strong>Fungsional:</strong> cek tambah & tandai selesai. <strong>Non-Fungsional:</strong> uji lintas browser (Chrome/Firefox) dan ketahanan data saat halaman dimuat ulang.
                        </p>
                    </div>

                    <!-- Tahap 5 -->
                    <div class="p-4 rounded-xl bg-slate-50 border border-slate-200/70 hover:border-indigo-200 transition-colors">
                        <div class="flex items-center gap-2.5 mb-2">
                            <span class="w-7 h-7 rounded-lg bg-indigo-600 text-white text-xs font-bold flex items-center justify-center">5</span>
                            <h3 class="font-bold text-slate-900 text-sm">Implementasi</h3>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">
                            Mempublikasikan aplikasi ke layanan hosting gratis (seperti <strong>GitHub Pages</strong>) serta menyediakan dokumentasi panduan penggunaan aplikasi.
                        </p>
                    </div>

                    <!-- Tahap 6 -->
                    <div class="p-4 rounded-xl bg-slate-50 border border-slate-200/70 hover:border-indigo-200 transition-colors">
                        <div class="flex items-center gap-2.5 mb-2">
                            <span class="w-7 h-7 rounded-lg bg-indigo-600 text-white text-xs font-bold flex items-center justify-center">6</span>
                            <h3 class="font-bold text-slate-900 text-sm">Pemeliharaan</h3>
                        </div>
                        <p class="text-xs text-slate-600 leading-relaxed">
                            Perbaikan bug berdasarkan masukan pengguna, penambahan fitur baru (seperti <strong>hapus tugas</strong> atau database server), serta optimasi kecepatan muat.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- ========================================================= -->
        <!-- BAGIAN 2: QUIZ INTERAKTIF SDLC & PEMROGRAMAN TO-DO LIST   -->
        <!-- ========================================================= -->
        <section id="section-quiz" class="hidden">
            <div class="max-w-3xl mx-auto">
                
                <!-- Layar Mulai Quiz -->
                <div id="quiz-start-screen" class="bg-white rounded-2xl border border-slate-200 shadow-sm p-8 sm:p-10 text-center">
                    <div class="w-16 h-16 rounded-2xl bg-indigo-100 text-indigo-600 flex items-center justify-center mx-auto mb-5">
                        <i class="fa-solid fa-graduation-cap text-3xl"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-slate-900">Quiz Interaktif: SDLC & Aplikasi To-Do List</h2>
                    <p class="text-slate-600 text-sm sm:text-base mt-2 max-w-xl mx-auto leading-relaxed">
                        Uji pemahaman Anda mengenai 6 tahapan SDLC (Analisis Kebutuhan hingga Pemeliharaan), arsitektur <code>localStorage</code>, serta logika struktur data program To-Do List.
                    </p>
                    
                    <div class="grid grid-cols-3 gap-3 max-w-md mx-auto my-7 text-left">
                        <div class="p-3.5 rounded-xl bg-slate-50 border border-slate-200/80">
                            <p class="text-xs text-slate-500">Jumlah Soal</p>
                            <p class="text-base font-bold text-slate-900 mt-0.5">8 Pilihan Ganda</p>
                        </div>
                        <div class="p-3.5 rounded-xl bg-slate-50 border border-slate-200/80">
                            <p class="text-xs text-slate-500">Umpan Balik</p>
                            <p class="text-base font-bold text-indigo-600 mt-0.5">Langsung</p>
                        </div>
                        <div class="p-3.5 rounded-xl bg-slate-50 border border-slate-200/80">
                            <p class="text-xs text-slate-500">Materi</p>
                            <p class="text-base font-bold text-slate-900 mt-0.5">SDLC & Kode</p>
                        </div>
                    </div>

                    <button onclick="startQuiz()" class="px-8 py-3.5 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold rounded-xl shadow-md shadow-indigo-200 transition-all inline-flex items-center gap-2 cursor-pointer">
                        <span>Mulai Quiz Sekarang</span>
                        <i class="fa-solid fa-play text-xs"></i>
                    </button>
                </div>

                <!-- Layar Pertanyaan Quiz -->
                <div id="quiz-question-screen" class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden hidden">
                    <!-- Header Progress -->
                    <div class="px-6 sm:px-8 py-5 bg-slate-50 border-b border-slate-200">
                        <div class="flex items-center justify-between text-sm font-semibold mb-2.5">
                            <span id="quiz-progress-text" class="text-indigo-600">Soal 1 dari 8</span>
                            <span id="quiz-score-live" class="text-slate-600">Skor: 0</span>
                        </div>
                        <div class="w-full h-2.5 bg-slate-200 rounded-full overflow-hidden">
                            <div id="quiz-progress-bar" class="h-full bg-indigo-600 rounded-full transition-all duration-300" style="width: 12.5%"></div>
                        </div>
                    </div>

                    <!-- Isi Pertanyaan -->
                    <div class="p-6 sm:p-8">
                        <span id="quiz-category" class="inline-block px-3 py-1 rounded-full text-xs font-semibold bg-indigo-50 text-indigo-700 mb-3">
                            Tahap SDLC
                        </span>
                        <h3 id="quiz-question-text" class="text-lg sm:text-xl font-bold text-slate-900 leading-snug">
                            Pertanyaan akan muncul di sini...
                        </h3>

                        <!-- Pilihan Jawaban -->
                        <div id="quiz-options" class="mt-6 space-y-3">
                            <!-- Tombol opsi dirender via JS -->
                        </div>

                        <!-- Kotak Umpan Balik / Pembahasan -->
                        <div id="quiz-feedback" class="mt-6 p-4 rounded-xl border hidden">
                            <div class="flex items-start gap-3">
                                <div id="feedback-icon" class="mt-0.5 text-lg shrink-0"></div>
                                <div>
                                    <h4 id="feedback-title" class="font-bold text-sm"></h4>
                                    <p id="feedback-explanation" class="text-xs sm:text-sm mt-1 leading-relaxed"></p>
                                </div>
                            </div>
                        </div>

                        <!-- Tombol Lanjut -->
                        <div class="mt-6 flex justify-end">
                            <button id="btn-next-question" onclick="nextQuestion()" class="px-6 py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold text-sm rounded-xl shadow-sm transition-all hidden items-center gap-2 cursor-pointer">
                                <span id="btn-next-text">Soal Berikutnya</span>
                                <i class="fa-solid fa-arrow-right"></i>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Layar Hasil Akhir Quiz -->
                <div id="quiz-result-screen" class="bg-white rounded-2xl border border-slate-200 shadow-sm p-8 sm:p-10 text-center hidden">
                    <div id="result-icon-wrapper" class="w-20 h-20 rounded-full flex items-center justify-center mx-auto mb-5">
                        <i id="result-icon" class="text-4xl"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-slate-900">Quiz Selesai!</h2>
                    <p id="result-message" class="text-slate-600 text-sm sm:text-base mt-1.5 max-w-md mx-auto"></p>

                    <div class="my-7 p-6 rounded-2xl bg-slate-50 border border-slate-200/80 max-w-sm mx-auto">
                        <p class="text-xs font-semibold uppercase tracking-wider text-slate-500">Skor Akhir Anda</p>
                        <div class="flex items-baseline justify-center gap-1 mt-2">
                            <span id="final-score" class="text-4xl sm:text-5xl font-extrabold text-indigo-600">0</span>
                            <span class="text-lg font-bold text-slate-400">/ 100</span>
                        </div>
                        <p id="final-correct-count" class="text-xs text-slate-600 mt-2 font-medium"></p>
                    </div>

                    <div class="flex flex-col sm:flex-row items-center justify-center gap-3">
                        <button onclick="startQuiz()" class="w-full sm:w-auto px-6 py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold text-sm rounded-xl shadow-sm transition-all flex items-center justify-center gap-2 cursor-pointer">
                            <i class="fa-solid fa-rotate-right"></i>
                            <span>Ulangi Quiz</span>
                        </button>
                        <button onclick="switchTab('todo')" class="w-full sm:w-auto px-6 py-3 bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold text-sm rounded-xl transition-all flex items-center justify-center gap-2 cursor-pointer">
                            <i class="fa-solid fa-list-check"></i>
                            <span>Kembali ke To-Do List</span>
                        </button>
                    </div>
                </div>

            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-slate-200 py-5 mt-12">
        <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row items-center justify-between gap-2 text-xs text-slate-500">
            <p>Implementasi Aplikasi Sederhana Berdasarkan SDLC (Analisis, Desain, Pengembangan, Pengujian, Implementasi, Pemeliharaan).</p>
            <p class="font-medium text-slate-600">Penyimpanan: Browser LocalStorage</p>
        </div>
    </footer>

    <!-- ========================================================= -->
    <!-- LOGIKA JAVASCRIPT (TO-DO LIST LOCALSTORAGE & QUIZ ENGINE) -->
    <!-- ========================================================= -->
    <script>
        // ---------------------------------------------------------
        // 1. MANAJEMEN NAVIGASI TAB & NOTIFIKASI
        // ---------------------------------------------------------
        function switchTab(tabName) {
            const sectionTodo = document.getElementById('section-todo');
            const sectionQuiz = document.getElementById('section-quiz');
            const tabTodo = document.getElementById('tab-todo');
            const tabQuiz = document.getElementById('tab-quiz');

            if (tabName === 'todo') {
                sectionTodo.classList.remove('hidden');
                sectionQuiz.classList.add('hidden');
                
                tabTodo.className = 'flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 bg-white text-indigo-600 shadow-sm';
                tabQuiz.className = 'flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-600 hover:text-slate-900';
            } else {
                sectionTodo.classList.add('hidden');
                sectionQuiz.classList.remove('hidden');
                
                tabQuiz.className = 'flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 bg-white text-indigo-600 shadow-sm';
                tabTodo.className = 'flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-600 hover:text-slate-900';
            }
        }

        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            const colors = type === 'success' 
                ? 'bg-slate-900 text-white border-slate-700' 
                : 'bg-rose-600 text-white border-rose-500';
            const icon = type === 'success' 
                ? '<i class="fa-solid fa-circle-check text-emerald-400"></i>' 
                : '<i class="fa-solid fa-circle-exclamation text-white"></i>';

            toast.className = `pointer-events-auto flex items-center gap-2.5 px-4 py-3 rounded-xl border shadow-lg text-xs sm:text-sm font-medium transform transition-all duration-300 translate-y-2 opacity-0 ${colors}`;
            toast.innerHTML = `${icon}<span>${message}</span>`;
            
            container.appendChild(toast);
            
            // Animasi masuk
            requestAnimationFrame(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            });

            // Hilang otomatis setelah 2.8 detik
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 2800);
        }

        // ---------------------------------------------------------
        // 2. LOGIKA APLIKASI TO-DO LIST (SESUAI DOKUMEN SDLC)
        // Struktur data menyerupai struct Task { task, isDone }
        // ---------------------------------------------------------
        const STORAGE_KEY = 'sdlc_todo_tasks_v1';
        let tasks = [];
        let currentFilter = 'all';

        // Memuat data dari LocalStorage (Arsitektur Sistem & Pengujian Non-Fungsional)
        function loadTasks() {
            try {
                const saved = localStorage.getItem(STORAGE_KEY);
                if (saved) {
                    tasks = JSON.parse(saved);
                } else {
                    // Data contoh awal agar pengguna langsung melihat cara kerja aplikasi
                    tasks = [
                        { id: 1, task: 'Mempelajari tahap Analisis Kebutuhan SDLC', isDone: 1 },
                        { id: 2, task: 'Menguji penyimpanan data di LocalStorage browser', isDone: 0 },
                        { id: 3, task: 'Mengerjakan Quiz Interaktif SDLC', isDone: 0 }
                    ];
                    saveTasks();
                }
            } catch (e) {
                console.error('Gagal memuat LocalStorage:', e);
                tasks = [];
            }
        }

        // Menyimpan data ke LocalStorage
        function saveTasks() {
            try {
                localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
            } catch (e) {
                showToast('Gagal menyimpan data ke memori browser.', 'error');
            }
        }

        // Fungsi Menambah Tugas (addTask)
        document.getElementById('todo-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const input = document.getElementById('task-input');
            const taskText = input.value.trim();

            if (!taskText) {
                showToast('Nama tugas tidak boleh kosong!', 'error');
                return;
            }

            if (tasks.length >= 100) {
                showToast('Daftar tugas sudah penuh (maksimal 100 tugas)!', 'error');
                return;
            }

            const newTask = {
                id: Date.now(),
                task: taskText,
                isDone: 0 // 0: Belum selesai, 1: Selesai
            };

            tasks.unshift(newTask);
            saveTasks();
            renderTasks();
            input.value = '';
            showToast('Tugas berhasil ditambahkan!');
        });

        // Fungsi Menandai Tugas Selesai / Batal Selesai (markTaskAsDone)
        function toggleTaskStatus(id) {
            const index = tasks.findIndex(t => t.id === id);
            if (index !== -1) {
                tasks[index].isDone = tasks[index].isDone === 1 ? 0 : 1;
                saveTasks();
                renderTasks();
                
                if (tasks[index].isDone === 1) {
                    showToast('Tugas telah ditandai sebagai selesai.');
                } else {
                    showToast('Status tugas dikembalikan ke Belum Selesai.');
                }
            }
        }

        // Fungsi Menghapus Tugas (Fitur Tahap Pemeliharaan SDLC)
        function deleteTask(id) {
            tasks = tasks.filter(t => t.id !== id);
            saveTasks();
            renderTasks();
            showToast('Tugas berhasil dihapus.');
        }

        // Menghapus semua tugas yang sudah selesai
        function clearCompletedTasks() {
            const initialCount = tasks.length;
            tasks = tasks.filter(t => t.isDone === 0);
            if (tasks.length < initialCount) {
                saveTasks();
                renderTasks();
                showToast('Semua tugas yang selesai telah dibersihkan.');
            }
        }

        // Mengatur filter tampilan
        function setFilter(filter) {
            currentFilter = filter;
            const buttons = ['all', 'pending', 'done'];
            buttons.forEach(btn => {
                const el = document.getElementById(`filter-${btn}`);
                if (btn === filter) {
                    el.className = 'px-3 py-1.5 rounded-lg text-xs font-semibold bg-indigo-600 text-white transition-colors';
                } else {
                    el.className = 'px-3 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:bg-slate-200/70 transition-colors';
                }
            });
            renderTasks();
        }

        // Fungsi Menampilkan Daftar Tugas (displayTasks)
        function renderTasks() {
            const listEl = document.getElementById('task-list');
            const emptyEl = document.getElementById('empty-state');
            const btnClearDone = document.getElementById('btn-clear-done');

            // Update statistik
            const totalCount = tasks.length;
            const doneCount = tasks.filter(t => t.isDone === 1).length;
            const pendingCount = totalCount - doneCount;

            document.getElementById('stat-total').textContent = totalCount;
            document.getElementById('stat-pending').textContent = pendingCount;
            document.getElementById('stat-done').textContent = doneCount;

            // Tampilkan tombol bersihkan jika ada tugas selesai
            if (doneCount > 0) {
                btnClearDone.classList.remove('hidden');
                btnClearDone.classList.add('flex');
            } else {
                btnClearDone.classList.add('hidden');
                btnClearDone.classList.remove('flex');
            }

            // Filter daftar
            const filteredTasks = tasks.filter(t => {
                if (currentFilter === 'pending') return t.isDone === 0;
                if (currentFilter === 'done') return t.isDone === 1;
                return true;
            });

            listEl.innerHTML = '';

            if (filteredTasks.length === 0) {
                emptyEl.classList.remove('hidden');
            } else {
                emptyEl.classList.add('hidden');
                
                filteredTasks.forEach((item, index) => {
                    const isCompleted = item.isDone === 1;
                    const li = document.createElement('li');
                    li.className = `group flex flex-col sm:flex-row sm:items-center justify-between gap-3 p-4 rounded-xl border transition-all duration-200 ${
                        isCompleted 
                        ? 'bg-emerald-50/40 border-emerald-200/80' 
                        : 'bg-white border-slate-200 hover:border-indigo-300 shadow-2xs'
                    }`;

                    // Escape HTML untuk mencegah XSS
                    const safeText = escapeHTML(item.task);

                    li.innerHTML = `
                        <div class="flex items-start sm:items-center gap-3.5 min-w-0">
                            <span class="w-6 h-6 rounded-full text-xs font-bold flex items-center justify-center shrink-0 mt-0.5 sm:mt-0 ${
                                isCompleted ? 'bg-emerald-100 text-emerald-700' : 'bg-slate-100 text-slate-600'
                            }">
                                ${index + 1}
                            </span>
                            <div class="min-w-0 flex-1">
                                <p class="text-sm sm:text-base font-medium break-words transition-all ${
                                    isCompleted ? 'line-through text-slate-400' : 'text-slate-800'
                                }">
                                    ${safeText}
                                </p>
                                <span class="inline-flex items-center gap-1 mt-1 text-[11px] font-semibold px-2 py-0.5 rounded-md ${
                                    isCompleted 
                                    ? 'bg-emerald-100 text-emerald-700' 
                                    : 'bg-amber-100 text-amber-700'
                                }">
                                    <i class="fa-solid ${isCompleted ? 'fa-check-circle' : 'fa-clock'} text-[10px]"></i>
                                    ${isCompleted ? 'Selesai' : 'Belum Selesai'}
                                </span>
                            </div>
                        </div>

                        <div class="flex items-center gap-2 self-end sm:self-center shrink-0">
                            <button 
                                onclick="toggleTaskStatus(${item.id})" 
                                class="px-3.5 py-2 rounded-lg text-xs font-semibold transition-all flex items-center gap-1.5 cursor-pointer ${
                                    isCompleted 
                                    ? 'bg-slate-100 hover:bg-slate-200 text-slate-700' 
                                    : 'bg-emerald-600 hover:bg-emerald-700 text-white shadow-xs'
                                }"
                                title="${isCompleted ? 'Batalkan status selesai' : 'Tandai tugas selesai'}"
                            >
                                <i class="fa-solid ${isCompleted ? 'fa-rotate-left' : 'fa-check'}"></i>
                                <span>${isCompleted ? 'Batal' : 'Selesai'}</span>
                            </button>
                            <button 
                                onclick="deleteTask(${item.id})" 
                                class="p-2 rounded-lg text-slate-400 hover:text-rose-600 hover:bg-rose-50 transition-colors cursor-pointer"
                                title="Hapus tugas (Fitur Pemeliharaan)"
                                aria-label="Hapus tugas"
                            >
                                <i class="fa-solid fa-trash-can text-sm"></i>
                            </button>
                        </div>
                    `;
                    listEl.appendChild(li);
                });
            }
        }

        function escapeHTML(str) {
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }

        // ---------------------------------------------------------
        // 3. LOGIKA QUIZ INTERAKTIF (BERDASARKAN MATERI DOKUMEN)
        // ---------------------------------------------------------
        const quizData = [
            {
                category: "Tahap 1: Analisis Kebutuhan",
                question: "Apa hasil utama yang diharapkan pada tahap Analisis Kebutuhan saat merancang aplikasi To-Do List?",
                options: [
                    "Kode program bahasa C yang siap dikompilasi",
                    "Dokumen kebutuhan perangkat lunak sederhana (tujuan, kebutuhan teknis, dan fungsional)",
                    "Aplikasi yang sudah dipublikasikan di GitHub Pages",
                    "Laporan bug dari hasil pengujian lintas browser"
                ],
                correctIndex: 1,
                explanation: "Pada tahap Analisis Kebutuhan, pengembang mengidentifikasi tujuan aplikasi, kebutuhan teknis (HTML, CSS, JS), serta daftar kebutuhan fungsional yang menghasilkan dokumen kebutuhan perangkat lunak sederhana."
            },
            {
                category: "Tahap 2: Desain Sistem",
                question: "Berdasarkan bagian Desain Sistem pada dokumen, teknologi apa yang dirancang untuk menyimpan data tugas di memori browser?",
                options: [
                    "MySQL Database Server",
                    "Session Cookies Eksternal",
                    "LocalStorage",
                    "File CSV Lokal"
                ],
                correctIndex: 2,
                explanation: "Pada sub-tahap Merancang Arsitektur Sistem, disebutkan bahwa data tugas disimpan di memori browser menggunakan LocalStorage agar tidak hilang saat halaman dimuat ulang."
            },
            {
                category: "Tahap 3: Pengembangan & Kode",
                question: "Dalam struktur data 'Task' pada contoh kode program To-Do List, apa arti dari nilai variabel 'isDone = 0'?",
                options: [
                    "Tugas telah dihapus dari memori",
                    "Tugas berstatus Belum Selesai",
                    "Daftar tugas sudah penuh",
                    "Tugas berstatus Selesai"
                ],
                correctIndex: 1,
                explanation: "Sesuai definisi struct Task pada dokumen: int isDone; // 0: Belum selesai, 1: Selesai."
            },
            {
                category: "Tahap 4: Pengujian Aplikasi",
                question: "Manakah yang termasuk ke dalam Pengujian Non-Fungsional berdasarkan dokumen SDLC tersebut?",
                options: [
                    "Memastikan tugas baru muncul di daftar setelah ditambahkan",
                    "Memastikan tombol Selesai mengubah status tugas",
                    "Menguji aplikasi di browser berbeda (Chrome, Firefox) dan memastikan data tetap ada setelah reload",
                    "Menulis fungsi addTask dan displayTasks"
                ],
                correctIndex: 2,
                explanation: "Pengujian Non-Fungsional berfokus pada kualitas operasional seperti kompatibilitas lintas browser (Chrome, Firefox) dan ketahanan data saat browser dimuat ulang."
            },
            {
                category: "Tahap 4: Pengujian Fungsional",
                question: "Skenario 'Klik tombol Selesai → Apakah tugas diberi tanda selesai?' termasuk dalam kategori pengujian apa?",
                options: [
                    "Pengujian Fungsional",
                    "Pengujian Keamanan Jaringan",
                    "Pengujian Non-Fungsional",
                    "Tahap Implementasi Hosting"
                ],
                correctIndex: 0,
                explanation: "Pengujian Fungsional memverifikasi bahwa setiap fitur utama (menambah tugas dan menandai tugas selesai) berjalan sesuai spesifikasi kebutuhan."
            },
            {
                category: "Tahap 5: Implementasi",
                question: "Layanan gratis apa yang dicontohkan dalam dokumen pada tahap Implementasi untuk mempublikasikan aplikasi web To-Do List?",
                options: [
                    "Google Drive",
                    "GitHub Pages",
                    "Adobe XD",
                    "GCC Compiler"
                ],
                correctIndex: 1,
                explanation: "Pada tahap Implementasi (Menyediakan Hosting), dokumen menyarankan penggunaan layanan hosting gratis seperti GitHub Pages serta menyediakan dokumentasi panduan penggunaan."
            },
            {
                category: "Tahap 6: Pemeliharaan",
                question: "Menambahkan fitur baru seperti 'menghapus tugas dari daftar' dan 'menyimpan tugas ke server menggunakan database' merupakan aktivitas pada tahap...",
                options: [
                    "Analisis Kebutuhan",
                    "Desain Sistem",
                    "Pengujian Fungsional",
                    "Pemeliharaan (Maintenance)"
                ],
                correctIndex: 3,
                explanation: "Tahap Pemeliharaan mencakup perbaikan bug berdasarkan masukan pengguna, penambahan fitur baru (seperti hapus tugas & database server), serta optimasi performa."
            },
            {
                category: "Alur Kerja Sistem",
                question: "Bagaimana urutan alur kerja aplikasi To-Do List yang dirancang pada tahap Desain Sistem?",
                options: [
                    "Pengguna menghapus tugas → Hosting aplikasi → Menambah tugas",
                    "Pengguna menambahkan tugas → Tugas ditampilkan di daftar → Pengguna menandai tugas selesai",
                    "Pengguna menguji browser → Membuat wireframe → Menandai tugas selesai",
                    "Menandai tugas selesai → Menyimpan ke LocalStorage → Membuat form input"
                ],
                correctIndex: 1,
                explanation: "Diagram alur kerja pada Desain Sistem menjelaskan siklus sederhana: Pengguna menambahkan tugas, tugas ditampilkan di daftar, lalu pengguna dapat menandai tugas yang sudah selesai."
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let isAnswered = false;

        function startQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            isAnswered = false;

            document.getElementById('quiz-start-screen').classList.add('hidden');
            document.getElementById('quiz-result-screen').classList.add('hidden');
            document.getElementById('quiz-question-screen').classList.remove('hidden');

            renderQuestion();
        }

        function renderQuestion() {
            isAnswered = false;
            const currentQ = quizData[currentQuestionIndex];
            const totalQ = quizData.length;

            // Update Progress Bar & Teks
            document.getElementById('quiz-progress-text').textContent = `Soal ${currentQuestionIndex + 1} dari ${totalQ}`;
            document.getElementById('quiz-score-live').textContent = `Benar: ${score}`;
            const progressPercent = ((currentQuestionIndex + 1) / totalQ) * 100;
            document.getElementById('quiz-progress-bar').style.width = `${progressPercent}%`;

            // Update Kategori & Teks Soal
            document.getElementById('quiz-category').textContent = currentQ.category;
            document.getElementById('quiz-question-text').textContent = currentQ.question;

            // Sembunyikan feedback & tombol lanjut
            const feedbackEl = document.getElementById('quiz-feedback');
            feedbackEl.classList.add('hidden');
            
            const nextBtn = document.getElementById('btn-next-question');
            nextBtn.classList.add('hidden');
            nextBtn.classList.remove('inline-flex');

            // Render Opsi Jawaban
            const optionsContainer = document.getElementById('quiz-options');
            optionsContainer.innerHTML = '';

            const optionLetters = ['A', 'B', 'C', 'D'];

            currentQ.options.forEach((optionText, idx) => {
                const btn = document.createElement('button');
                btn.type = 'button';
                btn.onclick = () => selectAnswer(idx);
                btn.id = `option-btn-${idx}`;
                btn.className = 'w-full text-left p-4 rounded-xl border border-slate-200 hover:border-indigo-400 hover:bg-indigo-50/40 transition-all flex items-start gap-3.5 cursor-pointer group';
                
                btn.innerHTML = `
                    <span id="option-badge-${idx}" class="w-7 h-7 rounded-lg bg-slate-100 group-hover:bg-indigo-100 text-slate-700 group-hover:text-indigo-700 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5 transition-colors">
                        ${optionLetters[idx]}
                    </span>
                    <span class="text-sm sm:text-base text-slate-800 font-medium leading-snug">${optionText}</span>
                `;
                optionsContainer.appendChild(btn);
            });
        }

        function selectAnswer(selectedIndex) {
            if (isAnswered) return;
            isAnswered = true;

            const currentQ = quizData[currentQuestionIndex];
            const isCorrect = selectedIndex === currentQ.correctIndex;

            if (isCorrect) {
                score++;
                document.getElementById('quiz-score-live').textContent = `Benar: ${score}`;
            }

            // Update tampilan semua tombol opsi
            currentQ.options.forEach((_, idx) => {
                const btn = document.getElementById(`option-btn-${idx}`);
                const badge = document.getElementById(`option-badge-${idx}`);
                
                btn.disabled = true;
                btn.classList.remove('cursor-pointer', 'hover:border-indigo-400', 'hover:bg-indigo-50/40');

                if (idx === currentQ.correctIndex) {
                    // Jawaban yang benar selalu disorot hijau
                    btn.className = 'w-full text-left p-4 rounded-xl border-2 border-emerald-500 bg-emerald-50/70 flex items-start gap-3.5';
                    badge.className = 'w-7 h-7 rounded-lg bg-emerald-600 text-white font-bold text-xs flex items-center justify-center shrink-0 mt-0.5';
                    badge.innerHTML = '<i class="fa-solid fa-check"></i>';
                } else if (idx === selectedIndex && !isCorrect) {
                    // Jawaban salah yang dipilih disorot merah
                    btn.className = 'w-full text-left p-4 rounded-xl border-2 border-rose-500 bg-rose-50/70 flex items-start gap-3.5';
                    badge.className = 'w-7 h-7 rounded-lg bg-rose-600 text-white font-bold text-xs flex items-center justify-center shrink-0 mt-0.5';
                    badge.innerHTML = '<i class="fa-solid fa-xmark"></i>';
                } else {
                    btn.className = 'w-full text-left p-4 rounded-xl border border-slate-200 bg-slate-50/50 opacity-60 flex items-start gap-3.5';
                }
            });

            // Tampilkan Kotak Pembahasan
            const feedbackEl = document.getElementById('quiz-feedback');
            const feedbackIcon = document.getElementById('feedback-icon');
            const feedbackTitle = document.getElementById('feedback-title');
            const feedbackExp = document.getElementById('feedback-explanation');

            feedbackEl.classList.remove('hidden');
            if (isCorrect) {
                feedbackEl.className = 'mt-6 p-4 rounded-xl border border-emerald-200 bg-emerald-50 text-emerald-900';
                feedbackIcon.innerHTML = '<i class="fa-solid fa-circle-check text-emerald-600"></i>';
                feedbackTitle.textContent = 'Jawaban Anda Benar!';
            } else {
                feedbackEl.className = 'mt-6 p-4 rounded-xl border border-rose-200 bg-rose-50 text-rose-900';
                feedbackIcon.innerHTML = '<i class="fa-solid fa-circle-xmark text-rose-600"></i>';
                feedbackTitle.textContent = 'Kurang Tepat!';
            }
            feedbackExp.textContent = currentQ.explanation;

            // Tampilkan Tombol Lanjut / Lihat Hasil
            const nextBtn = document.getElementById('btn-next-question');
            const nextText = document.getElementById('btn-next-text');
            nextText.textContent = currentQuestionIndex === quizData.length - 1 ? 'Lihat Skor Akhir' : 'Soal Berikutnya';
            nextBtn.classList.remove('hidden');
            nextBtn.classList.add('inline-flex');
        }

        function nextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < quizData.length) {
                renderQuestion();
            } else {
                showQuizResults();
            }
        }

        function showQuizResults() {
            document.getElementById('quiz-question-screen').classList.add('hidden');
            document.getElementById('quiz-result-screen').classList.remove('hidden');

            const totalQ = quizData.length;
            const finalPercentage = Math.round((score / totalQ) * 100);

            document.getElementById('final-score').textContent = finalPercentage;
            document.getElementById('final-correct-count').textContent = `Menjawab benar ${score} dari ${totalQ} soal`;

            const iconWrapper = document.getElementById('result-icon-wrapper');
            const icon = document.getElementById('result-icon');
            const message = document.getElementById('result-message');

            if (finalPercentage >= 75) {
                iconWrapper.className = 'w-20 h-20 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center mx-auto mb-5';
                icon.className = 'fa-solid fa-trophy text-4xl';
                message.textContent = 'Luar biasa! Anda sangat memahami konsep SDLC dan implementasi aplikasi To-Do List.';
            } else if (finalPercentage >= 50) {
                iconWrapper.className = 'w-20 h-20 rounded-full bg-amber-100 text-amber-600 flex items-center justify-center mx-auto mb-5';
                icon.className = 'fa-solid fa-award text-4xl';
                message.textContent = 'Cukup bagus! Pelajari kembali panduan 6 tahapan SDLC di tab Aplikasi To-Do untuk menyempurnakan skor Anda.';
            } else {
                iconWrapper.className = 'w-20 h-20 rounded-full bg-indigo-100 text-indigo-600 flex items-center justify-center mx-auto mb-5';
                icon.className = 'fa-solid fa-book-reader text-4xl';
                message.textContent = 'Tetap semangat belajar! Silakan baca ringkasan materi SDLC di halaman utama lalu coba ulangi quiz ini.';
            }
        }

        // Inisialisasi aplikasi saat halaman dimuat
        window.addEventListener('DOMContentLoaded', () => {
            loadTasks();
            renderTasks();
        });
    </script>
</body>
</html>
