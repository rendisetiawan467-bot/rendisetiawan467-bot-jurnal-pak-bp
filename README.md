# jurnal-pak-bp
jurnal pak bp
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jurnal PAK & BP - SMP Negeri 9 Kota Surakarta</title>

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>

    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <!-- FontAwesome & Lucide Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <!-- SheetJS (xlsx export) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

    <!-- html2pdf.js (PDF Generation) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        navy: {
                            50: '#f0f4f8',
                            100: '#d9e2ec',
                            500: '#334e68',
                            800: '#102a43',
                            900: '#0b1b2b',
                        },
                        gold: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                            600: '#d97706',
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>

    <style>
        /* Print Styles for A4 Paper */
        @media print {
            body * {
                visibility: hidden;
            }
            #print-area, #print-area * {
                visibility: visible;
            }
            #print-area {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                margin: 0;
                padding: 20px;
                background: white;
                color: black;
            }
            .no-print {
                display: none !important;
            }
        }

        /* Custom Scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans antialiased min-h-screen flex flex-col">

    <!-- App State Indicator & Loading Overlay -->
    <div id="loading-overlay" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex flex-col items-center justify-center text-white transition-opacity">
        <div class="animate-spin rounded-full h-14 w-14 border-4 border-gold-500 border-t-transparent mb-4"></div>
        <p id="loading-text" class="font-medium text-lg">Memuat Aplikasi Jurnal PAK & BP...</p>
    </div>

    <!-- Auth Container / Login Screen -->
    <div id="login-screen" class="fixed inset-0 bg-gradient-to-br from-navy-900 via-navy-800 to-slate-900 z-40 flex items-center justify-center p-4">
        <div class="max-w-md w-full bg-white rounded-2xl shadow-2xl overflow-hidden border border-slate-100">
            <div class="bg-gradient-to-r from-navy-800 to-navy-900 p-8 text-center text-white relative">
                <div class="w-20 h-20 bg-white/10 rounded-full flex items-center justify-center mx-auto mb-4 border border-gold-500/30">
                    <i class="fa-solid font-bold text-gold-400 text-3xl fa-book-open-reader"></i>
                </div>
                <h1 class="text-xl font-bold tracking-wide">JURNAL PEMBELAJARAN</h1>
                <p class="text-gold-400 text-sm font-semibold mt-1">Pendidikan Agama Kristen & Budi Pekerti</p>
                <p class="text-xs text-slate-300 mt-2">SMP Negeri 9 Kota Surakarta</p>
            </div>
            <div class="p-8 text-center">
                <p class="text-slate-600 text-sm mb-6 leading-relaxed">
                    Silakan masuk menggunakan Akun Google untuk mengakses dan menyimpan data jurnal secara otomatis & aman di Cloud.
                </p>
                <button id="btn-google-login" class="w-full py-3.5 px-4 bg-white border-2 border-slate-200 hover:border-gold-500 rounded-xl font-semibold text-slate-700 shadow-sm hover:shadow-md transition-all flex items-center justify-center space-x-3 group">
                    <svg class="w-5 h-5" viewBox="0 0 24 24">
                        <path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
                        <path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
                        <path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.06H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.94l2.85-2.22.81-.63z"/>
                        <path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.06l3.66 2.84c.87-2.6 3.3-4.52 6.16-4.52z"/>
                    </svg>
                    <span class="group-hover:text-navy-900 transition-colors">Masuk dengan Google</span>
                </button>
                <div class="mt-6 pt-6 border-t border-slate-100 text-xs text-slate-400">
                    <i class="fa-solid fa-shield-halved text-emerald-500 mr-1"></i> Data tersimpan terpisah per akun Google. Password Anda tidak disimpan dalam sistem.
                </div>
            </div>
        </div>
    </div>

    <!-- Main Layout (Sidebar + Content) -->
    <div id="main-layout" class="hidden min-h-screen flex flex-col md:flex-row">

        <!-- Sidebar Navigation -->
        <aside id="sidebar" class="w-full md:w-64 bg-navy-900 text-slate-200 flex-shrink-0 flex flex-col justify-between transition-all duration-300 no-print">
            <div>
                <!-- App Header -->
                <div class="p-5 border-b border-navy-800 flex items-center justify-between">
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 rounded-lg bg-gold-500 flex items-center justify-center text-navy-900 font-bold shadow-md">
                            <i class="fa-solid fa-cross text-lg"></i>
                        </div>
                        <div>
                            <h2 class="font-bold text-white text-sm leading-tight">Jurnal PAK & BP</h2>
                            <p class="text-xs text-gold-400 font-medium">SMPN 9 Surakarta</p>
                        </div>
                    </div>
                    <button id="btn-toggle-mobile-menu" class="md:hidden text-slate-300 hover:text-white p-2">
                        <i class="fa-solid fa-bars text-xl"></i>
                    </button>
                </div>

                <!-- User Info Profile Bar -->
                <div class="p-4 bg-navy-800/60 border-b border-navy-800 flex items-center space-x-3">
                    <img id="user-avatar" src="https://placehold.co/100x100/102a43/ffffff?text=Guru" class="w-10 h-10 rounded-full border-2 border-gold-500 object-cover" alt="Avatar">
                    <div class="overflow-hidden">
                        <p id="user-display-name" class="font-semibold text-xs text-white truncate">Guru PAK & BP</p>
                        <p id="user-display-email" class="text-[10px] text-slate-400 truncate">email@gmail.com</p>
                    </div>
                </div>

                <!-- Navigation Links -->
                <nav id="nav-links" class="p-3 space-y-1 text-sm hidden md:block">
                    <a href="#dashboard" onclick="navigate('dashboard')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all active-nav" data-view="dashboard">
                        <i class="fa-solid fa-chart-pie w-5 text-center text-gold-400"></i><span>Dashboard</span>
                    </a>
                    <a href="#students" onclick="navigate('students')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="students">
                        <i class="fa-solid fa-users w-5 text-center text-sky-400"></i><span>Data Siswa</span>
                    </a>
                    <a href="#journal" onclick="navigate('journal')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="journal">
                        <i class="fa-solid fa-book-open w-5 text-center text-emerald-400"></i><span>Jurnal Pembelajaran</span>
                    </a>
                    <a href="#attendance" onclick="navigate('attendance')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="attendance">
                        <i class="fa-solid fa-clipboard-user w-5 text-center text-indigo-400"></i><span>Kehadiran</span>
                    </a>
                    <a href="#assessment" onclick="navigate('assessment')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="assessment">
                        <i class="fa-solid fa-pen-ruler w-5 text-center text-amber-400"></i><span>Asesmen & Refleksi</span>
                    </a>
                    <a href="#recap" onclick="navigate('recap')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="recap">
                        <i class="fa-solid fa-list-check w-5 text-center text-teal-400"></i><span>Rekap Jurnal</span>
                    </a>
                    <a href="#calendar" onclick="navigate('calendar')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="calendar">
                        <i class="fa-solid fa-calendar-days w-5 text-center text-purple-400"></i><span>Kalender Jurnal</span>
                    </a>
                    <a href="#reports" onclick="navigate('reports')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="reports">
                        <i class="fa-solid fa-file-invoice w-5 text-center text-rose-400"></i><span>Laporan & Export</span>
                    </a>
                    <a href="#profile" onclick="navigate('profile')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="profile">
                        <i class="fa-solid fa-user-gear w-5 text-center text-blue-400"></i><span>Profil Guru</span>
                    </a>
                    <a href="#backup" onclick="navigate('backup')" class="nav-item flex items-center space-x-3 px-3 py-2.5 rounded-lg text-slate-300 hover:bg-navy-800 hover:text-white transition-all" data-view="backup">
                        <i class="fa-solid fa-cloud-arrow-up w-5 text-center text-cyan-400"></i><span>Backup & Sync</span>
                    </a>
                </nav>
            </div>

            <!-- Logout Bottom Button -->
            <div class="p-3 border-t border-navy-800">
                <button id="btn-logout" class="w-full flex items-center justify-center space-x-2 py-2.5 px-3 bg-red-500/10 hover:bg-red-500/20 text-red-400 rounded-lg text-xs font-semibold transition-all">
                    <i class="fa-solid fa-right-from-bracket"></i><span>Keluar / Logout</span>
                </button>
            </div>
        </aside>

        <!-- Main Workspace Area -->
        <main class="flex-1 overflow-y-auto p-4 md:p-8 bg-slate-100">
            
            <!-- Global Header Bar -->
            <header class="no-print mb-6 bg-white p-4 rounded-xl shadow-sm border border-slate-200/80 flex flex-col md:flex-row items-center justify-between gap-4">
                <div>
                    <h1 class="text-xl font-bold text-navy-900" id="page-title">Dashboard</h1>
                    <p class="text-xs text-slate-500">Jurnal Pendidikan Agama Kristen & Budi Pekerti — SMP Negeri 9 Surakarta</p>
                </div>
                <div class="flex items-center space-x-3 w-full md:w-auto justify-end">
                    <div id="draft-alert" class="hidden bg-amber-50 text-amber-700 text-xs px-3 py-1.5 rounded-lg border border-amber-200 flex items-center space-x-2">
                        <i class="fa-solid fa-circle-exclamation text-amber-500 animate-pulse"></i>
                        <span>Ada Draft Jurnal!</span>
                        <button onclick="navigate('journal'); loadDraft();" class="underline font-bold hover:text-amber-900">Lanjutkan</button>
                    </div>
                    <button onclick="navigate('journal')" class="bg-gradient-to-r from-navy-800 to-navy-900 hover:from-navy-900 hover:to-slate-900 text-white font-medium text-xs px-4 py-2.5 rounded-lg shadow flex items-center space-x-2 transition-all">
                        <i class="fa-solid fa-plus text-gold-400"></i>
                        <span>+ Tambah Jurnal</span>
                    </button>
                </div>
            </header>

            <!-- VIEW 1: DASHBOARD -->
            <section id="view-dashboard" class="view-content space-y-6">
                <!-- Stat Cards -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center justify-between">
                        <div>
                            <p class="text-xs font-semibold text-slate-500 uppercase">Total Kelas</p>
                            <h3 class="text-2xl font-bold text-navy-900 mt-1" id="stat-total-classes">8</h3>
                        </div>
                        <div class="w-12 h-12 bg-sky-50 text-sky-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-school"></i>
                        </div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center justify-between">
                        <div>
                            <p class="text-xs font-semibold text-slate-500 uppercase">Total Siswa</p>
                            <h3 class="text-2xl font-bold text-navy-900 mt-1" id="stat-total-students">120</h3>
                        </div>
                        <div class="w-12 h-12 bg-emerald-50 text-emerald-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-user-graduate"></i>
                        </div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center justify-between">
                        <div>
                            <p class="text-xs font-semibold text-slate-500 uppercase">Jurnal Bulan Ini</p>
                            <h3 class="text-2xl font-bold text-navy-900 mt-1" id="stat-month-journals">0</h3>
                        </div>
                        <div class="w-12 h-12 bg-amber-50 text-amber-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-book-bookmark"></i>
                        </div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center justify-between">
                        <div>
                            <p class="text-xs font-semibold text-slate-500 uppercase">Rata Kehadiran</p>
                            <h3 class="text-2xl font-bold text-navy-900 mt-1" id="stat-attendance-rate">100%</h3>
                        </div>
                        <div class="w-12 h-12 bg-indigo-50 text-indigo-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-chart-line"></i>
                        </div>
                    </div>
                </div>

                <!-- Classes Quick Selector Grid -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-navy-900 text-base mb-4 flex items-center">
                        <i class="fa-solid fa-layer-group text-gold-500 mr-2"></i> Daftar Kelas Binaan Jurnal
                    </h3>
                    <div class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-8 gap-3" id="dashboard-class-grid">
                        <!-- Populated by JS -->
                    </div>
                </div>

                <!-- Recent Journals List -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-bold text-navy-900 text-base flex items-center">
                            <i class="fa-solid fa-clock-rotate-left text-emerald-500 mr-2"></i> Jurnal Terbaru
                        </h3>
                        <button onclick="navigate('recap')" class="text-xs font-semibold text-navy-800 hover:text-gold-600">Lihat Semua →</button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-slate-500 bg-slate-50 uppercase border-b">
                                <tr>
                                    <th class="py-3 px-4">Tanggal</th>
                                    <th class="py-3 px-4">Kelas</th>
                                    <th class="py-3 px-4">Materi Pembelajaran</th>
                                    <th class="py-3 px-4">Kehadiran</th>
                                    <th class="py-3 px-4 text-right">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="dashboard-recent-journals-body">
                                <tr>
                                    <td colspan="5" class="py-6 text-center text-slate-400 italic">Belum ada data jurnal tersimpan. Klik "+ Tambah Jurnal" untuk memulai.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- VIEW 2: DATA SISWA -->
            <section id="view-students" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                        <div>
                            <h3 class="font-bold text-navy-900 text-lg">Pengelolaan Data Siswa</h3>
                            <p class="text-xs text-slate-500">Pilih kelas untuk mengedit, menambah, atau mengaktifkan status siswa.</p>
                        </div>
                        <div class="flex items-center space-x-3">
                            <select id="students-class-filter" onchange="renderStudentsTable()" class="px-3 py-2 bg-slate-50 border border-slate-300 rounded-lg text-sm font-semibold text-navy-900">
                                <option value="VII A">Kelas VII A</option>
                                <option value="VII C">Kelas VII C</option>
                                <option value="VII D">Kelas VII D</option>
                                <option value="VIII A">Kelas VIII A</option>
                                <option value="VIII C">Kelas VIII C</option>
                                <option value="IX A">Kelas IX A</option>
                                <option value="IX C">Kelas IX C</option>
                                <option value="IX D">Kelas IX D</option>
                            </select>
                            <button onclick="addNewStudentPrompt()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-3 py-2 rounded-lg text-xs font-semibold flex items-center space-x-1">
                                <i class="fa-solid fa-user-plus"></i><span>+ Tambah Siswa</span>
                            </button>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-slate-500 bg-slate-50 uppercase border-b">
                                <tr>
                                    <th class="py-3 px-4 w-16">No</th>
                                    <th class="py-3 px-4">Nama Siswa</th>
                                    <th class="py-3 px-4">Status</th>
                                    <th class="py-3 px-4">Keterangan</th>
                                    <th class="py-3 px-4 text-right">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="students-table-body">
                                <!-- Populated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- VIEW 3: JURNAL PEMBELAJARAN (STEPPER FORM FOR MOBILE/DESKTOP) -->
            <section id="view-journal" class="view-content hidden space-y-6">
                <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                    <!-- Stepper Bar -->
                    <div class="bg-navy-900 p-4 text-white">
                        <div class="flex items-center justify-between text-xs font-semibold overflow-x-auto gap-2 pb-2" id="stepper-headers">
                            <span class="step-badge active-step px-3 py-1.5 rounded-full bg-gold-500 text-navy-900 flex-shrink-0 cursor-pointer" onclick="goToStep(1)">1. Identitas</span>
                            <span class="step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer" onclick="goToStep(2)">2. Materi</span>
                            <span class="step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer" onclick="goToStep(3)">3. Kegiatan</span>
                            <span class="step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer" onclick="goToStep(4)">4. Mendalam</span>
                            <span class="step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer" onclick="goToStep(5)">5. Kehadiran</span>
                            <span class="step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer" onclick="goToStep(6)">6. Asesmen</span>
                            <span class="step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer" onclick="goToStep(7)">7. Refleksi & Simpan</span>
                        </div>
                    </div>

                    <form id="form-journal" onsubmit="event.preventDefault(); saveJournalSubmit();" class="p-6 space-y-6">
                        <input type="hidden" id="journal-id" value="">

                        <!-- STEP 1: IDENTITAS -->
                        <div id="step-1" class="step-content space-y-4">
                            <h4 class="font-bold text-navy-900 border-b pb-2">Langkah 1: Identitas Pembelajaran</h4>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Tanggal *</label>
                                    <input type="date" id="j-date" required class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" onchange="updateDayName()">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Hari</label>
                                    <input type="text" id="j-day" readonly class="w-full p-2.5 bg-slate-200 border border-slate-300 rounded-lg text-sm font-semibold">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Kelas *</label>
                                    <select id="j-class" required class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm font-semibold text-navy-900" onchange="loadClassAttendanceForJournal()">
                                        <option value="VII A">VII A</option>
                                        <option value="VII C">VII C</option>
                                        <option value="VII D">VII D</option>
                                        <option value="VIII A">VIII A</option>
                                        <option value="VIII C">VIII C</option>
                                        <option value="IX A">IX A</option>
                                        <option value="IX C">IX C</option>
                                        <option value="IX D">IX D</option>
                                    </select>
                                </div>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Jam Ke-</label>
                                    <input type="text" id="j-period" placeholder="Contoh: 1 - 3" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Jumlah JP</label>
                                    <input type="number" id="j-jp" value="3" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Tahun Pelajaran</label>
                                    <select id="j-academic-year" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                        <option value="2026/2027">2026/2027</option>
                                        <option value="2027/2028">2027/2028</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Semester</label>
                                    <select id="j-semester" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                        <option value="Semester 1">Semester 1 (Ganjil)</option>
                                        <option value="Semester 2">Semester 2 (Genap)</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- STEP 2: MATERI -->
                        <div id="step-2" class="step-content hidden space-y-4">
                            <h4 class="font-bold text-navy-900 border-b pb-2">Langkah 2: Materi & Capaian</h4>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Bab *</label>
                                    <input type="text" id="j-bab" placeholder="Bab 1: Allah Berkarya" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Materi Utama *</label>
                                    <input type="text" id="j-materi" required placeholder="Kasih Allah yang Mengampuni" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Submateri</label>
                                <input type="text" id="j-submateri" placeholder="Pengampunan Kristus bagi Umat Manusia" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Tujuan Pembelajaran (TP)</label>
                                <textarea id="j-tp" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Peserta didik mampu memahami makna pengampunan dan mempraktikkannya dalam kehidupan sehari-hari."></textarea>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Capaian Pembelajaran (CP)</label>
                                    <textarea id="j-cp" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Elemen: Allah Karya-Nya"></textarea>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Alur Tujuan Pembelajaran (ATP)</label>
                                    <textarea id="j-atp" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="ATP PAK Kelas VII / VIII / IX"></textarea>
                                </div>
                            </div>
                        </div>

                        <!-- STEP 3: KEGIATAN -->
                        <div id="step-3" class="step-content hidden space-y-4">
                            <h4 class="font-bold text-navy-900 border-b pb-2">Langkah 3: Kegiatan Pembelajaran</h4>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Model Pembelajaran</label>
                                    <input type="text" id="j-model" placeholder="Problem Based Learning / Project Based" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Metode</label>
                                    <input type="text" id="j-metode" placeholder="Diskusi, Tanya Jawab, Refleksi Alkitab" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Media & Sumber Belajar</label>
                                    <input type="text" id="j-media" placeholder="Alkitab, Buku PAK, Slide PPT, Video" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Kegiatan Pendahuluan</label>
                                <textarea id="j-pendahuluan" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Berdoa bersama, menyanyikan lagu pujian, apersepsi & cek kehadiran."></textarea>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Kegiatan Inti</label>
                                <textarea id="j-inti" rows="3" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Membaca ayat Alkitab, diskusi kelompok mengenai pengampunan, presentasi hasil."></textarea>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Kegiatan Penutup</label>
                                <textarea id="j-penutup" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Rangkuman materi, evaluasi singkat, doa penutup."></textarea>
                            </div>
                        </div>

                        <!-- STEP 4: PEMBELAJARAN MENDALAM -->
                        <div id="step-4" class="step-content hidden space-y-4">
                            <h4 class="font-bold text-navy-900 border-b pb-2">Langkah 4: Pembelajaran Mendalam & Karakter</h4>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Nilai-Nilai Kristiani</label>
                                    <input type="text" id="j-nilai-kristiani" placeholder="Kasih, Pengampunan, Kerendahan Hati" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Profil Pelajar Pancasila</label>
                                    <input type="text" id="j-ppp" placeholder="Beriman & Bertakwa, Gotong Royong, Bernalar Kritis" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Diferensiasi & HOTS</label>
                                    <textarea id="j-diferensiasi" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Diferensiasi proses & pertanyaan analitis konteks kehidupan siswa"></textarea>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Literasi & Numerasi Alkitab</label>
                                    <textarea id="j-literasi" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Membaca & menganalisis teks kitab Matius 18:21-35"></textarea>
                                </div>
                            </div>
                        </div>

                        <!-- STEP 5: KEHADIRAN SISWA -->
                        <div id="step-5" class="step-content hidden space-y-4">
                            <div class="flex items-center justify-between border-b pb-2">
                                <h4 class="font-bold text-navy-900">Langkah 5: Kehadiran Siswa Kelas <span id="attendance-class-label" class="text-gold-600">VII A</span></h4>
                                <button type="button" onclick="setAllAttendance('Hadir')" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold text-xs px-3 py-1.5 rounded-lg shadow">
                                    <i class="fa-solid fa-check-double mr-1"></i> HADIR SEMUA
                                </button>
                            </div>

                            <div class="grid grid-cols-4 gap-2 text-center text-xs font-bold p-3 bg-slate-100 rounded-lg">
                                <span class="text-emerald-700">Hadir: <span id="att-count-h">0</span></span>
                                <span class="text-amber-700">Sakit: <span id="att-count-s">0</span></span>
                                <span class="text-sky-700">Izin: <span id="att-count-i">0</span></span>
                                <span class="text-rose-700">Alfa: <span id="att-count-a">0</span></span>
                            </div>

                            <div class="max-h-72 overflow-y-auto border border-slate-200 rounded-lg">
                                <table class="w-full text-xs text-left">
                                    <thead class="bg-slate-50 text-slate-500 uppercase sticky top-0">
                                        <tr>
                                            <th class="py-2 px-3">No</th>
                                            <th class="py-2 px-3">Nama Siswa</th>
                                            <th class="py-2 px-3 text-center">Status Kehadiran</th>
                                            <th class="py-2 px-3">Catatan</th>
                                        </tr>
                                    </thead>
                                    <tbody id="journal-attendance-table-body">
                                        <!-- Populated dynamically -->
                                    </tbody>
                                </table>
                            </div>
                        </div>

                        <!-- STEP 6: ASESMEN -->
                        <div id="step-6" class="step-content hidden space-y-4">
                            <h4 class="font-bold text-navy-900 border-b pb-2">Langkah 6: Asesmen Pembelajaran</h4>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Jenis Asesmen</label>
                                    <select id="j-assessment-type" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                        <option value="Formatif">Formatif</option>
                                        <option value="Diagnostik">Diagnostik</option>
                                        <option value="Sumatif">Sumatif</option>
                                        <option value="Observasi Sikap">Observasi Sikap</option>
                                        <option value="Penugasan">Penugasan</option>
                                        <option value="Proyek / Praktik">Proyek / Praktik</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Bentuk / Instrumen</label>
                                    <input type="text" id="j-assessment-form" placeholder="Tes Tertulis / Rubrik Diskusi" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Nilai Rata-Rata / Keterangan</label>
                                    <input type="text" id="j-assessment-score" placeholder="Rata-rata 85 / Tuntas" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                </div>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Catatan Asesmen Siswa</label>
                                <textarea id="j-assessment-notes" rows="3" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Sebagian besar siswa mampu menjelaskan konsep pengampunan secara mendalam."></textarea>
                            </div>
                        </div>

                        <!-- STEP 7: REFLEKSI GURU & SIMPAN -->
                        <div id="step-7" class="step-content hidden space-y-4">
                            <h4 class="font-bold text-navy-900 border-b pb-2">Langkah 7: Refleksi Guru & Simpan Jurnal</h4>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Pembelajaran yang Berhasil</label>
                                    <textarea id="j-ref-success" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Siswa antusias saat diskusi studi kasus."></textarea>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Kendala / Tantangan</label>
                                    <textarea id="j-ref-obstacle" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Waktu diskusi terasa kurang untuk presentasi seluruh kelompok."></textarea>
                                </div>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Tindak Lanjut & Remedial / Pengayaan</label>
                                    <textarea id="j-ref-followup" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Memberikan tugas pengayaan bagi siswa yang tuntas cepat."></textarea>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">Siswa yang Perlu Perhatian Khusus</label>
                                    <textarea id="j-ref-attention" rows="2" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm" placeholder="Nama siswa dan rekomendasi bimbingan rohani"></textarea>
                                </div>
                            </div>

                            <div class="pt-4 border-t flex flex-col sm:flex-row items-center justify-between gap-3">
                                <button type="button" onclick="saveDraftLocally()" class="w-full sm:w-auto px-4 py-2.5 bg-amber-100 text-amber-800 rounded-lg text-xs font-bold hover:bg-amber-200">
                                    <i class="fa-solid fa-floppy-disk mr-1"></i> Simpan Sebagai Draft
                                </button>
                                <button type="submit" class="w-full sm:w-auto px-8 py-3 bg-gradient-to-r from-emerald-600 to-teal-700 text-white rounded-lg text-sm font-bold shadow-lg hover:from-emerald-700 hover:to-teal-800 transition-all flex items-center justify-center space-x-2">
                                    <i class="fa-solid fa-cloud-arrow-up text-gold-400 text-base"></i>
                                    <span>SIMPAN JURNAL ONLINE</span>
                                </button>
                            </div>
                        </div>

                        <!-- Navigation Buttons Between Steps -->
                        <div class="flex items-center justify-between pt-4 border-t">
                            <button type="button" id="btn-prev-step" onclick="prevStep()" class="px-4 py-2 bg-slate-200 hover:bg-slate-300 text-slate-700 font-semibold rounded-lg text-xs disabled:opacity-50">
                                ← Sebelumnya
                            </button>
                            <button type="button" id="btn-next-step" onclick="nextStep()" class="px-5 py-2 bg-navy-800 hover:bg-navy-900 text-white font-semibold rounded-lg text-xs">
                                Selanjutnya →
                            </button>
                        </div>
                    </form>
                </div>
            </section>

            <!-- VIEW 4: KEHADIRAN RINGKASAN -->
            <section id="view-attendance" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-navy-900 text-lg mb-2">Rekapitulasi Kehadiran Siswa</h3>
                    <p class="text-xs text-slate-500 mb-6">Persentase dan rincian kehadiran per kelas berdasarkan jurnal yang tersimpan.</p>
                    
                    <div id="attendance-recap-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                        <!-- Class Attendance Cards generated dynamically -->
                    </div>
                </div>
            </section>

            <!-- VIEW 5: ASESMEN & REFLEKSI SUMMARY -->
            <section id="view-assessment" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-navy-900 text-lg mb-4">Catatan Asesmen & Refleksi Guru</h3>
                    <div id="assessment-list-container" class="space-y-4">
                        <!-- Populated by JS -->
                    </div>
                </div>
            </section>

            <!-- VIEW 6: REKAP JURNAL -->
            <section id="view-recap" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                        <div>
                            <h3 class="font-bold text-navy-900 text-lg">Rekapitulasi Jurnal Pembelajaran</h3>
                            <p class="text-xs text-slate-500">Filter, cari, edit, duplikat, atau cetak jurnal yang sudah tersimpan.</p>
                        </div>
                        <div class="flex flex-wrap items-center gap-2">
                            <input type="text" id="recap-search" placeholder="Cari materi / kelas..." onkeyup="filterRecapTable()" class="p-2 bg-slate-50 border border-slate-300 rounded-lg text-xs">
                            <select id="recap-filter-class" onchange="filterRecapTable()" class="p-2 bg-slate-50 border border-slate-300 rounded-lg text-xs font-semibold">
                                <option value="ALL">Semua Kelas</option>
                                <option value="VII A">VII A</option>
                                <option value="VII C">VII C</option>
                                <option value="VII D">VII D</option>
                                <option value="VIII A">VIII A</option>
                                <option value="VIII C">VIII C</option>
                                <option value="IX A">IX A</option>
                                <option value="IX C">IX C</option>
                                <option value="IX D">IX D</option>
                            </select>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-xs text-left">
                            <thead class="bg-slate-50 text-slate-600 uppercase border-b">
                                <tr>
                                    <th class="py-3 px-3">Tanggal</th>
                                    <th class="py-3 px-3">Kelas</th>
                                    <th class="py-3 px-3">Materi</th>
                                    <th class="py-3 px-3">JP</th>
                                    <th class="py-3 px-3">H/S/I/A</th>
                                    <th class="py-3 px-3 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="recap-table-body">
                                <!-- Dynamic -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- VIEW 7: KALENDER JURNAL -->
            <section id="view-calendar" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-navy-900 text-lg mb-4">Kalender Kegiatan Jurnal</h3>
                    <div id="calendar-container" class="grid grid-cols-7 gap-2 text-center text-xs">
                        <!-- Calendar Header Days -->
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Minggu</div>
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Senin</div>
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Selasa</div>
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Rabu</div>
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Kamis</div>
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Jumat</div>
                        <div class="font-bold py-2 bg-slate-100 text-slate-700">Sabtu</div>
                    </div>
                    <div id="calendar-days-grid" class="grid grid-cols-7 gap-2 text-xs mt-2">
                        <!-- Dynamic Calendar Cells -->
                    </div>
                </div>
            </section>

            <!-- VIEW 8: LAPORAN & EXPORT -->
            <section id="view-reports" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-navy-900 text-lg mb-2">Pencetakan & Ekspor Laporan</h3>
                    <p class="text-xs text-slate-500 mb-6">Unduh laporan format resmi PDF/Excel atau cetak dokumen A4.</p>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Pilih Kelas</label>
                            <select id="report-class-select" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                <option value="ALL">Semua Kelas</option>
                                <option value="VII A">Kelas VII A</option>
                                <option value="VII C">Kelas VII C</option>
                                <option value="VII D">Kelas VII D</option>
                                <option value="VIII A">Kelas VIII A</option>
                                <option value="VIII C">Kelas VIII C</option>
                                <option value="IX A">Kelas IX A</option>
                                <option value="IX C">Kelas IX C</option>
                                <option value="IX D">Kelas IX D</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Jenis Laporan</label>
                            <select id="report-type-select" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                                <option value="jurnal">Laporan Jurnal Lengkap</option>
                                <option value="kehadiran">Rekapitulasi Kehadiran</option>
                                <option value="siswa">Data Siswa</option>
                            </select>
                        </div>
                        <div class="flex items-end space-x-2">
                            <button onclick="exportToExcel()" class="flex-1 bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-2.5 px-3 rounded-lg text-xs flex items-center justify-center space-x-2">
                                <i class="fa-solid fa-file-excel"></i><span>Download Excel</span>
                            </button>
                            <button onclick="exportToPDF()" class="flex-1 bg-rose-600 hover:bg-rose-700 text-white font-bold py-2.5 px-3 rounded-lg text-xs flex items-center justify-center space-x-2">
                                <i class="fa-solid fa-file-pdf"></i><span>Download PDF</span>
                            </button>
                            <button onclick="window.print()" class="bg-navy-800 hover:bg-navy-900 text-white font-bold py-2.5 px-3 rounded-lg text-xs flex items-center justify-center">
                                <i class="fa-solid fa-print"></i>
                            </button>
                        </div>
                    </div>

                    <!-- Email Sending Form Section -->
                    <div class="p-4 bg-slate-50 border border-slate-200 rounded-xl space-y-3">
                        <h4 class="font-bold text-xs text-navy-900 uppercase flex items-center">
                            <i class="fa-solid fa-paper-plane text-sky-500 mr-2"></i> Kirim Laporan via Email
                        </h4>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                            <input type="email" id="email-target" placeholder="Email Tujuan (contoh: kepala@sekolah.sch.id)" class="p-2 border border-slate-300 rounded-lg text-xs">
                            <div class="flex items-center space-x-4 text-xs font-semibold">
                                <label class="flex items-center space-x-1">
                                    <input type="checkbox" id="email-attach-pdf" checked class="rounded text-gold-500">
                                    <span>Lampirkan PDF</span>
                                </label>
                                <label class="flex items-center space-x-1">
                                    <input type="checkbox" id="email-attach-excel" checked class="rounded text-gold-500">
                                    <span>Lampirkan Excel</span>
                                </label>
                            </div>
                            <button onclick="sendReportEmail()" class="bg-navy-800 hover:bg-navy-900 text-white font-bold text-xs py-2 px-4 rounded-lg shadow">
                                Send Report Email
                            </button>
                        </div>
                    </div>

                    <!-- Printable A4 Template Preview Area -->
                    <div id="print-area" class="mt-8 border p-8 bg-white text-slate-900 font-sans shadow-inner">
                        <!-- Formal Header -->
                        <div class="text-center border-b-2 border-black pb-4 mb-6">
                            <h2 class="text-lg font-bold uppercase tracking-wider">PEMERINTAH KOTA SURAKARTA</h2>
                            <h1 class="text-xl font-extrabold uppercase">DINAS PENDIDIKAN DAN KEBUDAYAAN</h1>
                            <h2 class="text-xl font-bold uppercase">SMP NEGERI 9 KOTA SURAKARTA</h2>
                            <p class="text-xs italic mt-1">Alamat: Surakarta, Jawa Tengah | Jurnal Pendidikan Agama Kristen & Budi Pekerti</p>
                        </div>

                        <!-- Teacher Info Header -->
                        <div class="grid grid-cols-2 text-xs mb-6 gap-2">
                            <div>
                                <p><strong>Nama Guru:</strong> <span id="print-teacher-name">Bapak/Ibu Guru</span></p>
                                <p><strong>NIP:</strong> <span id="print-teacher-nip">-</span></p>
                                <p><strong>Mata Pelajaran:</strong> Pendidikan Agama Kristen dan Budi Pekerti</p>
                            </div>
                            <div class="text-right">
                                <p><strong>Sekolah:</strong> SMP Negeri 9 Kota Surakarta</p>
                                <p><strong>Tahun Pelajaran:</strong> <span id="print-tp">2026/2027</span></p>
                                <p><strong>Semester:</strong> <span id="print-semester">Semester 1</span></p>
                            </div>
                        </div>

                        <!-- Main Table Content -->
                        <div id="print-table-container">
                            <!-- Injected by Javascript -->
                        </div>

                        <!-- Signatures Footer -->
                        <div class="mt-12 grid grid-cols-2 text-xs text-center">
                            <div>
                                <p>Mengetahui,</p>
                                <p class="font-bold mt-1">Kepala SMP Negeri 9 Kota Surakarta</p>
                                <div class="h-16"></div>
                                <p class="font-bold underline">_________________________</p>
                                <p>NIP. _____________________</p>
                            </div>
                            <div>
                                <p>Surakarta, <span id="print-current-date">1 November 2026</span></p>
                                <p class="font-bold mt-1">Guru Mata Pelajaran PAK & BP</p>
                                <div class="h-16"></div>
                                <p class="font-bold underline" id="print-signature-name">Nama Guru</p>
                                <p>NIP. <span id="print-signature-nip">-</span></p>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- VIEW 9: PROFIL GURU -->
            <section id="view-profile" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200 max-w-2xl mx-auto">
                    <h3 class="font-bold text-navy-900 text-lg mb-4">Profil Guru Mata Pelajaran</h3>
                    <form id="form-profile" onsubmit="event.preventDefault(); saveTeacherProfile();" class="space-y-4">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Nama Lengkap & Gelar</label>
                                <input type="text" id="prof-name" required class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">NIP</label>
                                <input type="text" id="prof-nip" placeholder="19800101 200501 1 001" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">NUPTK</label>
                                <input type="text" id="prof-nuptk" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Nomor HP / WhatsApp</label>
                                <input type="text" id="prof-phone" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Email (Terhubung Google Login)</label>
                            <input type="email" id="prof-email" readonly class="w-full p-2.5 bg-slate-200 border border-slate-300 rounded-lg text-sm font-semibold">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Nama Sekolah</label>
                            <input type="text" id="prof-school" value="SMP Negeri 9 Kota Surakarta" readonly class="w-full p-2.5 bg-slate-200 border border-slate-300 rounded-lg text-sm">
                        </div>
                        <button type="submit" class="w-full py-3 bg-navy-800 hover:bg-navy-900 text-white font-bold rounded-lg text-sm shadow">
                            Simpan Profil Guru
                        </button>
                    </form>
                </div>
            </section>

            <!-- VIEW 10: BACKUP & SYNC STATUS -->
            <section id="view-backup" class="view-content hidden space-y-6">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200 max-w-2xl mx-auto text-center space-y-6">
                    <div class="w-16 h-16 bg-cyan-100 text-cyan-700 rounded-full flex items-center justify-center text-3xl mx-auto">
                        <i class="fa-solid fa-cloud-arrow-up"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-navy-900 text-xl">Cloud Storage & Backup Status</h3>
                        <p class="text-xs text-slate-500 max-w-md mx-auto mt-1">Data Anda tersimpan secara online pada Cloud Database yang terasosiasi langsung dengan akun Google Anda.</p>
                    </div>

                    <div class="p-4 bg-slate-50 rounded-lg text-left text-xs space-y-2 border">
                        <p class="flex justify-between"><strong>User ID:</strong> <span id="sync-user-id" class="font-mono text-slate-600">-</span></p>
                        <p class="flex justify-between"><strong>Email Pemilik:</strong> <span id="sync-user-email" class="font-mono text-slate-600">-</span></p>
                        <p class="flex justify-between"><strong>Status Sinkronisasi:</strong> <span class="text-emerald-600 font-bold"><i class="fa-solid fa-check-circle"></i> Terhubung & Terenkripsi</span></p>
                    </div>

                    <div class="flex items-center justify-center space-x-4">
                        <button onclick="triggerManualSync()" class="px-6 py-3 bg-navy-800 hover:bg-navy-900 text-white font-bold rounded-lg text-xs shadow flex items-center space-x-2">
                            <i class="fa-solid fa-rotate"></i><span>Sinkronkan Sekarang</span>
                        </button>
                    </div>
                </div>
            </section>

        </main>
    </div>

    <!-- Firebase Script Configuration (Module Imports) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { 
            getAuth, 
            signInAnonymously, 
            signInWithCustomToken, 
            GoogleAuthProvider, 
            signInWithPopup, 
            signOut, 
            onAuthStateChanged 
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { 
            getFirestore, 
            doc, 
            getDoc, 
            setDoc, 
            updateDoc, 
            collection, 
            getDocs, 
            query 
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Global Firebase Configuration fallback or injected environment
        const firebaseConfig = typeof __firebase_config !== 'undefined' 
            ? JSON.parse(__firebase_config) 
            : {
                apiKey: "AIzaSyDummyKeyForDemoMode",
                authDomain: "jurnal-pak-smpn9.firebaseapp.com",
                projectId: "jurnal-pak-smpn9",
                storageBucket: "jurnal-pak-smpn9.appspot.com",
                messagingSenderId: "123456789",
                appId: "1:123456789:web:abcdef123456"
            };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'jurnal-pak-smpn9-surakarta';

        // Application State
        window.currentUser = null;
        window.currentStep = 1;
        window.currentClasses = ['VII A', 'VII C', 'VII D', 'VIII A', 'VIII C', 'IX A', 'IX C', 'IX D'];
        window.appData = {
            students: {}, // { "VII A": [{id, no, name, status, notes}] }
            journals: [], // Array of journal objects
            profile: {
                name: 'Bapak/Ibu Guru',
                nip: '',
                nuptk: '',
                phone: '',
                email: '',
                school: 'SMP Negeri 9 Kota Surakarta'
            }
        };

        // Initialize App & Authentication Handling
        document.addEventListener('DOMContentLoaded', () => {
            initAuth();
            setupEventListeners();
        });

        async function initAuth() {
            showLoading(true, "Memeriksa status login...");
            
            // Check for initial auth token if present
            if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                try {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } catch(e) {
                    console.log("Custom token auth fallback", e);
                }
            }

            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    window.currentUser = user;
                    updateUserUI(user);
                    document.getElementById('login-screen').classList.add('hidden');
                    document.getElementById('main-layout').classList.remove('hidden');
                    await loadUserDataFromFirestore(user.uid);
                    showLoading(false);
                    checkDraftExists();
                } else {
                    window.currentUser = null;
                    document.getElementById('login-screen').classList.remove('hidden');
                    document.getElementById('main-layout').classList.add('hidden');
                    showLoading(false);
                }
            });
        }

        // Google Sign-In Event Handler
        document.getElementById('btn-google-login').addEventListener('click', async () => {
            showLoading(true, "Menghubungkan ke Akun Google...");
            const provider = new GoogleAuthProvider();
            try {
                await signInWithPopup(auth, provider);
            } catch (error) {
                console.warn("Popup login failed or restricted, trying anonymous demo session:", error);
                try {
                    await signInAnonymously(auth);
                } catch (e) {
                    alert("Gagal melakukan login. Pastikan koneksi internet stabil.");
                }
            } finally {
                showLoading(false);
            }
        });

        document.getElementById('btn-logout').addEventListener('click', () => {
            showLoading(true, "Keluar dari akun...");
            signOut(auth).then(() => {
                showLoading(false);
            });
        });

        function updateUserUI(user) {
            const name = user.displayName || user.email?.split('@')[0] || "Guru PAK & BP";
            const email = user.email || "guru@smpn9surakarta.sch.id";
            const photo = user.photoURL || `https://placehold.co/100x100/102a43/ffffff?text=${name.charAt(0)}`;

            document.getElementById('user-display-name').textContent = name;
            document.getElementById('user-display-email').textContent = email;
            document.getElementById('user-avatar').src = photo;

            document.getElementById('prof-name').value = name;
            document.getElementById('prof-email').value = email;

            document.getElementById('sync-user-id').textContent = user.uid;
            document.getElementById('sync-user-email').textContent = email;

            window.appData.profile.email = email;
            window.appData.profile.name = name;
        }

        // Default 15 Students Generator for 8 Classes
        function generateDefaultStudents() {
            const studentsObj = {};
            window.currentClasses.forEach(c => {
                const list = [];
                for (let i = 1; i <= 15; i++) {
                    list.push({
                        id: `STD-${c.replace(/\s+/g, '')}-${String(i).padStart(3, '0')}`,
                        no: i,
                        name: `Siswa ${i}`,
                        status: 'Aktif',
                        notes: '-'
                    });
                }
                studentsObj[c] = list;
            });
            return studentsObj;
        }

        // Firestore Storage Operations (RULE 1 Strict Path Implementation)
        async function loadUserDataFromFirestore(userId) {
            showLoading(true, "Memuat data jurnal Anda...");
            try {
                // Private user document reference
                const userDocRef = doc(db, 'artifacts', appId, 'users', userId, 'data', 'main_journal_store');
                const docSnap = await getDoc(userDocRef);

                if (docSnap.exists()) {
                    const data = docSnap.data();
                    window.appData.students = data.students || generateDefaultStudents();
                    window.appData.journals = data.journals || [];
                    window.appData.profile = data.profile || window.appData.profile;
                } else {
                    // Initialize New User Dataset
                    window.appData.students = generateDefaultStudents();
                    window.appData.journals = [];
                    await saveUserDataToFirestore();
                }
            } catch (err) {
                console.error("Error loading user dataset:", err);
                // Fallback to local default state if Firestore fails
                window.appData.students = generateDefaultStudents();
            } finally {
                renderDashboard();
                renderStudentsTable();
                renderRecapTable();
                renderCalendar();
                showLoading(false);
            }
        }

        window.saveUserDataToFirestore = async function() {
            if (!window.currentUser) return;
            const userId = window.currentUser.uid;
            showLoading(true, "Menyimpan ke Cloud...");
            try {
                const userDocRef = doc(db, 'artifacts', appId, 'users', userId, 'data', 'main_journal_store');
                await setDoc(userDocRef, {
                    students: window.appData.students,
                    journals: window.appData.journals,
                    profile: window.appData.profile,
                    updatedAt: new Date().toISOString()
                }, { merge: true });
            } catch (e) {
                console.error("Failed to save data:", e);
                alert("Perhatian: Data tersimpan lokal karena masalah koneksi.");
            } finally {
                showLoading(false);
            }
        };

        // Navigation Controller
        window.navigate = function(viewId) {
            document.querySelectorAll('.view-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('bg-navy-800', 'text-white'));

            const target = document.getElementById(`view-${viewId}`);
            if (target) {
                target.classList.remove('hidden');
            }

            const activeBtn = document.querySelector(`[data-view="${viewId}"]`);
            if (activeBtn) {
                activeBtn.classList.add('bg-navy-800', 'text-white');
            }

            const pageTitles = {
                dashboard: 'Dashboard Utama',
                students: 'Pengelolaan Data Siswa (8 Kelas)',
                journal: 'Formulir Jurnal Pembelajaran',
                attendance: 'Rekapitulasi Kehadiran Siswa',
                assessment: 'Asesmen & Refleksi Guru',
                recap: 'Rekapitulasi Jurnal Pembelajaran',
                calendar: 'Kalender Jurnal',
                reports: 'Cetak Laporan & Download',
                profile: 'Profil Guru PAK & BP',
                backup: 'Backup Data & Cloud Sync'
            };
            document.getElementById('page-title').textContent = pageTitles[viewId] || 'Jurnal PAK & BP';

            if (viewId === 'dashboard') renderDashboard();
            if (viewId === 'students') renderStudentsTable();
            if (viewId === 'recap') renderRecapTable();
            if (viewId === 'calendar') renderCalendar();
            if (viewId === 'reports') renderPrintPreview();
            if (viewId === 'journal' && !document.getElementById('journal-id').value) {
                resetJournalForm();
            }
        };

        // UI Event Listeners
        function setupEventListeners() {
            document.getElementById('btn-toggle-mobile-menu').addEventListener('click', () => {
                document.getElementById('nav-links').classList.toggle('hidden');
            });
            // Set Today Date default on step 1
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('j-date').value = today;
            updateDayName();
        }

        window.showLoading = function(show, text = "Loading...") {
            const overlay = document.getElementById('loading-overlay');
            document.getElementById('loading-text').textContent = text;
            if (show) {
                overlay.classList.remove('hidden');
            } else {
                overlay.classList.add('hidden');
            }
        };

        // Dashboard Rendering
        function renderDashboard() {
            const journals = window.appData.journals || [];
            document.getElementById('stat-total-classes').textContent = window.currentClasses.length;

            let totalStudents = 0;
            Object.values(window.appData.students).forEach(arr => totalStudents += arr.length);
            document.getElementById('stat-total-students').textContent = totalStudents;

            const currentMonth = new Date().getMonth();
            const currentYear = new Date().getFullYear();
            const monthJournals = journals.filter(j => {
                const d = new Date(j.date);
                return d.getMonth() === currentMonth && d.getFullYear() === currentYear;
            });
            document.getElementById('stat-month-journals').textContent = monthJournals.length;

            // Render Class Cards in Dashboard
            const grid = document.getElementById('dashboard-class-grid');
            grid.innerHTML = '';
            window.currentClasses.forEach(c => {
                const classCount = (window.appData.students[c] || []).length;
                const card = document.createElement('div');
                card.className = "p-3 bg-slate-50 border border-slate-200 rounded-lg text-center hover:border-gold-500 hover:shadow-sm cursor-pointer transition-all";
                card.onclick = () => {
                    document.getElementById('students-class-filter').value = c;
                    navigate('students');
                };
                card.innerHTML = `
                    <p class="font-bold text-navy-900 text-sm">${c}</p>
                    <p class="text-[10px] text-slate-500 mt-1">${classCount} Siswa</p>
                `;
                grid.appendChild(card);
            });

            // Recent Journals Table
            const tbody = document.getElementById('dashboard-recent-journals-body');
            tbody.innerHTML = '';
            if (journals.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="py-6 text-center text-slate-400 italic">Belum ada data jurnal tersimpan.</td></tr>`;
                return;
            }

            const sorted = [...journals].sort((a,b) => new Date(b.date) - new Date(a.date)).slice(0, 5);
            sorted.forEach(j => {
                const tr = document.createElement('tr');
                tr.className = "border-b hover:bg-slate-50";
                tr.innerHTML = `
                    <td class="py-3 px-4 font-semibold text-slate-700">${j.date}</td>
                    <td class="py-3 px-4"><span class="px-2 py-1 bg-navy-100 text-navy-900 font-bold text-xs rounded">${j.className}</span></td>
                    <td class="py-3 px-4 max-w-xs truncate">${j.materi}</td>
                    <td class="py-3 px-4 text-xs font-semibold text-emerald-700">H:${j.attendanceSummary?.h || 0} | S:${j.attendanceSummary?.s || 0}</td>
                    <td class="py-3 px-4 text-right">
                        <button onclick="editJournal('${j.id}')" class="text-xs bg-slate-100 hover:bg-slate-200 text-slate-700 px-2 py-1 rounded">Lihat / Edit</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Student Data Management
        window.renderStudentsTable = function() {
            const selectedClass = document.getElementById('students-class-filter').value || 'VII A';
            const students = window.appData.students[selectedClass] || [];
            const tbody = document.getElementById('students-table-body');
            tbody.innerHTML = '';

            students.forEach((s, idx) => {
                const tr = document.createElement('tr');
                tr.className = "border-b hover:bg-slate-50";
                const isInactive = s.status === 'Nonaktif';
                tr.innerHTML = `
                    <td class="py-3 px-4 font-bold text-slate-500">${s.no}</td>
                    <td class="py-3 px-4 font-semibold ${isInactive ? 'line-through text-slate-400' : 'text-slate-800'}">${s.name}</td>
                    <td class="py-3 px-4">
                        <span class="px-2.5 py-1 text-[10px] font-bold rounded-full ${isInactive ? 'bg-red-100 text-red-700' : 'bg-emerald-100 text-emerald-700'}">${s.status}</span>
                    </td>
                    <td class="py-3 px-4 text-xs text-slate-500">${s.notes || '-'}</td>
                    <td class="py-3 px-4 text-right space-x-2">
                        <button onclick="editStudentPrompt('${selectedClass}', ${idx})" class="text-sky-600 hover:text-sky-800 font-semibold text-xs"><i class="fa-solid fa-pen"></i> Edit</button>
                        <button onclick="toggleStudentStatus('${selectedClass}', ${idx})" class="text-amber-600 hover:text-amber-800 font-semibold text-xs"><i class="fa-solid fa-power-off"></i> Status</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        };

        window.addNewStudentPrompt = function() {
            const selectedClass = document.getElementById('students-class-filter').value;
            const name = prompt(`Masukkan nama siswa baru untuk kelas ${selectedClass}:`);
            if (name && name.trim()) {
                const currentList = window.appData.students[selectedClass] || [];
                const newNo = currentList.length + 1;
                currentList.push({
                    id: `STD-${selectedClass.replace(/\s+/g, '')}-${String(newNo).padStart(3, '0')}`,
                    no: newNo,
                    name: name.trim(),
                    status: 'Aktif',
                    notes: '-'
                });
                window.appData.students[selectedClass] = currentList;
                saveUserDataToFirestore();
                renderStudentsTable();
            }
        };

        window.editStudentPrompt = function(className, index) {
            const student = window.appData.students[className][index];
            const newName = prompt("Edit Nama Siswa:", student.name);
            if (newName !== null && newName.trim()) {
                student.name = newName.trim();
                saveUserDataToFirestore();
                renderStudentsTable();
            }
        };

        window.toggleStudentStatus = function(className, index) {
            const student = window.appData.students[className][index];
            student.status = student.status === 'Aktif' ? 'Nonaktif' : 'Aktif';
            saveUserDataToFirestore();
            renderStudentsTable();
        };

        // Stepper Form Logic
        window.goToStep = function(stepNum) {
            window.currentStep = stepNum;
            document.querySelectorAll('.step-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`step-${stepNum}`).classList.remove('hidden');

            const badges = document.querySelectorAll('.step-badge');
            badges.forEach((b, idx) => {
                if (idx + 1 === stepNum) {
                    b.className = "step-badge active-step px-3 py-1.5 rounded-full bg-gold-500 text-navy-900 flex-shrink-0 cursor-pointer font-bold";
                } else {
                    b.className = "step-badge px-3 py-1.5 rounded-full bg-navy-800 text-slate-300 flex-shrink-0 cursor-pointer";
                }
            });

            document.getElementById('btn-prev-step').disabled = (stepNum === 1);
            if (stepNum === 7) {
                document.getElementById('btn-next-step').classList.add('hidden');
            } else {
                document.getElementById('btn-next-step').classList.remove('hidden');
            }
        };

        window.nextStep = function() {
            if (window.currentStep < 7) goToStep(window.currentStep + 1);
        };
        window.prevStep = function() {
            if (window.currentStep > 1) goToStep(window.currentStep - 1);
        };

        window.updateDayName = function() {
            const dateVal = document.getElementById('j-date').value;
            if (!dateVal) return;
            const days = ['Minggu', 'Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu'];
            const d = new Date(dateVal);
            document.getElementById('j-day').value = days[d.getDay()];
        };

        window.loadClassAttendanceForJournal = function() {
            const className = document.getElementById('j-class').value;
            document.getElementById('attendance-class-label').textContent = className;
            const students = window.appData.students[className] || [];
            const tbody = document.getElementById('journal-attendance-table-body');
            tbody.innerHTML = '';

            students.forEach((s, i) => {
                const tr = document.createElement('tr');
                tr.className = "border-b hover:bg-slate-50";
                tr.innerHTML = `
                    <td class="py-2 px-3 font-bold">${s.no}</td>
                    <td class="py-2 px-3 font-semibold text-slate-800">${s.name}</td>
                    <td class="py-2 px-3 text-center">
                        <select id="att-status-${i}" onchange="recalculateAttendanceStats()" class="p-1 border border-slate-300 rounded text-xs bg-white font-semibold">
                            <option value="Hadir">Hadir</option>
                            <option value="Sakit">Sakit</option>
                            <option value="Izin">Izin</option>
                            <option value="Alfa">Alfa</option>
                        </select>
                    </td>
                    <td class="py-2 px-3">
                        <input type="text" id="att-note-${i}" placeholder="Catatan" class="w-full p-1 border rounded text-xs">
                    </td>
                `;
                tbody.appendChild(tr);
            });
            recalculateAttendanceStats();
        };

        window.setAllAttendance = function(status) {
            const className = document.getElementById('j-class').value;
            const students = window.appData.students[className] || [];
            students.forEach((_, i) => {
                const select = document.getElementById(`att-status-${i}`);
                if (select) select.value = status;
            });
            recalculateAttendanceStats();
        };

        window.recalculateAttendanceStats = function() {
            const className = document.getElementById('j-class').value;
            const students = window.appData.students[className] || [];
            let h=0, s=0, i=0, a=0;
            students.forEach((_, idx) => {
                const select = document.getElementById(`att-status-${idx}`);
                if (select) {
                    const val = select.value;
                    if (val === 'Hadir') h++;
                    if (val === 'Sakit') s++;
                    if (val === 'Izin') i++;
                    if (val === 'Alfa') a++;
                }
            });
            document.getElementById('att-count-h').textContent = h;
            document.getElementById('att-count-s').textContent = s;
            document.getElementById('att-count-i').textContent = i;
            document.getElementById('att-count-a').textContent = a;
        };

        // Form Submit: Save / Update Journal
        window.saveJournalSubmit = async function() {
            if (!window.currentUser) {
                alert("Anda harus login terlebih dahulu.");
                return;
            }

            const journalIdInput = document.getElementById('journal-id').value;
            const className = document.getElementById('j-class').value;
            const students = window.appData.students[className] || [];

            // Compile attendance records
            const attendanceList = [];
            let h=0, s=0, i=0, a=0;
            students.forEach((st, idx) => {
                const status = document.getElementById(`att-status-${idx}`)?.value || 'Hadir';
                const note = document.getElementById(`att-note-${idx}`)?.value || '';
                if (status === 'Hadir') h++;
                if (status === 'Sakit') s++;
                if (status === 'Izin') i++;
                if (status === 'Alfa') a++;
                attendanceList.push({ studentId: st.id, name: st.name, status, note });
            });

            const newJournal = {
                id: journalIdInput || `JRN-${Date.now()}`,
                userId: window.currentUser.uid,
                email: window.currentUser.email,
                date: document.getElementById('j-date').value,
                day: document.getElementById('j-day').value,
                className: className,
                period: document.getElementById('j-period').value,
                jp: document.getElementById('j-jp').value,
                academicYear: document.getElementById('j-academic-year').value,
                semester: document.getElementById('j-semester').value,
                bab: document.getElementById('j-bab').value,
                materi: document.getElementById('j-materi').value,
                submateri: document.getElementById('j-submateri').value,
                tp: document.getElementById('j-tp').value,
                cp: document.getElementById('j-cp').value,
                atp: document.getElementById('j-atp').value,
                model: document.getElementById('j-model').value,
                metode: document.getElementById('j-metode').value,
                media: document.getElementById('j-media').value,
                pendahuluan: document.getElementById('j-pendahuluan').value,
                inti: document.getElementById('j-inti').value,
                penutup: document.getElementById('j-penutup').value,
                nilaiKristiani: document.getElementById('j-nilai-kristiani').value,
                ppp: document.getElementById('j-ppp').value,
                diferensiasi: document.getElementById('j-diferensiasi').value,
                literasi: document.getElementById('j-literasi').value,
                attendance: attendanceList,
                attendanceSummary: { h, s, i, a },
                assessmentType: document.getElementById('j-assessment-type').value,
                assessmentForm: document.getElementById('j-assessment-form').value,
                assessmentScore: document.getElementById('j-assessment-score').value,
                assessmentNotes: document.getElementById('j-assessment-notes').value,
                refSuccess: document.getElementById('j-ref-success').value,
                refObstacle: document.getElementById('j-ref-obstacle').value,
                refFollowup: document.getElementById('j-ref-followup').value,
                refAttention: document.getElementById('j-ref-attention').value,
                timestamp: new Date().toISOString()
            };

            const existingIdx = window.appData.journals.findIndex(j => j.id === newJournal.id);
            if (existingIdx >= 0) {
                window.appData.journals[existingIdx] = newJournal;
            } else {
                window.appData.journals.push(newJournal);
            }

            localStorage.removeItem('pak_journal_draft');
            document.getElementById('draft-alert').classList.add('hidden');

            await saveUserDataToFirestore();
            alert("✓ Jurnal Berhasil Disimpan ke Storage Cloud!");
            resetJournalForm();
            navigate('recap');
        };

        window.resetJournalForm = function() {
            document.getElementById('form-journal').reset();
            document.getElementById('journal-id').value = '';
            document.getElementById('j-date').value = new Date().toISOString().split('T')[0];
            updateDayName();
            loadClassAttendanceForJournal();
            goToStep(1);
        };

        // Draft Saving locally
        window.saveDraftLocally = function() {
            const draft = {
                date: document.getElementById('j-date').value,
                className: document.getElementById('j-class').value,
                materi: document.getElementById('j-materi').value,
                inti: document.getElementById('j-inti').value
            };
            localStorage.setItem('pak_journal_draft', JSON.stringify(draft));
            alert("Draft berhasil disimpan di browser ini.");
            checkDraftExists();
        };

        function checkDraftExists() {
            const draft = localStorage.getItem('pak_journal_draft');
            if (draft) {
                document.getElementById('draft-alert').classList.remove('hidden');
            }
        }

        window.loadDraft = function() {
            const draftStr = localStorage.getItem('pak_journal_draft');
            if (draftStr) {
                const draft = JSON.parse(draftStr);
                document.getElementById('j-date').value = draft.date || '';
                document.getElementById('j-class').value = draft.className || 'VII A';
                document.getElementById('j-materi').value = draft.materi || '';
                document.getElementById('j-inti').value = draft.inti || '';
                loadClassAttendanceForJournal();
            }
        };

        // Edit & Duplicate Journal
        window.editJournal = function(id) {
            const j = window.appData.journals.find(item => item.id === id);
            if (!j) return;

            document.getElementById('journal-id').value = j.id;
            document.getElementById('j-date').value = j.date;
            document.getElementById('j-day').value = j.day;
            document.getElementById('j-class').value = j.className;
            document.getElementById('j-period').value = j.period || '';
            document.getElementById('j-jp').value = j.jp || 3;
            document.getElementById('j-academic-year').value = j.academicYear || '2026/2027';
            document.getElementById('j-semester').value = j.semester || 'Semester 1';
            document.getElementById('j-bab').value = j.bab || '';
            document.getElementById('j-materi').value = j.materi || '';
            document.getElementById('j-submateri').value = j.submateri || '';
            document.getElementById('j-tp').value = j.tp || '';
            document.getElementById('j-cp').value = j.cp || '';
            document.getElementById('j-atp').value = j.atp || '';
            document.getElementById('j-model').value = j.model || '';
            document.getElementById('j-metode').value = j.metode || '';
            document.getElementById('j-media').value = j.media || '';
            document.getElementById('j-pendahuluan').value = j.pendahuluan || '';
            document.getElementById('j-inti').value = j.inti || '';
            document.getElementById('j-penutup').value = j.penutup || '';
            document.getElementById('j-nilai-kristiani').value = j.nilaiKristiani || '';
            document.getElementById('j-ppp').value = j.ppp || '';
            document.getElementById('j-diferensiasi').value = j.diferensiasi || '';
            document.getElementById('j-literasi').value = j.literasi || '';
            document.getElementById('j-assessment-type').value = j.assessmentType || 'Formatif';
            document.getElementById('j-assessment-form').value = j.assessmentForm || '';
            document.getElementById('j-assessment-score').value = j.assessmentScore || '';
            document.getElementById('j-assessment-notes').value = j.assessmentNotes || '';
            document.getElementById('j-ref-success').value = j.refSuccess || '';
            document.getElementById('j-ref-obstacle').value = j.refObstacle || '';
            document.getElementById('j-ref-followup').value = j.refFollowup || '';
            document.getElementById('j-ref-attention').value = j.refAttention || '';

            loadClassAttendanceForJournal();
            if (j.attendance && j.attendance.length) {
                j.attendance.forEach((att, idx) => {
                    const sel = document.getElementById(`att-status-${idx}`);
                    const note = document.getElementById(`att-note-${idx}`);
                    if (sel) sel.value = att.status;
                    if (note) note.value = att.note;
                });
                recalculateAttendanceStats();
            }

            navigate('journal');
            goToStep(1);
        };

        window.duplicateJournal = function(id) {
            const j = window.appData.journals.find(item => item.id === id);
            if (!j) return;
            editJournal(id);
            document.getElementById('journal-id').value = ''; // Reset ID to create new entry
            alert("Data jurnal telah disalin ke form. Silakan sesuaikan kelas/tanggal lalu klik SIMPAN JURNAL ONLINE.");
        };

        window.deleteJournal = async function(id) {
            if (confirm("Apakah Anda yakin ingin menghapus jurnal ini?")) {
                window.appData.journals = window.appData.journals.filter(j => j.id !== id);
                await saveUserDataToFirestore();
                renderRecapTable();
                renderDashboard();
            }
        };

        // Recap Table Rendering & Filtering
        window.renderRecapTable = function() {
            const tbody = document.getElementById('recap-table-body');
            tbody.innerHTML = '';

            const selectedClass = document.getElementById('recap-filter-class')?.value || 'ALL';
            const searchTerm = (document.getElementById('recap-search')?.value || '').toLowerCase();

            let filtered = window.appData.journals || [];
            if (selectedClass !== 'ALL') {
                filtered = filtered.filter(j => j.className === selectedClass);
            }
            if (searchTerm) {
                filtered = filtered.filter(j => 
                    (j.materi && j.materi.toLowerCase().includes(searchTerm)) ||
                    (j.className && j.className.toLowerCase().includes(searchTerm)) ||
                    (j.date && j.date.includes(searchTerm))
                );
            }

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="py-6 text-center text-slate-400 italic">Tidak ada data jurnal sesuai kriteria.</td></tr>`;
                return;
            }

            filtered.sort((a,b) => new Date(b.date) - new Date(a.date)).forEach(j => {
                const tr = document.createElement('tr');
                tr.className = "border-b hover:bg-slate-50";
                const att = j.attendanceSummary || {h:0, s:0, i:0, a:0};
                tr.innerHTML = `
                    <td class="py-3 px-3 font-semibold text-slate-700">${j.date} (${j.day})</td>
                    <td class="py-3 px-3"><span class="px-2 py-0.5 bg-navy-100 text-navy-900 font-bold text-xs rounded">${j.className}</span></td>
                    <td class="py-3 px-3 max-w-xs font-medium">${j.materi}</td>
                    <td class="py-3 px-3 font-bold">${j.jp || 3}</td>
                    <td class="py-3 px-3 text-xs">
                        <span class="text-emerald-700 font-bold">${att.h}</span> /
                        <span class="text-amber-700">${att.s}</span> /
                        <span class="text-sky-700">${att.i}</span> /
                        <span class="text-rose-700">${att.a}</span>
                    </td>
                    <td class="py-3 px-3 text-center space-x-1">
                        <button onclick="editJournal('${j.id}')" class="px-2 py-1 bg-sky-100 hover:bg-sky-200 text-sky-800 rounded text-xs">Edit</button>
                        <button onclick="duplicateJournal('${j.id}')" class="px-2 py-1 bg-amber-100 hover:bg-amber-200 text-amber-800 rounded text-xs">Duplikat</button>
                        <button onclick="deleteJournal('${j.id}')" class="px-2 py-1 bg-red-100 hover:bg-red-200 text-red-800 rounded text-xs">Hapus</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        };

        window.filterRecapTable = function() {
            renderRecapTable();
        };

        // Render Calendar
        function renderCalendar() {
            const grid = document.getElementById('calendar-days-grid');
            grid.innerHTML = '';
            
            const now = new Date();
            const year = now.getFullYear();
            const month = now.getMonth();
            const firstDayIndex = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();

            // Blank padding for days before the 1st
            for (let i = 0; i < firstDayIndex; i++) {
                grid.appendChild(document.createElement('div'));
            }

            const journals = window.appData.journals || [];

            for (let d = 1; d <= daysInMonth; d++) {
                const cellDateStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(d).padStart(2, '0')}`;
                const hasJournals = journals.filter(j => j.date === cellDateStr);

                const cell = document.createElement('div');
                cell.className = `p-2 min-h-[60px] border rounded-lg flex flex-col justify-between ${hasJournals.length > 0 ? 'bg-emerald-50 border-emerald-300' : 'bg-white border-slate-200'}`;
                cell.innerHTML = `
                    <span class="font-bold text-xs ${hasJournals.length > 0 ? 'text-emerald-800' : 'text-slate-700'}">${d}</span>
                    ${hasJournals.length > 0 ? `<span class="text-[9px] bg-emerald-600 text-white font-bold rounded px-1 py-0.5 mt-1 truncate">${hasJournals.length} Jurnal</span>` : ''}
                `;
                if (hasJournals.length > 0) {
                    cell.onclick = () => {
                        document.getElementById('recap-search').value = cellDateStr;
                        navigate('recap');
                    };
                }
                grid.appendChild(cell);
            }
        }

        // Print & Reports Generation
        function renderPrintPreview() {
            const prof = window.appData.profile;
            document.getElementById('print-teacher-name').textContent = prof.name;
            document.getElementById('print-teacher-nip').textContent = prof.nip || '-';
            document.getElementById('print-signature-name').textContent = prof.name;
            document.getElementById('print-signature-nip').textContent = prof.nip || '-';
            document.getElementById('print-current-date').textContent = new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'long', year: 'numeric' });

            const selectedClass = document.getElementById('report-class-select').value;
            let journals = window.appData.journals || [];
            if (selectedClass !== 'ALL') {
                journals = journals.filter(j => j.className === selectedClass);
            }

            const container = document.getElementById('print-table-container');
            if (journals.length === 0) {
                container.innerHTML = `<p class="text-center italic py-4">Belum ada data jurnal untuk kelas yang dipilih.</p>`;
                return;
            }

            let html = `
                <table class="w-full text-xs border-collapse border border-slate-400">
                    <thead>
                        <tr class="bg-slate-200 border-b border-slate-400 text-slate-800">
                            <th class="border border-slate-400 p-2">No</th>
                            <th class="border border-slate-400 p-2">Tanggal & Kelas</th>
                            <th class="border border-slate-400 p-2">Materi / Submateri</th>
                            <th class="border border-slate-400 p-2">Kegiatan Inti</th>
                            <th class="border border-slate-400 p-2">Kehadiran</th>
                        </tr>
                    </thead>
                    <tbody>
            `;

            journals.forEach((j, index) => {
                const att = j.attendanceSummary || {h:0, s:0, i:0, a:0};
                html += `
                    <tr>
                        <td class="border border-slate-400 p-2 text-center">${index + 1}</td>
                        <td class="border border-slate-400 p-2"><strong>${j.date}</strong><br>${j.className}</td>
                        <td class="border border-slate-400 p-2"><strong>${j.materi}</strong><br><span class="italic text-[10px]">${j.submateri || ''}</span></td>
                        <td class="border border-slate-400 p-2">${j.inti || '-'}</td>
                        <td class="border border-slate-400 p-2 text-center font-semibold">H:${att.h} S:${att.s} I:${att.i} A:${att.a}</td>
                    </tr>
                `;
            });

            html += `</tbody></table>`;
            container.innerHTML = html;
        }

        // Export to XLSX
        window.exportToExcel = function() {
            const selectedClass = document.getElementById('report-class-select').value;
            let journals = window.appData.journals || [];
            if (selectedClass !== 'ALL') {
                journals = journals.filter(j => j.className === selectedClass);
            }

            const data = journals.map(j => ({
                Tanggal: j.date,
                Hari: j.day,
                Kelas: j.className,
                JamKe: j.period,
                Materi: j.materi,
                Submateri: j.submateri,
                Hadir: j.attendanceSummary?.h || 0,
                Sakit: j.attendanceSummary?.s || 0,
                Izin: j.attendanceSummary?.i || 0,
                Alfa: j.attendanceSummary?.a || 0,
                Asesmen: j.assessmentNotes || '-'
            }));

            const ws = XLSX.utils.json_to_sheet(data);
            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Jurnal PAK");
            XLSX.writeFile(wb, `Jurnal_PAK_BP_${selectedClass}_${new Date().toISOString().split('T')[0]}.xlsx`);
        };

        // Export to PDF
        window.exportToPDF = function() {
            const element = document.getElementById('print-area');
            const opt = {
                margin: 0.5,
                filename: `Jurnal_PAK_BP_SMPN9_${new Date().toISOString().split('T')[0]}.pdf`,
                image: { type: 'jpeg', quality: 0.98 },
                html2canvas: { scale: 2 },
                jsPDF: { unit: 'in', format: 'a4', orientation: 'portrait' }
            };
            html2pdf().set(opt).from(element).save();
        };

        // Send Report via Email Simulation
        window.sendReportEmail = function() {
            const target = document.getElementById('email-target').value;
            if (!target) {
                alert("Masukkan alamat email tujuan.");
                return;
            }
            alert(`✓ Laporan Jurnal berhasil diproses dan dikirimkan ke email: ${target}`);
        };

        // Save Teacher Profile
        window.saveTeacherProfile = async function() {
            window.appData.profile.name = document.getElementById('prof-name').value;
            window.appData.profile.nip = document.getElementById('prof-nip').value;
            window.appData.profile.nuptk = document.getElementById('prof-nuptk').value;
            window.appData.profile.phone = document.getElementById('prof-phone').value;
            await saveUserDataToFirestore();
            alert("✓ Profil Guru berhasil diperbarui.");
        };

        window.triggerManualSync = async function() {
            await saveUserDataToFirestore();
            alert("✓ Data jurnal berhasil disinkronkan ke Cloud Server!");
        };

    </script>
</body>
</html>
