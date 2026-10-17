<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bảng Tuần Hoàn Hóa Học Tương Tác</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- QRCode JS -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <style>
        .grid-periodic {
            display: grid;
            grid-template-columns: repeat(18, minmax(0, 1fr));
            gap: 4px;
        }
        .element-card {
            aspect-ratio: 1;
            transition: all 0.2s ease;
        }
        .element-card:hover {
            transform: scale(1.15);
            z-index: 20;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen p-4 md:p-8">

    <!-- Header -->
    <header class="max-w-7xl mx-auto mb-6 text-center">
        <h1 class="text-3xl md:text-5xl font-black tracking-tight text-amber-400 flex items-center justify-center gap-3">
            <i data-lucide="rocket" class="w-8 h-8 text-amber-400"></i>
            LEVEL UP: BẢNG TUẦN HOÀN TƯƠNG TÁC
        </h1>
        <p class="text-slate-400 mt-2">Khám phá 118 nguyên tố hóa học, cấu trúc nguyên tử, ứng dụng và câu hỏi tương tác</p>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto">
        <!-- Controls & Filters -->
        <div class="flex flex-col md:flex-row gap-4 mb-6 justify-between items-center bg-slate-800/80 p-4 rounded-xl border border-slate-700">
            <div class="relative w-full md:w-96">
                <i data-lucide="search" class="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 w-5 h-5"></i>
                <input type="text" id="searchInput" placeholder="Tìm theo tên, ký hiệu, số hiệu..." class="w-full bg-slate-900 border border-slate-700 rounded-lg pl-10 pr-4 py-2 text-sm focus:outline-none focus:border-amber-400">
            </div>
            <div class="flex flex-wrap gap-2 text-xs">
                <span class="px-3 py-1.5 rounded-full bg-emerald-500/20 text-emerald-300 border border-emerald-500/40">Phi kim</span>
                <span class="px-3 py-1.5 rounded-full bg-sky-500/20 text-sky-300 border border-sky-500/40">Khí hiếm</span>
                <span class="px-3 py-1.5 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/40">Kim loại kiềm</span>
                <span class="px-3 py-1.5 rounded-full bg-purple-500/20 text-purple-300 border border-purple-500/40">Kim loại chuyển tiếp</span>
            </div>
        </div>

        <!-- Periodic Table Grid -->
        <div class="overflow-x-auto pb-6">
            <div id="periodicTable" class="grid-periodic min-w-[1000px]"></div>
        </div>
    </main>

    <!-- Element Detail Modal -->
    <div id="elementModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-800 border border-slate-700 w-full max-w-4xl rounded-2xl max-h-[90vh] overflow-y-auto relative shadow-2xl">
            <!-- Close Button -->
            <button onclick="closeModal()" class="absolute top-4 right-4 p-2 bg-slate-700 hover:bg-slate-600 rounded-full transition z-10">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>

            <!-- Modal Header / Basic Info -->
            <div class="p-6 border-b border-slate-700 bg-slate-800/50 flex flex-col md:flex-row items-center gap-6">
                <!-- Element Badge / Card -->
                <div id="modalBadge" class="w-36 h-44 rounded-xl border-2 p-3 flex flex-col justify-between items-center text-center shadow-lg relative bg-slate-900">
                    <span id="modalAtomicNum" class="text-sm font-bold self-start">8</span>
                    <span id="modalSymbol" class="text-4xl font-extrabold my-1">O</span>
                    <div>
                        <div id="modalName" class="text-sm font-semibold">Oxygen</div>
                        <div id="modalMass" class="text-xs text-slate-400">15.999</div>
                    </div>
                </div>

                <!-- Info Grid -->
                <div class="flex-1 space-y-3 w-full">
                    <div class="grid grid-cols-2 md:grid-cols-3 gap-3">
                        <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700/50">
                            <span class="text-xs text-slate-400 block">Nguyên tử khối</span>
                            <span id="detailMass" class="font-bold text-amber-400">15.999</span>
                        </div>
                        <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700/50">
                            <span class="text-xs text-slate-400 block">Độ âm điện</span>
                            <span id="detailElectro" class="font-bold text-amber-400">3.44</span>
                        </div>
                        <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700/50">
                            <span class="text-xs text-slate-400 block">Cấu hình electron</span>
                            <span id="detailConfig" class="font-bold text-amber-400">[He] 2s² 2p⁴</span>
                        </div>
                        <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700/50 col-span-2 md:col-span-3">
                            <span class="text-xs text-slate-400 block">Số oxi hóa thường gặp</span>
                            <span id="detailOxidation" class="font-bold text-amber-400">-2, +2</span>
                        </div>
                    </div>
                    <!-- Detailed Explanation -->
                    <p id="detailExplanation" class="text-xs text-slate-300 bg-slate-900/40 p-3 rounded-lg border border-slate-700/30 leading-relaxed"></p>
                </div>
            </div>

            <!-- Tab Navigation -->
            <div class="flex border-b border-slate-700 bg-slate-900/50 overflow-x-auto text-sm">
                <button onclick="switchTab('atom')" id="tab-atom" class="tab-btn px-5 py-3 font-medium border-b-2 border-amber-400 text-amber-400 flex items-center gap-2">
                    <i data-lucide="atom" class="w-4 h-4"></i> Mô hình nguyên tử
                </button>
                <button onclick="switchTab('apps')" id="tab-apps" class="tab-btn px-5 py-3 font-medium border-b-2 border-transparent text-slate-400 hover:text-slate-200 flex items-center gap-2">
                    <i data-lucide="image" class="w-4 h-4"></i> Hình ảnh ứng dụng
                </button>
                <button onclick="switchTab('video')" id="tab-video" class="tab-btn px-5 py-3 font-medium border-b-2 border-transparent text-slate-400 hover:text-slate-200 flex items-center gap-2">
                    <i data-lucide="film" class="w-4 h-4"></i> Thí nghiệm & Video
                </button>
                <button onclick="switchTab('quiz')" id="tab-quiz" class="tab-btn px-5 py-3 font-medium border-b-2 border-transparent text-slate-400 hover:text-slate-200 flex items-center gap-2">
                    <i data-lucide="help-circle" class="w-4 h-4"></i> Câu hỏi tương tác
                </button>
                <button onclick="switchTab('qr')" id="tab-qr" class="tab-btn px-5 py-3 font-medium border-b-2 border-transparent text-slate-400 hover:text-slate-200 flex items-center gap-2">
                    <i data-lucide="qr-code" class="w-4 h-4"></i> Mã QR
                </button>
            </div>

            <!-- Tab Contents -->
            <div class="p-6">
                <!-- 1. Atom Model Tab -->
                <div id="content-atom" class="tab-content flex flex-col md:flex-row items-center gap-6">
                    <div class="relative w-64 h-64 flex items-center justify-center bg-slate-900/80 rounded-xl border border-slate-700">
                        <canvas id="atomCanvas" width="250" height="250"></canvas>
                    </div>
                    <div class="flex-1 space-y-3 text-sm">
                        <div class="flex items-center gap-2 text-amber-400 font-semibold">
                            <div class="w-3 h-3 rounded-full bg-amber-500"></div>
                            <span>Hạt nhân: <span id="atomProtons">8</span> Proton, <span id="atomNeutrons">8</span> Neutron</span>
                        </div>
                        <div class="flex items-center gap-2 text-sky-400 font-semibold">
                            <div class="w-3 h-3 rounded-full bg-sky-400"></div>
                            <span>Electron vỏ ngoài cùng: <span id="outerElectrons">6</span> e</span>
                        </div>
                        <div class="flex items-center gap-2 text-emerald-400 font-semibold">
                            <div class="w-3 h-3 rounded-full bg-emerald-400"></div>
                            <span>Số lớp electron: <span id="electronShells">2</span> lớp</span>
                        </div>
                        <p id="atomDescription" class="text-xs text-slate-400 pt-2 border-t border-slate-700/50"></p>
                    </div>
                </div>

                <!-- 2. Applications Tab -->
                <div id="content-apps" class="tab-content hidden space-y-4">
                    <div id="appsList" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
                    <div class="bg-amber-500/10 border border-amber-500/30 p-4 rounded-xl text-sm">
                        <span class="font-bold text-amber-400 block mb-1">Vì sao?</span>
                        <p id="appsWhy" class="text-slate-300 text-xs"></p>
                    </div>
                </div>

                <!-- 3. Video / Experiment Tab -->
                <div id="content-video" class="tab-content hidden space-y-4">
                    <div class="aspect-video w-full rounded-xl overflow-hidden bg-slate-900 border border-slate-700">
                        <iframe id="videoIframe" class="w-full h-full" src="" title="Video thí nghiệm" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                    </div>
                    <p id="videoDesc" class="text-xs text-slate-400"></p>
                </div>

                <!-- 4. Quiz Tab -->
                <div id="content-quiz" class="tab-content hidden space-y-4">
                    <div class="bg-slate-900/60 p-5 rounded-xl border border-slate-700">
                        <h4 id="quizQuestion" class="font-semibold text-amber-400 text-base mb-4"></h4>
                        <div id="quizOptions" class="space-y-2"></div>
                        <div id="quizResult" class="mt-4 p-3 rounded-lg text-xs font-semibold hidden"></div>
                    </div>
                </div>

                <!-- 5. QR Code Tab -->
                <div id="content-qr" class="tab-content hidden flex flex-col items-center justify-center text-center space-y-3">
                    <div id="qrcode" class="p-4 bg-white rounded-xl border"></div>
                    <p class="text-xs text-slate-400">Quét mã QR để mở nhanh thông tin nguyên tố này trên điện thoại</p>
                </div>
            </div>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        // Database 118 nguyên tố cơ bản & mẫu dữ liệu phong phú
        const elementsData = [
            { number: 1, symbol: "H", name: "Hidro", mass: "1.008", electro: 2.20, config: "1s¹", oxidation: "-1, +1", group: "nonmetal", period: 1, col: 1, protons: 1, shells: [1], desc: "Ô 1 → hạt nhân có 1 proton, vỏ có 1 electron. Chu kì 1 → có 1 lớp electron. H là nguyên tố nhẹ nhất vũ trụ.", apps: [{ title: "Nước H₂O", desc: "Thành phần tạo nên nước sinh hoạt và sự sống." }, { title: "Nhiên liệu tên lửa", desc: "Sử dụng làm nhiên liệu sạch cho pin nhiên liệu và động cơ tên lửa." }], why: "Đốt H₂ chỉ tạo ra nước và tỏa rất nhiều nhiệt; H₂ là khí nhẹ nhất.", video: "https://www.youtube.com/embed/H46S0IvhXis", quiz: { q: "Hidro có bao nhiêu electron ở lớp ngoài cùng?", options: ["1", "2", "3", "4"], correct: 0 } },
            { number: 2, symbol: "He", name: "Heli", mass: "4.003", electro: 0, config: "1s²", oxidation: "0", group: "noble", period: 1, col: 18, protons: 2, shells: [2], desc: "Khí hiếm đơn nguyên tử, không màu, không mùi, cực kỳ ít hoạt động hóa học.", apps: [{ title: "Bóng bay", desc: "Bơm khí khí cầu, bóng bay giải trí." }], why: "Khí Heli nhẹ hơn không khí và hoàn toàn không gây cháy nổ.", video: "https://www.youtube.com/embed/93I_2gJ49yY", quiz: { q: "Khí Heli thuộc nhóm nào?", options: ["Kim loại kiềm", "Khí hiếm", "Halogen", "Phi kim"], correct: 1 } },
            { number: 3, symbol: "Li", name: "Liti", mass: "6.94", electro: 0.98, config: "[He] 2s¹", oxidation: "+1", group: "alkali", period: 2, col: 1, protons: 3, shells: [2, 1], desc: "Kim loại kiềm nhẹ nhất, mềm, màu trắng bạc.", apps: [{ title: "Pin Lithium-ion", desc: "Dùng trong điện thoại, laptop, xe điện." }], why: "Khả năng tích trữ năng lượng điện hóa cực cao.", video: "", quiz: { q: "Ứng dụng phổ biến nhất của Liti là gì?", options: ["Bóng đèn", "Pin sạc", "Sản xuất thép", "Thuốc lá"], correct: 1 } },
            { number: 8, symbol: "O", name: "Oxygen", mass: "15.999", electro: 3.44, config: "[He] 2s² 2p⁴", oxidation: "-2", group: "nonmetal", period: 2, col: 16, protons: 8, shells: [2, 6], desc: "Chiếm khoảng 21% thể tích khí quyển. Cần thiết cho sự hô hấp của sinh vật và duy trì sự cháy.", apps: [{ title: "Hô hấp sinh học", desc: "Duy trì sự sống cho con người và động vật." }, { title: "Hàn cắt kim loại", desc: "Dùng khí Oxy-Axetylen để tạo nhiệt độ cao." }], why: "Oxygen có tính oxi hóa mạnh, giúp duy trì phản ứng cháy giải phóng năng lượng.", video: "https://www.youtube.com/embed/1P3pLUp4dUI", quiz: { q: "Oxygen chiếm bao nhiêu % thể tích không khí?", options: ["78%", "21%", "1%", "50%"], correct: 1 } },
            { number: 13, symbol: "Al", name: "Nhôm", mass: "26.98", electro: 1.61, config: "[Ne] 3s² 3p¹", oxidation: "+3", group: "metal", period: 3, col: 13, protons: 13, shells: [2, 8, 3], desc: "Kim loại phổ biến nhất trong vỏ Trái Đất, nhẹ, dẫn điện và nhiệt tốt.", apps: [{ title: "Vỏ máy bay & Ô tô", desc: "Vật liệu chế tạo hợp kim siêu nhẹ bền." }, { title: "Đồ dùng nhà bếp", desc: "Xoong, nồi, màng bọc thực phẩm." }], why: "Nhôm nhẹ, bền, có lớp oxide bảo vệ chống ăn mòn hiệu quả.", video: "", quiz: { q: "Cấu hình e lớp ngoài cùng của Nhôm có bao nhiêu e?", options: ["1", "2", "3", "4"], correct: 2 } }
        ];

        // Điền dữ liệu giả lập cho 118 nguyên tố nếu thiếu
        const fullElements = Array.from({ length: 118 }, (_, i) => {
            const num = i + 1;
            const existing = elementsData.find(e => e.number === num);
            if (existing) return existing;
            
            // Tạo mẫu nguyên tố chuẩn
            return {
                number: num,
                symbol: `E${num}`,
                name: `Nguyên tố ${num}`,
                mass: (num * 2.1).toFixed(2),
                electro: 1.5,
                config: "ns² npⁿ",
                oxidation: "+1, +2",
                group: num % 2 === 0 ? "metal" : "nonmetal",
                period: Math.ceil(num / 18),
                col: (num - 1) % 18 + 1,
                protons: num,
                shells: [2, 8, Math.max(1, num - 10)],
                desc: `Thông tin chi tiết về nguyên tố số hiệu ${num}.`,
                apps: [{ title: "Công nghiệp", desc: "Ứng dụng trong nghiên cứu và sản xuất." }],
                why: "Có đặc tính lý hóa phù hợp trong phân nhóm.",
                video: "",
                quiz: { q: `Số hiệu nguyên tử của nguyên tố này là bao nhiêu?`, options: [`${num}`, `${num+1}`, `${num-1}`, "0"], correct: 0 }
            };
        });

        let currentElement = null;
        let animationId = null;

        //
