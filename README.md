<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glacier Kumu | 3D Portfolio</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600&family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        :root {
            --pastel-blue: #a8d8ea;
            --pastel-pink: #faccff;
            --pastel-soft-pink: #ffd3e2;
            --pastel-bg-1: #eaf6ff;
            --pastel-bg-2: #ffeaf2;
            --text-main: #4a4e69;
            --text-sub: #6c5ce7;
            --glass-bg: rgba(255, 255, 255, 0.65);
            --glass-border: rgba(255, 255, 255, 0.85);
            --shadow: rgba(168, 216, 234, 0.35);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Kanit', sans-serif;
        }

        body {
            overflow-x: hidden;
            min-height: 100vh;
            background: linear-gradient(135deg, var(--pastel-bg-1) 0%, var(--pastel-bg-2) 100%);
            color: var(--text-main);
        }

        /* Canvas พื้นหลัง 3D */
        #webgl-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* Overlay UI Container */
        .ui-container {
            position: relative;
            z-index: 2;
            width: 100%;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 30px 20px;
        }

        /* Card นำเสนอหลัก (Glassmorphism) */
        .portfolio-card {
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 2px solid var(--glass-border);
            border-radius: 32px;
            padding: 40px;
            max-width: 800px;
            width: 100%;
            box-shadow: 0 20px 40px var(--shadow);
            animation: fadeIn 1s ease-out;
        }

        /* Header Info */
        .header-section {
            text-align: center;
            margin-bottom: 25px;
        }

        .avatar-box {
            width: 90px;
            height: 90px;
            margin: 0 auto 15px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--pastel-blue), var(--pastel-soft-pink));
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 2.2rem;
            border: 4px solid #fff;
            box-shadow: 0 8px 20px var(--shadow);
        }

        h1 {
            font-size: 1.8rem;
            font-weight: 700;
            color: #3d3b62;
        }

        .pen-name {
            font-size: 1.1rem;
            font-weight: 600;
            background: linear-gradient(45deg, #7b2cbf, #ff85a1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 8px;
        }

        .bio-tag {
            display: inline-block;
            background: rgba(255, 255, 255, 0.9);
            padding: 6px 18px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            color: var(--text-sub);
            border: 1px solid rgba(168, 216, 234, 0.4);
        }

        /* Navigation Tabs */
        .tabs-nav {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 30px;
            background: rgba(255, 255, 255, 0.5);
            padding: 6px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.8);
            flex-wrap: wrap;
        }

        .tab-btn {
            border: none;
            background: transparent;
            padding: 10px 20px;
            border-radius: 15px;
            font-size: 0.9rem;
            font-weight: 600;
            color: #5a5a7a;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .tab-btn:hover {
            color: #7b2cbf;
            background: rgba(255, 255, 255, 0.6);
        }

        .tab-btn.active {
            background: #ffffff;
            color: #7b2cbf;
            box-shadow: 0 4px 12px rgba(123, 44, 191, 0.15);
        }

        /* Tab Content Section */
        .tab-content {
            display: none;
            animation: fadeInTab 0.5s ease-in-out;
        }

        .tab-content.active {
            display: block;
        }

        /* About Tab / Contact Grid */
        .about-text {
            text-align: center;
            font-size: 0.95rem;
            line-height: 1.6;
            margin-bottom: 25px;
            color: #555577;
        }

        .social-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
        }

        .social-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            padding: 12px;
            border-radius: 16px;
            background: #ffffff;
            color: var(--text-main);
            text-decoration: none;
            font-size: 0.9rem;
            font-weight: 500;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
            transition: all 0.3s ease;
        }

        .social-btn:hover {
            transform: translateY(-3px);
            color: #fff;
        }

        .social-btn.fb:hover { background: #1877f2; box-shadow: 0 8px 16px rgba(24, 119, 242, 0.3); }
        .social-btn.ig:hover { background: linear-gradient(45deg, #f09433, #dc2743, #bc1888); box-shadow: 0 8px 16px rgba(220, 39, 67, 0.3); }
        .social-btn.x:hover { background: #000; box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); }
        .social-btn.tiktok:hover { background: #000; color: #00f2fe; box-shadow: 0 8px 16px rgba(0, 242, 254, 0.3); }

        /* Gallery Grid (Portfolio Showcase) */
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }

        .art-card {
            background: rgba(255, 255, 255, 0.8);
            border-radius: 18px;
            overflow: hidden;
            border: 1px solid #fff;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.03);
            transition: all 0.3s ease;
        }

        .art-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(168, 216, 234, 0.5);
        }

        .art-placeholder {
            width: 100%;
            height: 180px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: #8e8aa8;
            font-size: 2rem;
            gap: 8px;
        }

        .art-placeholder span {
            font-size: 0.85rem;
            font-weight: 500;
        }

        .art-card.char .art-placeholder { background: linear-gradient(135deg, #e0c3fc, #faccff); }
        .art-card.concept .art-placeholder { background: linear-gradient(135deg, #a8d8ea, #c4faf8); }

        .art-info {
            padding: 12px;
            text-align: center;
        }

        .art-title {
            font-size: 0.9rem;
            font-weight: 600;
            color: #3d3b62;
        }

        /* Keyframes */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes fadeInTab {
            from { opacity: 0; transform: scale(0.98); }
            to { opacity: 1; transform: scale(1); }
        }

        /* Mobile Responsive */
        @media (max-width: 600px) {
            .portfolio-card { padding: 25px 20px; }
            .social-grid { grid-template-columns: 1fr; }
            .tab-btn { padding: 8px 14px; font-size: 0.8rem; }
        }
    </style>
</head>
<body>

    <!-- 3D Canvas Container -->
    <div id="webgl-container"></div>

    <!-- UI Overlay -->
    <div class="ui-container">
        <div class="portfolio-card">
            
            <!-- Header Profile -->
            <div class="header-section">
                <div class="avatar-box">
                    <i class="fa-solid fa-paintbrush"></i>
                </div>
                <h1>Jaranya Tosanhuan</h1>
                <div class="pen-name">Glacier Kumu</div>
                <div class="bio-tag">Character Design & Concept Art</div>
            </div>

            <!-- Navigation Tabs -->
            <div class="tabs-nav">
                <button class="tab-btn active" onclick="switchTab('about')">
                    <i class="fa-solid fa-user"></i> About & Contact
                </button>
                <button class="tab-btn" onclick="switchTab('character')">
                    <i class="fa-solid fa-user-ninja"></i> Character Design
                </button>
                <button class="tab-btn" onclick="switchTab('concept')">
                    <i class="fa-solid fa-mountain-sun"></i> Concept Art
                </button>
            </div>

            <!-- TAB 1: About & Contact -->
            <div id="about" class="tab-content active">
                <p class="about-text">
                    ✨ I love Character Design and Concept art. <br>
                    ยินดีต้อนรับสู่พอร์ตโฟลิโอ 3D สีพาสเทล สามารถเลือกชมผลงานและติดต่องานได้ผ่านช่องทางด้านล่างครับ/ค่ะ
                </p>
                <div class="social-grid">
                    <a href="https://www.facebook.com/glacier.kumu/" target="_blank" rel="noopener noreferrer" class="social-btn fb">
                        <i class="fa-brands fa-facebook"></i> Facebook
                    </a>
                    <a href="https://www.instagram.com/kuximumu_" target="_blank" rel="noopener noreferrer" class="social-btn ig">
                        <i class="fa-brands fa-instagram"></i> Instagram
                    </a>
                    <a href="https://x.com/kuximumu_" target="_blank" rel="noopener noreferrer" class="social-btn x">
                        <i class="fa-brands fa-x-twitter"></i> Twitter (X)
                    </a>
                    <a href="https://www.tiktok.com/@kuximumu_" target="_blank" rel="noopener noreferrer" class="social-btn tiktok">
                        <i class="fa-brands fa-tiktok"></i> TikTok
                    </a>
                </div>
            </div>

            <!-- TAB 2: Character Design -->
            <div id="character" class="tab-content">
                <div class="gallery-grid">
                    <div class="art-card char">
                        <div class="art-placeholder"><i class="fa-solid fa-mask"></i><span>Artwork 01</span></div>
                        <div class="art-info"><div class="art-title">Original Character #1</div></div>
                    </div>
                    <div class="art-card char">
                        <div class="art-placeholder"><i class="fa-solid fa-ghost"></i><span>Artwork 02</span></div>
                        <div class="art-info"><div class="art-title">Original Character #2</div></div>
                    </div>
                    <div class="art-card char">
                        <div class="art-placeholder"><i class="fa-solid fa-wand-magic-sparkles"></i><span>Artwork 03</span></div>
                        <div class="art-info"><div class="art-title">Chibi Concept</div></div>
                    </div>
                </div>
            </div>

            <!-- TAB 3: Concept Art -->
            <div id="concept" class="tab-content">
                <div class="gallery-grid">
                    <div class="art-card concept">
                        <div class="art-placeholder"><i class="fa-solid fa-cloud-moon"></i><span>Artwork 01</span></div>
                        <div class="art-info"><div class="art-title">Pastel Fantasy World</div></div>
                    </div>
                    <div class="art-card concept">
                        <div class="art-placeholder"><i class="fa-solid fa-castle"></i><span>Artwork 02</span></div>
                        <div class="art-info"><div class="art-title">Environment Design</div></div>
                    </div>
                    <div class="art-card concept">
                        <div class="art-placeholder"><i class="fa-solid fa-tree"></i><span>Artwork 03</span></div>
                        <div class="art-info"><div class="art-title">Background Concept</div></div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <!-- Import Three.js Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <script>
        // --- 1. TAB SWITCHING SYSTEM ---
        function switchTab(tabId) {
            // Hide all tabs
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            // Remove active style from buttons
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            // Show selected tab & active button
            document.getElementById(tabId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        // --- 2. THREE.JS 3D BACKGROUND ---
        const container = document.getElementById('webgl-container');
        const scene = new THREE.Scene();

        // Pastel Fog Background
        scene.fog = new THREE.FogExp2(0xeaf6ff, 0.015);

        const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 28;

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        container.appendChild(renderer.domElement);

        // Lights
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.8);
        scene.add(ambientLight);

        const blueLight = new THREE.DirectionalLight(0xa8d8ea, 1.2);
        blueLight.position.set(-10, 10, 10);
        scene.add(blueLight);

        const pinkLight = new THREE.DirectionalLight(0xfaccff, 1.2);
        pinkLight.position.set(10, -10, 10);
        scene.add(pinkLight);

        // Floating Pastel Shapes
        const shapes = [];
        const pastelColors = [0xa8d8ea, 0xfaccff, 0xffd3e2, 0xe0c3fc, 0xc4faf8];
        const geometries = [
            new THREE.IcosahedronGeometry(1.2, 0),
            new THREE.TorusGeometry(1, 0.4, 16, 40),
            new THREE.OctahedronGeometry(1.2, 0),
            new THREE.SphereGeometry(1, 32, 32)
        ];

        for (let i = 0; i < 40; i++) {
            const geom = geometries[Math.floor(Math.random() * geometries.length)];
            const color = pastelColors[Math.floor(Math.random() * pastelColors.length)];

            const mat = new THREE.MeshPhongMaterial({
                color: color,
                shininess: 70,
                flatShading: true,
                transparent: true,
                opacity: 0.8
            });

            const mesh = new THREE.Mesh(geom, mat);
            mesh.position.x = (Math.random() - 0.5) * 50;
            mesh.position.y = (Math.random() - 0.5) * 50;
            mesh.position.z = (Math.random() - 0.5) * 30 - 5;

            mesh.rotation.x = Math.random() * Math.PI;
            mesh.rotation.y = Math.random() * Math.PI;

            mesh.userData = {
                rotSpeedX: (Math.random() - 0.5) * 0.012,
                rotSpeedY: (Math.random() - 0.5) * 0.012,
                floatSpeed: Math.random() * 0.015 + 0.005,
                floatOffset: Math.random() * Math.PI * 2
            };

            scene.add(mesh);
            shapes.push(mesh);
        }

        // Mouse Parallax Movement
        let mouseX = 0;
        let mouseY = 0;

        window.addEventListener('mousemove', (e) => {
            mouseX = (e.clientX / window.innerWidth - 0.5) * 2;
            mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
        });

        // Animation Loop
        const clock = new THREE.Clock();

        function animate() {
            requestAnimationFrame(animate);
            const elapsedTime = clock.getElapsedTime();

            shapes.forEach(shape => {
                shape.rotation.x += shape.userData.rotSpeedX;
                shape.rotation.y += shape.userData.rotSpeedY;
                shape.position.y += Math.sin(elapsedTime * 1.5 + shape.userData.floatOffset) * 0.012;
            });

            camera.position.x += (mouseX * 3 - camera.position.x) * 0.05;
            camera.position.y += (-mouseY * 3 - camera.position.y) * 0.05;
            camera.lookAt(scene.position);

            renderer.render(scene, camera);
        }

        animate();

        // Resize Listener
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
