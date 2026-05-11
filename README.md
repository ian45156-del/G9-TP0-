# G9-TP0-<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>上銀科技企業心智圖</title>
    
    <!-- 引入 Tailwind CSS 進行快速排版 -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 引入 Lucide Icons 圖示庫 -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- 自訂配色主題 (參照現代設計比賽常見的高質感配色) -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            dark: '#0a192f',      // 深邃藍背景
                            sidebar: '#112240',   // 側邊欄與卡片背景
                            border: '#233554',    // 邊界線條
                            textBase: '#ccd6f6',  // 主要文字
                            textMuted: '#8892b0', // 次要/說明文字
                            accent: '#64ffda',    // 螢光青強調色
                            accentHover: '#52e0c4'// 螢光青(Hover)
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* 自訂淡入動畫 */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fadeIn 0.4s ease-out forwards;
        }
        
        /* 隱藏預設捲動條，美化畫面 */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #233554; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #8892b0; }
    </style>
</head>
<body class="bg-brand-dark text-brand-textBase font-sans flex flex-col md:flex-row h-screen overflow-hidden selection:bg-brand-accent selection:text-brand-dark">

    <!-- 1. 左側選單 (Sidebar) -->
    <aside class="w-full md:w-64 lg:w-72 bg-brand-sidebar border-b md:border-b-0 md:border-r border-brand-border flex flex-col shrink-0 z-10 shadow-xl">
        
        <!-- 標題區塊 -->
        <div class="p-6 border-b border-brand-border">
            <h1 class="text-xl md:text-2xl font-bold text-brand-textBase flex items-center tracking-wide">
                <i data-lucide="network" class="w-7 h-7 mr-3 text-brand-accent"></i>
                HIWIN
            </h1>
            <p class="text-sm text-brand-textMuted mt-2 tracking-widest uppercase">上銀科技心智圖</p>
        </div>
        
        <!-- 導覽選單 -->
        <nav class="flex-1 p-4 flex flex-col gap-2 overflow-y-auto">
            <div class="text-xs font-semibold text-brand-textMuted mb-2 ml-2 tracking-wider">導覽選單 MENU</div>
            
            <a href="#" class="flex items-center px-4 py-3 bg-brand-accent/10 text-brand-accent rounded-lg border border-brand-accent/30 transition-all">
                <i data-lucide="layout-template" class="w-5 h-5 mr-3"></i>
                <span class="font-medium">心智圖導覽</span>
            </a>
            
            <!-- 3. 新增的「引用資料」選單 -->
            <a href="https://jeremywang0121.github.io/CDPS_First-Place-in-114_webpage-competition/" target="_blank" rel="noopener noreferrer" 
               class="flex items-center px-4 py-3 text-brand-textMuted hover:bg-brand-dark hover:text-brand-textBase rounded-lg border border-transparent hover:border-brand-border transition-all group">
                <i data-lucide="external-link" class="w-5 h-5 mr-3 group-hover:text-brand-accent transition-colors"></i>
                <span class="font-medium">引用資料</span>
            </a>
        </nav>
        
        <!-- 全域操作按鈕 -->
        <div class="p-4 border-t border-brand-border">
            <div class="flex gap-3">
                <button onclick="expandAll()" class="flex-1 py-2.5 bg-brand-dark hover:bg-brand-border text-brand-textMuted hover:text-brand-textBase text-sm font-medium rounded-lg transition-all border border-brand-border hover:border-brand-textMuted">
                    展開全部
                </button>
                <button onclick="collapseAll()" class="flex-1 py-2.5 bg-brand-dark hover:bg-brand-border text-brand-textMuted hover:text-brand-textBase text-sm font-medium rounded-lg transition-all border border-brand-border hover:border-brand-textMuted">
                    收合全部
                </button>
            </div>
        </div>
    </aside>

    <!-- 主內容區 (Main Content) -->
    <main class="flex-1 overflow-y-auto p-4 md:p-8 flex flex-col lg:flex-row gap-6 lg:gap-8">
        
        <!-- 左半部：心智圖樹狀結構 -->
        <section class="w-full lg:w-3/5 bg-brand-sidebar rounded-2xl shadow-lg border border-brand-border p-5 md:p-6 overflow-x-auto">
            <h2 class="text-sm font-bold text-brand-textMuted mb-6 pb-4 border-b border-brand-border flex items-center uppercase tracking-widest">
                <i data-lucide="git-merge" class="w-4 h-4 mr-2"></i>
                結構檢視 Structure
            </h2>
            <div id="tree-container" class="min-w-max pb-4 pr-4">
                <!-- 樹狀圖將由 JavaScript 動態渲染於此 -->
            </div>
        </section>

        <!-- 右半部：詳細資訊儀表板 -->
        <section class="w-full lg:w-2/5">
            <div id="details-panel" class="bg-brand-sidebar rounded-2xl shadow-lg border border-brand-border p-6 md:p-8 sticky top-8">
                <!-- 詳細資訊將由 JavaScript 動態渲染於此 -->
            </div>
        </section>

    </main>

    <!-- 系統邏輯腳本 -->
    <script>
        // 上銀科技的心智圖資料
        const hiwinData = {
            id: 'root',
            label: '上銀科技 (HIWIN Technologies)',
            icon: 'Network',
            description: '上銀科技是全球傳動控制與系統科技的領導品牌，致力於線性滑軌、滾珠螺桿、工業機器人等精密零組件的研發與製造，為全球客戶提供機電整合解決方案。',
            children: [
                {
                    id: 'profile',
                    label: '公司概況',
                    icon: 'Info',
                    description: '成立於 1989 年，總部位於台灣台中，以「專業水準、工作熱誠、職業道德」為企業核心理念。',
                    children: [
                        { id: 'p1', label: '創立年份：1989年', icon: 'Award', description: '由卓永財總裁創立，深耕精密機械領域三十餘年。' },
                        { id: 'p2', label: '全球總部：台灣台中', icon: 'MapPin', description: '位於台灣台中市精密機械科技創新園區。' },
                        { id: 'p3', label: '品牌涵義', icon: 'Target', description: 'HIWIN 源自 Hi-tech Winner，代表與客戶共同成為高科技領域的贏家。' }
                    ]
                },
                {
                    id: 'products',
                    label: '核心產品與技術',
                    icon: 'Box',
                    description: '提供全方位的傳動控制元件與次系統，涵蓋從基礎零件到高階智慧製造設備。',
                    children: [
                        { 
                            id: 'prod1', 
                            label: '傳動元件', 
                            icon: 'Settings',
                            description: '包含滾珠螺桿 (Ballscrews)、線性滑軌 (Linear Guideways)、特殊軸承等。',
                            children: [
                                { id: 'prod1-1', label: '精密滾珠螺桿', icon: 'Circle', description: '高精度、低噪音、長壽命的傳動元件。' },
                                { id: 'prod1-2', label: '線性滑軌', icon: 'Circle', description: '具備高剛性、重負荷能力的線性運動導引元件。' }
                            ]
                        },
                        { 
                            id: 'prod2', 
                            label: '智慧自動化與機器人', 
                            icon: 'Cpu',
                            description: '多關節機器人、晶圓機器人、單軸機器人及醫療機器人。',
                            children: [
                                { id: 'prod2-1', label: '工業機器人', icon: 'Circle', description: '適用於焊接、搬運、組裝等自動化產線。' },
                                { id: 'prod2-2', label: '醫療機器人', icon: 'Circle', description: '下肢復健機、內視鏡扶持機器手臂等精準醫療設備。' }
                            ]
                        }
                    ]
                },
                {
                    id: 'global',
                    label: '全球佈局',
                    icon: 'Globe',
                    description: '在歐、美、亞太地區設有多家海外子公司與研發中心，提供全球化的在地服務。',
                    children: [
                        { id: 'g1', label: '歐洲市場', icon: 'MapPin', description: '德國 (歐總部)、義大利、瑞士、法國等分公司與研發中心。' },
                        { id: 'g2', label: '亞洲市場', icon: 'MapPin', description: '日本、韓國、新加坡、中國大陸等地設有據點與廠房。' },
                        { id: 'g3', label: '美洲市場', icon: 'MapPin', description: '美國芝加哥、矽谷等地設有據點，服務北美客戶。' }
                    ]
                },
                {
                    id: 'applications',
                    label: '產業應用領域',
                    icon: 'Target',
                    description: '產品廣泛應用於各類高科技與傳統產業，推動產業升級。',
                    children: [
                        { id: 'app1', label: '半導體設備', icon: 'Circle', description: '高精度定位與晶圓搬運系統。' },
                        { id: 'app2', label: '工具機與自動化', icon: 'Circle', description: 'CNC銑床、車床及各類自動化生產線。' },
                        { id: 'app3', label: '醫療與生技', icon: 'Circle', description: '復健設備、微創手術設備零組件。' },
                        { id: 'app4', label: '綠能與電動車', icon: 'Circle', description: '風力發電設備零組件、電動車相關製造設備。' }
                    ]
                },
                {
                    id: 'esg',
                    label: '企業永續與 ESG',
                    icon: 'Leaf',
                    description: '致力於環境保護、社會責任與公司治理，推動綠色製造與人才培育。',
                    children: [
                        { id: 'esg1', label: '綠色製造', icon: 'Leaf', description: '推動節能減碳、水資源回收與綠色供應鏈管理。' },
                        { id: 'esg2', label: '產學合作與人才', icon: 'Users', description: '舉辦上銀機械碩士論文獎，長期培育機電整合人才。' }
                    ]
                }
            ]
        };

        // 圖示名稱對照表 (配合 Lucide Icons)
        const iconMap = {
            'Network': 'network',
            'Info': 'info',
            'Box': 'package',
            'Globe': 'globe',
            'Cpu': 'cpu',
            'Target': 'target',
            'Users': 'users',
            'Leaf': 'leaf',
            'MapPin': 'map-pin',
            'Award': 'award',
            'Settings': 'settings',
            'Circle': 'circle-dot'
        };

        function getIconName(icon) {
            return iconMap[icon] || 'circle';
        }

        // 應用程式狀態
        let expandedNodes = new Set(['root', 'profile', 'products', 'global', 'applications', 'esg']);
        let selectedNode = hiwinData;

        // 遞迴尋找節點
        function findNode(id, currentNode = hiwinData) {
            if (currentNode.id === id) return currentNode;
            if (currentNode.children) {
                for (let child of currentNode.children) {
                    const found = findNode(id, child);
                    if (found) return found;
                }
            }
            return null;
        }

        // 展開/收合圖示點擊處理
        window.handleToggle = function(event, id) {
            event.stopPropagation();
            if (expandedNodes.has(id)) {
                expandedNodes.delete(id);
            } else {
                expandedNodes.add(id);
            }
            renderApp();
        }

        // 節點點擊處理
        window.handleNodeClick = function(id) {
            selectedNode = findNode(id);
            if (selectedNode && selectedNode.children && selectedNode.children.length > 0) {
                expandedNodes.add(id);
            }
            renderApp();
        }

        // 展開全部
        window.expandAll = function() {
            const collectIds = (node) => {
                expandedNodes.add(node.id);
                if (node.children) node.children.forEach(collectIds);
            };
            collectIds(hiwinData);
            renderApp();
        }

        // 收合全部
        window.collapseAll = function() {
            expandedNodes.clear();
            expandedNodes.add('root');
            renderApp();
        }

        // 產生樹狀結構的 HTML
        function buildTreeHTML(node, level = 0) {
            const hasChildren = node.children && node.children.length > 0;
            const isExpanded = expandedNodes.has(node.id);
            const isSelected = selectedNode && selectedNode.id === node.id;

            let html = `<div class="select-none">`;
            
            // 節點本體
            html += `<div class="flex items-center py-2.5 px-3 my-1.5 rounded-xl cursor-pointer transition-all duration-300 border
                     ${isSelected ? 'bg-brand-accent/10 border-brand-accent/50 shadow-[0_0_15px_rgba(100,255,218,0.1)]' : 'hover:bg-brand-dark border-transparent hover:border-brand-border'}
                     ${level === 0 ? 'bg-brand-dark/50 border-brand-border' : ''}"
                     style="margin-left: ${level * 1.75}rem"
                     onclick="handleNodeClick('${node.id}')">`;
                     
            // 展開箭頭
            html += `<div class="w-6 flex justify-center items-center mr-2" onclick="handleToggle(event, '${node.id}')">`;
            if (hasChildren) {
                html += `<i data-lucide="${isExpanded ? 'chevron-down' : 'chevron-right'}" class="w-4 h-4 text-brand-textMuted hover:text-brand-textBase transition-colors"></i>`;
            } else {
                html += `<div class="w-4 h-4"></div>`;
            }
            html += `</div>`;

            // 圖示
            html += `<div class="mr-3">
                        <i data-lucide="${getIconName(node.icon)}" class="w-5 h-5 ${isSelected ? 'text-brand-accent' : 'text-brand-textMuted'}"></i>
                     </div>`;

            // 文字標籤
            html += `<span class="text-sm md:text-base tracking-wide ${level === 0 ? 'font-bold text-brand-accent' : (isSelected ? 'font-bold text-brand-textBase' : 'text-brand-textBase font-medium')}">${node.label}</span>`;
            
            html += `</div>`;

            // 子節點渲染
            if (hasChildren && isExpanded) {
                html += `<div class="relative">`;
                // 左側引導線
                html += `<div class="absolute top-0 bottom-0 w-px bg-brand-border/50" style="left: ${(level + 1) * 1.75 - 0.75}rem"></div>`;
                node.children.forEach(child => {
                    html += buildTreeHTML(child, level + 1);
                });
                html += `</div>`;
            }
            
            html += `</div>`;
            return html;
        }

        // 更新詳細資訊面板
        function updateDetailsPanel() {
            const panel = document.getElementById('details-panel');
            if (!selectedNode) {
                panel.innerHTML = `
                    <div class="flex flex-col items-center justify-center h-64 text-brand-textMuted">
                        <i data-lucide="mouse-pointer-click" class="w-12 h-12 mb-4 opacity-50"></i>
                        <p class="tracking-wide">請從左側點擊任一節點以查看詳細資訊</p>
                    </div>`;
                return;
            }

            let html = `
                <div class="animate-fade-in">
                    <h2 class="text-xs font-bold tracking-[0.2em] text-brand-accent uppercase mb-2">節點詳細資訊</h2>
                    
                    <div class="flex items-center mt-4 mb-6 pb-6 border-b border-brand-border">
                        <div class="p-4 bg-brand-dark rounded-xl mr-5 text-brand-accent shadow-inner border border-brand-border/50">
                            <i data-lucide="${getIconName(selectedNode.icon)}" class="w-8 h-8"></i>
                        </div>
                        <h3 class="text-2xl lg:text-3xl font-bold text-brand-textBase leading-tight tracking-wide">
                            ${selectedNode.label}
                        </h3>
                    </div>
                    
                    <div class="prose prose-invert max-w-none mb-8">
                        ${selectedNode.description ? 
                            `<p class="text-brand-textBase leading-relaxed text-lg tracking-wide opacity-90">${selectedNode.description}</p>` : 
                            `<p class="text-brand-textMuted italic tracking-wide">尚無詳細描述。</p>`
                        }
                    </div>
            `;

            // 如果有子項目，則以卡片形式呈現摘要
            if (selectedNode.children && selectedNode.children.length > 0) {
                html += `
                    <div class="mt-8">
                        <h4 class="text-xs font-bold text-brand-textMuted mb-4 uppercase tracking-[0.2em] flex items-center">
                            <i data-lucide="layers" class="w-4 h-4 mr-2"></i> 包含子項目
                        </h4>
                        <div class="grid grid-cols-1 gap-3">
                `;
                selectedNode.children.forEach(child => {
                    html += `
                        <div class="flex items-start p-4 bg-brand-dark/50 hover:bg-brand-dark rounded-xl cursor-pointer transition-all duration-300 border border-brand-border hover:border-brand-accent/50 group"
                             onclick="handleNodeClick('${child.id}')">
                            <div class="mt-0.5 mr-4 text-brand-textMuted group-hover:text-brand-accent transition-colors duration-300">
                                <i data-lucide="${getIconName(child.icon)}" class="w-5 h-5"></i>
                            </div>
                            <div>
                                <div class="font-bold text-brand-textBase text-base mb-1 tracking-wide group-hover:text-brand-accent transition-colors duration-300">${child.label}</div>
                                ${child.description ? `<div class="text-sm text-brand-textMuted line-clamp-2 tracking-wide leading-relaxed">${child.description}</div>` : ''}
                            </div>
                        </div>
                    `;
                });
                html += `</div></div>`;
            }

            html += `</div>`;
            panel.innerHTML = html;
        }

        // 主要渲染函數
        function renderApp() {
            document.getElementById('tree-container').innerHTML = buildTreeHTML(hiwinData, 0);
            updateDetailsPanel();
            lucide.createIcons(); // 每次重繪後重新載入圖示
        }

        // 初始化應用程式
        window.onload = () => {
            renderApp();
        };

    </script>
</body>
</html>
