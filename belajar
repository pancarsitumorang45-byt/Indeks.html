<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi To-Do List - Implementasi SDLC</title>
    <!-- Menggunakan Tailwind CSS untuk styling modern dan responsif -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Menggunakan Font Inter dari Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Menggunakan Font Awesome untuk ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        /* Animasi halus untuk item daftar tugas */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .task-item {
            animation: fadeIn 0.25s ease-out forwards;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 8px;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 8px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between text-slate-800 bg-gradient-to-br from-indigo-50 via-slate-50 to-blue-50">

    <!-- Notifikasi Toast (Pengganti alert()) -->
    <div id="toast" class="fixed top-5 right-5 z-50 transform transition-all duration-300 translate-x-full opacity-0">
        <div id="toast-content" class="flex items-center gap-3 px-4 py-3 rounded-xl shadow-lg text-white font-medium text-sm">
            <i id="toast-icon" class="fas fa-info-circle"></i>
            <span id="toast-message">Pesan notifikasi</span>
        </div>
    </div>

    <!-- Header / Navbar -->
    <header class="bg-white/80 backdrop-blur-md border-b border-slate-200 sticky top-0 z-30">
        <div class="max-w-3xl mx-auto px-4 py-4 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-indigo-600 text-white flex items-center justify-center shadow-md shadow-indigo-200">
                    <i class="fas fa-check-double text-lg"></i>
                </div>
                <div>
                    <h1 class="text-lg sm:text-xl font-bold text-slate-900 leading-tight">To-Do List App</h1>
                    <p class="text-xs text-slate-500">Implementasi Siklus Hidup Pengembangan Sistem (SDLC)</p>
                </div>
            </div>
            <!-- Tombol Panduan Penggunaan -->
            <button onclick="toggleGuideModal()" class="flex items-center gap-2 px-3.5 py-2 text-sm font-medium text-indigo-600 bg-indigo-50 hover:bg-indigo-100 rounded-xl transition-colors">
                <i class="fas fa-book-open"></i>
                <span class="hidden sm:inline">Panduan</span>
            </button>
        </div>
    </header>

    <!-- Konten Utama -->
    <main class="flex-grow max-w-3xl w-full mx-auto px-4 py-8">
        
        <!-- Kartu Statistik Ringkas -->
        <div class="grid grid-cols-3 gap-3 sm:gap-4 mb-6">
            <div class="bg-white p-4 rounded-2xl border border-slate-200/75 shadow-sm flex flex-col items-center justify-center text-center">
                <span class="text-xs font-medium text-slate-500 uppercase tracking-wider">Total Tugas</span>
                <span id="stat-total" class="text-2xl font-bold text-slate-800 mt-1">0</span>
            </div>
            <div class="bg-white p-4 rounded-2xl border border-slate-200/75 shadow-sm flex flex-col items-center justify-center text-center">
                <span class="text-xs font-medium text-amber-600 uppercase tracking-wider">Belum Selesai</span>
                <span id="stat-pending" class="text-2xl font-bold text-amber-600 mt-1">0</span>
            </div>
            <div class="bg-white p-4 rounded-2xl border border-slate-200/75 shadow-sm flex flex-col items-center justify-center text-center">
                <span class="text-xs font-medium text-emerald-600 uppercase tracking-wider">Selesai</span>
                <span id="stat-completed" class="text-2xl font-bold text-emerald-600 mt-1">0</span>
            </div>
        </div>

        <!-- Form Tambah Tugas (Kebutuhan Fungsional & Desain UI) -->
        <section class="bg-white p-5 sm:p-6 rounded-2xl border border-slate-200/75 shadow-sm mb-6">
            <form id="todo-form" class="flex flex-col sm:flex-row gap-3">
                <div class="relative flex-grow">
                    <div class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                        <i class="fas fa-tasks"></i>
                    </div>
                    <input 
                        type="text" 
                        id="task-input" 
                        placeholder="Masukkan nama tugas baru di sini..." 
                        maxlength="100"
                        class="w-full pl-10 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:bg-white transition-all"
                        required
                    >
                </div>
                <button 
                    type="submit" 
                    class="px-6 py-3 bg-indigo-600 hover:bg-indigo-700 active:scale-[0.98] text-white font-semibold rounded-xl shadow-md shadow-indigo-200 transition-all flex items-center justify-center gap-2 whitespace-nowrap"
                >
                    <i class="fas fa-plus"></i>
                    <span>Tambah Tugas</span>
                </button>
            </form>
        </section>

        <!-- Daftar Tugas & Kontrol Filter / Pemeliharaan -->
        <section class="bg-white rounded-2xl border border-slate-200/75 shadow-sm overflow-hidden">
            <!-- Header Daftar Tugas -->
            <div class="p-4 sm:p-5 border-b border-slate-100 flex flex-wrap items-center justify-between gap-3 bg-slate-50/50">
                <div class="flex items-center gap-2">
                    <h2 class="font-bold text-slate-800 text-base sm:text-lg">Daftar Tugas</h2>
                    <span class="text-xs px-2.5 py-0.5 rounded-full bg-indigo-100 text-indigo-700 font-medium" id="storage-badge" title="Data disimpan otomatis di browser">
                        <i class="fas fa-database mr-1"></i>LocalStorage
                    </span>
                </div>

                <!-- Filter Status -->
                <div class="flex items-center gap-1 bg-slate-200/70 p-1 rounded-xl text-xs font-medium">
                    <button onclick="setFilter('all')" id="filter-all" class="filter-btn px-3 py-1.5 rounded-lg bg-white text-slate-800 shadow-sm transition-all">Semua</button>
                    <button onclick="setFilter('pending')" id="filter-pending" class="filter-btn px-3 py-1.5 rounded-lg text-slate-600 hover:text-slate-800 transition-all">Belum Selesai</button>
                    <button onclick="setFilter('completed')" id="filter-completed" class="filter-btn px-3 py-1.5 rounded-lg text-slate-600 hover:text-slate-800 transition-all">Selesai</button>
                </div>
            </div>

            <!-- Kontainer List Item -->
            <div id="task-list-container" class="divide-y divide-slate-100 max-h-[450px] overflow-y-auto">
                <!-- Item tugas akan dirender di sini oleh JavaScript -->
            </div>

            <!-- State Kosong -->
            <div id="empty-state" class="hidden py-12 px-4 text-center">
                <div class="w-16 h-16 bg-slate-100 text-slate-400 rounded-full flex items-center justify-center mx-auto mb-3 text-2xl">
                    <i class="fas fa-clipboard-list"></i>
                </div>
                <h3 class="text-slate-700 font-semibold text-base">Tidak ada tugas</h3>
                <p class="text-slate-400 text-sm mt-1 max-w-xs mx-auto">Tambahkan tugas baru menggunakan formulir di atas untuk mulai mencatat aktivitas Anda.</p>
            </div>

            <!-- Footer Aksi Pemeliharaan (Hapus Tugas Selesai / Kosongkan Daftar) -->
            <div id="maintenance-actions" class="hidden p-4 bg-slate-50 border-t border-slate-100 flex flex-wrap items-center justify-between gap-2 text-xs">
                <button 
                    onclick="clearCompletedTasks()" 
                    id="btn-clear-completed"
                    class="px-3 py-2 text-amber-700 bg-amber-50 hover:bg-amber-100 border border-amber-200/60 rounded-xl font-medium transition-colors flex items-center gap-1.5"
                >
                    <i class="fas fa-broom"></i>
                    <span>Hapus Tugas Selesai</span>
                </button>
                <button 
                    onclick="openConfirmClearAll()" 
                    class="px-3 py-2 text-rose-600 bg-rose-50 hover:bg-rose-100 border border-rose-200/60 rounded-xl font-medium transition-colors flex items-center gap-1.5"
                >
                    <i class="fas fa-trash-alt"></i>
                    <span>Kosongkan Semua Daftar</span>
                </button>
            </div>
        </section>

        <!-- Bagian Panduan Penggunaan Singkat (Tahap Implementasi) -->
        <section class="mt-8 bg-white/70 border border-slate-200/80 rounded-2xl p-5 sm:p-6">
            <div class="flex items-center gap-2.5 mb-3">
                <i class="fas fa-info-circle text-indigo-600 text-lg"></i>
                <h3 class="font-bold text-slate-800 text-base">Panduan Penggunaan Aplikasi</h3>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-sm text-slate-600">
                <div class="flex items-start gap-3">
                    <span class="w-6 h-6 rounded-full bg-indigo-100 text-indigo-700 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5">1</span>
                    <p><strong class="text-slate-800">Tambah Tugas:</strong> Ketik nama tugas pada kolom input di atas, lalu klik tombol <em>"Tambah Tugas"</em> atau tekan Enter.</p>
                </div>
                <div class="flex items-start gap-3">
                    <span class="w-6 h-6 rounded-full bg-emerald-100 text-emerald-700 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5">2</span>
                    <p><strong class="text-slate-800">Tandai Selesai:</strong> Klik tombol <em>"Selesai"</em> pada tugas yang telah dikerjakan untuk mengubah status dan mencoret teks tugas.</p>
                </div>
                <div class="flex items-start gap-3">
                    <span class="w-6 h-6 rounded-full bg-rose-100 text-rose-700 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5">3</span>
                    <p><strong class="text-slate-800">Hapus Tugas:</strong> Gunakan tombol ikon tempat sampah untuk menghapus satu tugas, atau gunakan tombol pembersih di bagian bawah daftar.</p>
                </div>
                <div class="flex items-start gap-3">
                    <span class="w-6 h-6 rounded-full bg-amber-100 text-amber-700 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5">4</span>
                    <p><strong class="text-slate-800">Penyimpanan Otomatis:</strong> Data tersimpan di <em>LocalStorage</em> browser. Tugas tidak akan hilang meskipun halaman dimuat ulang (refresh).</p>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="py-5 text-center text-xs text-slate-400 border-t border-slate-200/60 bg-white/40 mt-8">
        <p>Aplikasi To-Do List Sederhana &bull; Dibangun berdasarkan tahapan SDLC (Analisis, Desain, Pengembangan, Pengujian, Implementasi, Pemeliharaan)</p>
    </footer>

    <!-- Modal Panduan Detail (Akses cepat dari Header) -->
    <div id="guide-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/50 backdrop-blur-sm p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl border border-slate-100 transform transition-all">
            <div class="flex items-center justify-between border-b border-slate-100 pb-3 mb-4">
                <h3 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                    <i class="fas fa-book-open text-indigo-600"></i>
                    Dokumentasi & Panduan SDLC
                </h3>
                <button onclick="toggleGuideModal()" class="text-slate-400 hover:text-slate-600 p-1 rounded-lg">
                    <i class="fas fa-times text-lg"></i>
                </button>
            </div>
            <div class="space-y-3 text-sm text-slate-600 max-h-[60vh] overflow-y-auto pr-1">
                <p>Aplikasi ini merupakan implementasi web dari dokumen <strong>Pembuatan Aplikasi Sederhana Berdasarkan SDLC</strong>:</p>
                <ul class="list-disc pl-5 space-y-1.5">
                    <li><strong>Analisis Kebutuhan:</strong> Mencatat daftar tugas dan menandai tugas yang sudah selesai.</li>
                    <li><strong>Arsitektur Sistem:</strong> Menggunakan <code>localStorage</code> browser untuk menyimpan array objek tugas (<code>{ id, task, isDone, createdAt }</code>) secara persisten.</li>
                    <li><strong>Pengujian:</strong> Mendukung pengujian fungsional (tambah, selesai, hapus) dan non-fungsional (data tetap aman setelah browser di-refresh).</li>
                    <li><strong>Pemeliharaan:</strong> Dilengkapi fitur penghapusan satuan, hapus semua yang selesai, dan reset daftar.</li>
                </ul>
            </div>
            <div class="mt-6 flex justify-end">
                <button onclick="toggleGuideModal()" class="px-5 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white font-medium rounded-xl text-sm transition-colors">
                    Mengerti
                </button>
            </div>
        </div>
    </div>

    <!-- Modal Konfirmasi Kosongkan Semua (Pengganti confirm()) -->
    <div id="confirm-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/50 backdrop-blur-sm p-4">
        <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-2xl border border-slate-100 text-center">
            <div class="w-14 h-14 rounded-full bg-rose-100 text-rose-600 flex items-center justify-center mx-auto mb-4 text-2xl">
                <i class="fas fa-exclamation-triangle"></i>
            </div>
            <h3 class="text-lg font-bold text-slate-800">Kosongkan Semua Daftar?</h3>
            <p class="text-sm text-slate-500 mt-2">Semua tugas yang ada di daftar (baik yang selesai maupun belum) akan dihapus permanen dari memori browser.</p>
            <div class="mt-6 flex gap-3 justify-center">
                <button onclick="closeConfirmModal()" class="flex-1 px-4 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium rounded-xl text-sm transition-colors">
                    Batal
                </button>
                <button onclick="confirmClearAll()" class="flex-1 px-4 py-2.5 bg-rose-600 hover:bg-rose-700 text-white font-medium rounded-xl text-sm transition-colors">
                    Ya, Hapus Semua
                </button>
            </div>
        </div>
    </div>

    <!-- Logika JavaScript Aplikasi -->
    <script>
        // Kunci penyimpanan pada LocalStorage browser
        const STORAGE_KEY = 'sdlc_todo_tasks_v1';

        // State aplikasi
        let tasks = [];
        let currentFilter = 'all'; // 'all' | 'pending' | 'completed'

        // Referensi elemen DOM
        const todoForm = document.getElementById('todo-form');
        const taskInput = document.getElementById('task-input');
        const taskListContainer = document.getElementById('task-list-container');
        const emptyState = document.getElementById('empty-state');
        const maintenanceActions = document.getElementById('maintenance-actions');
        const btnClearCompleted = document.getElementById('btn-clear-completed');

        // Elemen Statistik
        const statTotal = document.getElementById('stat-total');
        const statPending = document.getElementById('stat-pending');
        const statCompleted = document.getElementById('stat-completed');

        /**
         * Inisialisasi aplikasi saat halaman dimuat
         */
        document.addEventListener('DOMContentLoaded', () => {
            loadTasksFromStorage();
            renderTasks();
        });

        /**
         * Memuat data tugas dari LocalStorage
         */
        function loadTasksFromStorage() {
            try {
                const storedData = localStorage.getItem(STORAGE_KEY);
                if (storedData) {
                    tasks = JSON.parse(storedData);
                } else {
                    // Contoh tugas awal jika baru pertama kali dibuka
                    tasks = [
                        { id: Date.now() - 2000, task: 'Mempelajari tahapan SDLC', isDone: true },
                        { id: Date.now() - 1000, task: 'Menguji fitur tambah dan selesai tugas', isDone: false }
                    ];
                    saveTasksToStorage();
                }
            } catch (error) {
                console.error('Gagal memuat data dari LocalStorage:', error);
                tasks = [];
            }
        }

        /**
         * Menyimpan data tugas ke LocalStorage
         */
        function saveTasksToStorage() {
            try {
                localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
            } catch (error) {
                console.error('Gagal menyimpan data ke LocalStorage:', error);
                showToast('Gagal menyimpan ke memori browser!', 'error');
            }
        }

        /**
         * Event Listener untuk menambah tugas baru (addTask)
         */
        todoForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const taskText = taskInput.value.trim();

            if (!taskText) {
                showToast('Nama tugas tidak boleh kosong!', 'warning');
                return;
            }

            // Struktur data sesuai konsep struct pada dokumen (task, isDone)
            const newTask = {
                id: Date.now(),
                task: taskText,
                isDone: false // false: Belum Selesai, true: Selesai
            };

            tasks.unshift(newTask); // Tambahkan di urutan paling atas
            saveTasksToStorage();
            taskInput.value = '';
            renderTasks();
            showToast('Tugas berhasil ditambahkan!', 'success');
        });

        /**
         * Menandai tugas selesai atau mengembalikan ke belum selesai (markTaskAsDone)
         */
        function toggleTaskStatus(id) {
            const taskIndex = tasks.findIndex(t => t.id === id);
            if (taskIndex !== -1) {
                tasks[taskIndex].isDone = !tasks[taskIndex].isDone;
                saveTasksToStorage();
                renderTasks();
                
                if (tasks[taskIndex].isDone) {
                    showToast('Tugas ditandai sebagai selesai!', 'success');
                } else {
                    showToast('Status tugas dikembalikan ke belum selesai.', 'info');
                }
            }
        }

        /**
         * Menghapus satu tugas berdasarkan ID (Tahap Pemeliharaan)
         */
        function deleteTask(id) {
            tasks = tasks.filter(t => t.id !== id);
            saveTasksToStorage();
            renderTasks();
            showToast('Tugas telah dihapus.', 'info');
        }

        /**
         * Menghapus semua tugas yang sudah selesai (Tahap Pemeliharaan)
         */
        function clearCompletedTasks() {
            const completedCount = tasks.filter(t => t.isDone).length;
            if (completedCount === 0) {
                showToast('Tidak ada tugas selesai untuk dihapus.', 'warning');
                return;
            }
            tasks = tasks.filter(t => !t.isDone);
            saveTasksToStorage();
            renderTasks();
            showToast(`${completedCount} tugas selesai berhasil dibersihkan.`, 'success');
        }

        /**
         * Mengosongkan seluruh daftar tugas
         */
        function confirmClearAll() {
            tasks = [];
            saveTasksToStorage();
            renderTasks();
            closeConfirmModal();
            showToast('Seluruh daftar tugas telah dikosongkan.', 'info');
        }

        /**
         * Mengatur filter tampilan daftar tugas
         */
        function setFilter(filterType) {
            currentFilter = filterType;
            
            // Update tampilan tombol filter aktif
            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.classList.remove('bg-white', 'text-slate-800', 'shadow-sm');
                btn.classList.add('text-slate-600');
            });
            const activeBtn = document.getElementById(`filter-${filterType}`);
            activeBtn.classList.remove('text-slate-600');
            activeBtn.classList.add('bg-white', 'text-slate-800', 'shadow-sm');

            renderTasks();
        }

        /**
         * Menampilkan daftar tugas ke antarmuka (displayTasks)
         */
        function renderTasks() {
            // Update statistik
            const total = tasks.length;
            const completed = tasks.filter(t => t.isDone).length;
            const pending = total - completed;

            statTotal.textContent = total;
            statPending.textContent = pending;
            statCompleted.textContent = completed;

            // Tampilkan/sembunyikan panel aksi pemeliharaan
            if (total > 0) {
                maintenanceActions.classList.remove('hidden');
                btnClearCompleted.style.display = completed > 0 ? 'flex' : 'none';
            } else {
                maintenanceActions.classList.add('hidden');
            }

            // Filter tugas sesuai tab yang dipilih
            let filteredTasks = tasks;
            if (currentFilter === 'pending') {
                filteredTasks = tasks.filter(t => !t.isDone);
            } else if (currentFilter === 'completed') {
                filteredTasks = tasks.filter(t => t.isDone);
            }

            // Kosongkan kontainer
            taskListContainer.innerHTML = '';

            // Cek jika daftar kosong
            if (filteredTasks.length === 0) {
                emptyState.classList.remove('hidden');
                return;
            } else {
                emptyState.classList.add('hidden');
            }

            // Render setiap item tugas
            filteredTasks.forEach((item, index) => {
                const taskElement = document.createElement('div');
                taskElement.className = `task-item p-4 sm:px-5 flex items-center justify-between gap-3 transition-colors ${
                    item.isDone ? 'bg-slate-50/80' : 'bg-white hover:bg-slate-50/60'
                }`;

                // Escape HTML untuk mencegah XSS
                const safeTaskText = escapeHTML(item.task);

                taskElement.innerHTML = `
                    <div class="flex items-center gap-3.5 min-w-0 flex-grow">
                        <span class="text-xs font-semibold text-slate-400 w-5 text-right shrink-0">${index + 1}.</span>
                        <div class="min-w-0 flex-grow">
                            <p class="text-sm sm:text-base font-medium break-words transition-all ${
                                item.isDone ? 'line-through text-slate-400' : 'text-slate-800'
                            }">
                                ${safeTaskText}
                            </p>
                            <span class="inline-flex items-center gap-1 mt-1 text-[11px] font-medium px-2 py-0.5 rounded-md ${
                                item.isDone 
                                    ? 'bg-emerald-100/80 text-emerald-700' 
                                    : 'bg-amber-100/80 text-amber-700'
                            }">
                                <i class="fas ${item.isDone ? 'fa-check-circle' : 'fa-clock'}"></i>
                                ${item.isDone ? 'Selesai' : 'Belum Selesai'}
                            </span>
                        </div>
                    </div>

                    <div class="flex items-center gap-2 shrink-0">
                        <!-- Tombol Selesai / Batal Selesai -->
                        <button 
                            onclick="toggleTaskStatus(${item.id})" 
                            class="px-3 py-2 rounded-xl text-xs sm:text-sm font-medium transition-all flex items-center gap-1.5 ${
                                item.isDone 
                                    ? 'bg-slate-200/80 hover:bg-slate-300/80 text-slate-700' 
                                    : 'bg-emerald-600 hover:bg-emerald-700 text-white shadow-sm shadow-emerald-200'
                            }"
                            title="${item.isDone ? 'Tandai Belum Selesai' : 'Tandai Tugas Selesai'}"
                        >
                            <i class="fas ${item.isDone ? 'fa-undo' : 'fa-check'}"></i>
                            <span class="hidden sm:inline">${item.isDone ? 'Batal' : 'Selesai'}</span>
                        </button>

                        <!-- Tombol Hapus -->
                        <button 
                            onclick="deleteTask(${item.id})" 
                            class="p-2 sm:px-3 sm:py-2 rounded-xl text-rose-600 bg-rose-50 hover:bg-rose-100 transition-colors"
                            title="Hapus Tugas"
                        >
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </div>
                `;

                taskListContainer.appendChild(taskElement);
            });
        }

        /**
         * Helper untuk mencegah injeksi HTML
         */
        function escapeHTML(str) {
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }

        /**
         * Menampilkan notifikasi Toast
         */
        let toastTimeout;
        function showToast(message, type = 'info') {
            const toast = document.getElementById('toast');
            const toastContent = document.getElementById('toast-content');
            const toastIcon = document.getElementById('toast-icon');
            const toastMessage = document.getElementById('toast-message');

            // Atur warna & ikon berdasarkan tipe
            toastContent.className = 'flex items-center gap-3 px-4 py-3 rounded-xl shadow-lg text-white font-medium text-sm ';
            if (type === 'success') {
                toastContent.classList.add('bg-emerald-600');
                toastIcon.className = 'fas fa-check-circle';
            } else if (type === 'error') {
                toastContent.classList.add('bg-rose-600');
                toastIcon.className = 'fas fa-exclamation-circle';
            } else if (type === 'warning') {
                toastContent.classList.add('bg-amber-600');
                toastIcon.className = 'fas fa-exclamation-triangle';
            } else {
                toastContent.classList.add('bg-slate-800');
                toastIcon.className = 'fas fa-info-circle';
            }

            toastMessage.textContent = message;

            // Tampilkan toast
            toast.classList.remove('translate-x-full', 'opacity-0');
            
            clearTimeout(toastTimeout);
            toastTimeout = setTimeout(() => {
                toast.classList.add('translate-x-full', 'opacity-0');
            }, 3000);
        }

        /**
         * Kontrol Modal Panduan & Modal Konfirmasi
         */
        function toggleGuideModal() {
            const modal = document.getElementById('guide-modal');
            if (modal.classList.contains('hidden')) {
                modal.classList.remove('hidden');
                modal.classList.add('flex');
            } else {
                modal.classList.add('hidden');
                modal.classList.remove('flex');
            }
        }

        function openConfirmClearAll() {
            const modal = document.getElementById('confirm-modal');
            modal.classList.remove('hidden');
            modal.classList.add('flex');
        }

        function closeConfirmModal() {
            const modal = document.getElementById('confirm-modal');
            modal.classList.add('hidden');
            modal.classList.remove('flex');
        }
    </script>
</body>
</html>

dari HTML diatas Buatkan Saya WEBnya

Aplikasi To-Do List SDLC

16 minutes ago

WEB aplikasi To-Do List berbasis SDLC sudah dibuat dan diterapkan sebagai halaman interaktif.

Fitur yang tersedia:

Menambahkan tugas baru
Menandai tugas sebagai selesai atau belum selesai
Menghapus tugas satu per satu
Filter tugas: Semua, Belum Selesai, dan Selesai
Statistik jumlah total, pending, dan tugas selesai
Menghapus seluruh tugas yang sudah selesai
Mengosongkan semua daftar dengan modal konfirmasi
Notifikasi toast untuk setiap tindakan
Modal dokumentasi dan panduan SDLC
Penyimpanan otomatis menggunakan LocalStorage
State kosong ketika tidak ada tugas
Tampilan responsif untuk desktop dan perangkat seluler
Animasi daftar tugas
Perlindungan terhadap injeksi HTML pada teks tugas
Canva AI responded with 1 message
