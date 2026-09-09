<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glacier Kumu | 3D Interactive Portfolio</title>
    
    <!-- Google Fonts & FontAwesome Icons -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&family=Outfit:wght@400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        :root {
            --pastel-blue: #a8d8ea;
            --pastel-pink: #faccff;
            --pastel-purple: #c4faf8;
            --pastel-accent: #7b2cbf;
            --text-dark: #2b2d42;
            --text-muted: #6c757d;
            --glass-card: rgba(255, 255, 255, 0.45);
            --glass-border: rgba(255, 255, 255, 0.8);
            --glass-shadow: 0 20px 50px rgba(168, 216, 234, 0.4);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Outfit', 'Kanit', sans-serif;
            user-select: none;
        }

        body {
            overflow: hidden;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, #eaf6ff 0%, #ffeaf2 50%, #f3e8ff 100%);
            color: var(--text-dark);
        }

        /* Three.js Canvas Container */
        #webgl-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* Overlay UI Container */
        .ui-layout {
            position: relative;
            z-index: 2;
            width: 100%;
            height: 100vh;
            display: grid;
            grid-template-columns: 360px 1fr;
            gap: 20px;
            padding: 30px;
            pointer-events: none;
        }

        /* Glassmorphism Sidebar (Profile Card) */
        .sidebar {
            pointer-events: auto;
            background: var(--glass-card);
            backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px);
            border: 2px solid var(--glass-border);
            border-radius: 30px;
            padding: 35px 25px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-shadow: var(--glass-shadow);
            animation: slideInLeft 1s cubic-bezier(0.16, 1, 0.3, 1);
        }

        .profile-header {
            text-align: center;
        }

        .profile-badge {
            width: 80px;
            height: 80px;
            margin: 0 auto 15px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--pastel-blue), var(--pastel-pink));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            color: #fff;
            box-shadow: 0 10px 25px rgba(250, 204, 255, 0.6);
            border: 3px solid #fff;
        }

        h1 {
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--text-dark);
            letter-spacing: -0.5px;
        }

        .pen-name {
            font-size: 1.1rem;
            font-weight: 700;
            background: linear-gradient(45deg, #7b2cbf, #ff85a1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 12px;
        }

        .bio-tag {
            display: inline-block;
            background: rgba(255, 255, 255, 0.85);
            padding: 6px 16px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            color: #5a5a7a;
            border: 1px solid rgba(255, 255, 255, 0.9);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.03);
            margin-bottom: 15px;
        }

        .bio-desc {
            font-size: 0.85rem;
            line-height: 1.6;
            color: var(--text-muted);
            margin-bottom: 20px;
        }

        /* Navigation Tabs */
        .nav-tabs {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }

        .tab-btn {
            border: none;
            background: rgba(255, 255, 255, 0.6);
            padding: 14px 20px;
            border-radius: 18px;
            font-size: 0.9rem;
            font-weight: 600;
            color: var(--text-dark);
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 12px;
            transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
            border: 1px solid rgba(255, 255, 255, 0.8);
        }

        .tab-btn i {
            font-size: 1.1rem;
            color: var(--pastel-accent);
        }

        .tab-btn:hover {
            background: #ffffff;
            transform: translateX(5px);
            box-shadow: 0 8px 20px rgba(168, 216, 234, 0.4);
        }

        .tab-btn.active {
            background: #ffffff;
            border-color: var(--pastel-pink);
            box-shadow: 0 10px 25px rgba(250, 204, 255, 0.5);
            transform: translateX(8px);
        }

        /* Contact Social Grid */
        .social-title {
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: #8a8aa0;
            margin-bottom: 10px;
        }

        .social-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .social-link {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 10px;
            border-radius: 12px;
            background: #ffffff;
            color: var(--text-dark);
            text-decoration: none;
            font-size: 0.8rem;
            font-weight: 600;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.9);
            transition: all 0.3s ease;
        }

        .social-link:hover {
            transform: translateY(-3px) scale(1.02);
            color: #fff;
        }

        .social-link.fb:hover { background: #1877f2; }
        .social-link.ig:hover { background: linear-gradient(45deg, #f09433, #dc2743, #bc1888); }
        .social-link.x:hover { background: #000; }
        .social-link.tiktok:hover { background: #000; color: #00f2fe; }

        /* Main Content Showcase Display */
        .main-content {
            pointer-events: auto;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .tab-pane {
            display: none;
            width: 100%;
            height: 100%;
            max-height: 80vh;
            overflow-y: auto;
            padding: 20px;
            animation: fadeIn 0.6s cubic-bezier(0.16, 1, 0.3, 1);
        }

        .tab-pane.active {
            display: block;
        }

        /* Custom Scrollbar */
        .tab-pane::-webkit-scrollbar {
            width: 6px;
        }
        .tab-pane::-webkit-scrollbar-thumb {
            background: rgba(250, 204, 255, 0.8);
            border-radius: 10px;
        }

        /* Showcase Grid */
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
            gap: 20px;
        }

        .art-card {
            background: var(--glass-card);
            backdrop-filter: blur(15px);
            border: 2px solid var(--glass-border);
            border-radius: 24px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
        }

        .art-card:hover {
            transform: translateY(-10px) rotate(1deg);
            box-shadow: 0 20px 40px rgba(168, 216, 234, 0.5);
            border-color: #fff;
        }

        .art-thumb {
            height: 200px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            color: rgba(255, 255, 255, 0.9);
            position: relative;
        }

        .art-card.char .art-thumb { background: linear-gradient(135deg, #a8d8ea, #faccff); }
        .art-card.concept .art-thumb { background: linear-gradient(135deg, #ffd3e2, #c4faf8); }

        .art-meta {
            padding: 15px;
            background: rgba(255, 255, 255, 0.8);
        }

        .art-meta h3 {
            font-size: 1rem;
            font-weight: 700;
            color: var(--text-dark);
            margin-bottom: 4px;
        }

        .art-meta p {
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        /* Keyframe Animations */
        @keyframes slideInLeft {
            from { opacity: 0; transform: translateX(-50px); }
            to { opacity: 1; transform: translateX(0); }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }

        /* Responsive */
        @media (max-width: 900px) {
            .ui-layout {
                grid-template-columns: 1fr;
                overflow-y: auto;
                height: auto;
            }
            body { overflow-y: auto; }
            .sidebar { max-width: 100%; }
        }
    </style>
</head>
<body>

    <!-- 3D Canvas Background -->
    <div id="webgl-container"></div>

    <!-- UI Overlay -->
    <div class="ui-layout">
        
        <!-- Sidebar Profile -->
        <div class="sidebar">
            <div>
                <div class="profile-header">
                    <div class="profile-badge">
                        <i class="fa-solid fa-wand-magic-sparkles"></i>
                    </div>
                    <h1>Jaranya Tosanhuan</h1>
                    <div class="pen-name">Glacier Kumu</div>
                    <div class="bio-tag">Character Design & Concept Art</div>
                </div>

                <p class="bio-desc">
                    ✨ I love Character Design and Concept art.<br>
                    ยินดีต้อนรับสู่โลกพาสเทล 3D มอนิเตอร์น้องนก Cockatiel ด้านหลังสามารถหันตามเมาส์ได้ครับ!
                </p>

                <!-- Tabs Navigation -->
                <div class="nav-tabs">
                    <button class="tab-btn active" onclick="switchTab('about', this)">
                        <i class="fa-solid fa-heart"></i> About & Contact
                    </button>
                    <button class="tab-btn" onclick="switchTab('character', this)">
                        <i class="fa-solid fa-user-astronaut"></i> Character Design
                    </button>
                    <button class="tab-btn" onclick="switchTab('concept', this)">
                        <i class="fa-solid fa-paint-brush"></i> Concept Art
                    </button>
                </div>
            </div>

            <!-- Social Links -->
            <div>
                <div class="social-title">Social Contact</div>
                <div class="social-grid">
                    <a href="https://www.facebook.com/glacier.kumu/" target="_blank" rel="noopener noreferrer" class="social-link fb">
                        <i class="fa-brands fa-facebook"></i> Facebook
                    </a>
                    <a href="https://www.instagram.com/kuximumu_" target="_blank" rel="noopener noreferrer" class="social-link ig">
                        <i class="fa-brands fa-instagram"></i> Instagram
                    </a>
                    <a href="https://x.com/kuximumu_" target="_blank" rel="noopener noreferrer" class="social-link x">
                        <i class="fa-brands fa-x-twitter"></i> Twitter (X)
                    </a>
                    <a href="https://www.tiktok.com/@kuximumu_" target="_blank" rel="noopener noreferrer" class="social-link tiktok">
                        <i class="fa-brands fa-tiktok"></i> TikTok
                    </a>
                </div>
            </div>
        </div>

        <!-- Main Content Area -->
        <div class="main-content">
            
            <!-- Tab 1: About -->
            <div id="about" class="tab-pane active">
                <!-- Blank for full 3D Cockatiel view -->
            </div>

            <!-- Tab 2: Character Design Showcase -->
            <div id="character" class="tab-pane">
                <div class="gallery-grid">
                    <div class="art-card char">
                        <div class="art-thumb"><i class="fa-solid fa-ghost"></i></div>
                        <div class="art-meta">
                            <h3>Pastel Guardian</h3>
                            <p>Character Design / Original Concept</p>
                        </div>
                    </div>
                    <div class="art-card char">
                        <div class="art-thumb"><i class="fa-solid fa-hat-wizard"></i></div>
                        <div class="art-meta">
                            <h3>Sky Sorcerer</h3>
                            <p>Character Sheet / Costume Design</p>
                        </div>
                    </div>
                    <div class="art-card char">
                        <div class="art-thumb"><i class="fa-solid fa-dragon"></i></div>
                        <div class="art-meta">
                            <h3>Cloud Mascot</h3>
                            <p>Creature & Chibi Design</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Tab 3: Concept Art Showcase -->
            <div id="concept" class="tab-pane">
                <div class="gallery-grid">
                    <div class="art-card concept">
                        <div class="art-thumb"><i class="fa-solid fa-cloud-sun"></i></div>
                        <div class="art-meta">
                            <h3>Floating Citadel</h3>
                            <p>Environment Concept Art</p>
                        </div>
                    </div>
                    <div class="art-card concept">
                        <div class="art-thumb"><i class="fa-solid fa-crystal-ball"></i></div>
                        <div class="art-meta">
                            <h3>Crystal Sanctuary</h3>
                            <p>Color Key & Moodboard</p>
                        </div>
                    </div>
                    <div class="art-card concept">
                        <div class="art-thumb"><i class="fa-solid fa-monument"></i></div>
                        <div class="art-meta">
                            <h3>Pastel World Engine</h3>
                            <p>Prop & Architecture Design</p>
                        </div>
                    </div>
                </div>
            </div>

        </div>

    </div>

    <!-- Three.js & OrbitControls -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <script>
        // --- 1. THREE.JS SCENE SETUP ---
        const container = document.getElementById('webgl-container');
        const scene = new THREE.Scene();
        scene.fog = new THREE.FogExp2(0xeaf6ff, 0.012);

        const camera = new THREE.PerspectiveCamera(55, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(2, 1, 14);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.shadowMap.enabled = true;
        container.appendChild(renderer.domElement);

        // --- 2. LIGHTING (PASTEL ATMOSPHERE) ---
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.9);
        scene.add(ambientLight);

        const blueLight = new THREE.DirectionalLight(0xa8d8ea, 1.5);
        blueLight.position.set(-10, 12, 10);
        scene.add(blueLight);

        const pinkLight = new THREE.DirectionalLight(0xfaccff, 1.5);
        pinkLight.position.set(10, -10, 8);
        scene.add(pinkLight);

        // --- 3. 3D COCKATIEL MASCOT CREATION ---
        const cockatielGroup = new THREE.Group();

        // Pastel Materials
        const bodyMat = new THREE.MeshPhongMaterial({ color: 0xfff8ee, flatShading: true });
        const headMat = new THREE.MeshPhongMaterial({ color: 0xffea85, flatShading: true });
        const cheekMat = new THREE.MeshPhongMaterial({ color: 0xffa3a5, flatShading: true });
        const beakMat = new THREE.MeshPhongMaterial({ color: 0xd8b4fe, flatShading: true });
        const eyeMat = new THREE.MeshBasicMaterial({ color: 0x3d3b62 });
        const wingMat = new THREE.MeshPhongMaterial({ color: 0xa8d8ea, flatShading: true });
        const tailMat = new THREE.MeshPhongMaterial({ color: 0xfaccff, flatShading: true });

        // Body
        const bodyGeom = new THREE.SphereGeometry(1.8, 16, 16);
        bodyGeom.scale(1, 1.3, 0.9);
        const body = new THREE.Mesh(bodyGeom, bodyMat);
        cockatielGroup.add(body);

        // Head Group (for independent rotation)
        const headGroup = new THREE.Group();
        headGroup.position.set(0, 2.0, 0.2);

        const headGeom = new THREE.SphereGeometry(1.3, 16, 16);
        const head = new THREE.Mesh(headGeom, headMat);
        headGroup.add(head);

        // Beak
        const beakGeom = new THREE.ConeGeometry(0.35, 0.7, 4);
        const beak = new THREE.Mesh(beakGeom, beakMat);
        beak.position.set(0, -0.2, 1.3);
        beak.rotation.x = Math.PI / 3;
        headGroup.add(beak);

        // Crest (Feathers)
        for (let i = 0; i < 4; i++) {
            const crestGeom = new THREE.ConeGeometry(0.15, 1.4 - i * 0.2, 4);
            const crest = new THREE.Mesh(crestGeom, headMat);
            crest.position.set(0, 1.2 + i * 0.15, -i * 0.15);
            crest.rotation.x = -0.2 - i * 0.15;
            headGroup.add(crest);
        }

        // Cheeks
        const cheekGeom = new THREE.CylinderGeometry(0.4, 0.4, 0.08, 12);
        const leftCheek = new THREE.Mesh(cheekGeom, cheekMat);
        leftCheek.position.set(-1.0, -0.2, 0.8);
        leftCheek.rotation.z = Math.PI / 2;
        leftCheek.rotation.y = -Math.PI / 6;

        const rightCheek = leftCheek.clone();
        rightCheek.position.set(1.0, -0.2, 0.8);
        rightCheek.rotation.y = Math.PI / 6;

        headGroup.add(leftCheek);
        headGroup.add(rightCheek);

        // Eyes
        const eyeGeom = new THREE.SphereGeometry(0.16, 8, 8);
        const leftEye = new THREE.Mesh(eyeGeom, eyeMat);
        leftEye.position.set(-0.75, 0.1, 0.95);

        const rightEye = leftEye.clone();
        rightEye.position.set(0.75, 0.1, 0.95);

        headGroup.add(leftEye);
        headGroup.add(rightEye);

        cockatielGroup.add(headGroup);

        // Wings Pivot
        const leftWingPivot = new THREE.Group();
        leftWingPivot.position.set(-1.6, 0.6, 0);
        const wingGeom = new THREE.ConeGeometry(1.0, 3.2, 4);
        const leftWing = new THREE.Mesh(wingGeom, wingMat);
        leftWing.position.set(-0.2, -1.2, 0);
        leftWing.rotation.z = 0.2;
        leftWingPivot.add(leftWing);
        cockatielGroup.add(leftWingPivot);

        const rightWingPivot = new THREE.Group();
        rightWingPivot.position.set(1.6, 0.6, 0);
        const rightWing = new THREE.Mesh(wingGeom, wingMat);
        rightWing.position.set(0.2, -1.2, 0);
        rightWing.rotation.z = -0.2;
        rightWingPivot.add(rightWing);
        cockatielGroup.add(rightWingPivot);

        // Tail
        const tailGeom = new THREE.BoxGeometry(0.7, 3.2, 0.08);
        const tail = new THREE.Mesh(tailGeom, tailMat);
        tail.position.set(0, -2.0, -0.8);
        tail.rotation.x = -Math.PI / 6;
        cockatielGroup.add(tail);

        cockatielGroup.position.set(2, 0, 0);
        cockatielGroup.scale.set(1.4, 1.4, 1.4);
        scene.add(cockatielGroup);

        // --- 4. MAGICAL PASTEL PARTICLES ---
        const particleCount = 120;
        const particleGeom = new THREE.BufferGeometry();
        const positions = new Float32Array(particleCount * 3);
        const colors = new Float32Array(particleCount * 3);

        const pColors = [new THREE.Color('#a8d8ea'), new THREE.Color('#faccff'), new THREE.Color('#ffd3e2')];

        for (let i = 0; i < particleCount; i++) {
            positions[i * 3] = (Math.random() - 0.5) * 40;
            positions[i * 3 + 1] = (Math.random() - 0.5) * 40;
            positions[i * 3 + 2] = (Math.random() - 0.5) * 20;

            const c = pColors[Math.floor(Math.random() * pColors.length)];
            colors[i * 3] = c.r;
            colors[i * 3 + 1] = c.g;
            colors[i * 3 + 2] = c.b;
        }

        particleGeom.setAttribute('position', new THREE.BufferAttribute(positions, 3));
        particleGeom.setAttribute('color', new THREE.BufferAttribute(colors, 3));

        const particleMat = new THREE.PointsMaterial({
            size: 0.35,
            vertexColors: true,
            transparent: true,
            opacity: 0.7
        });

        const particleSystem = new THREE.Points(particleGeom, particleMat);
        scene.add(particleSystem);

        // --- 5. INTERACTION & ANIMATION ---
        let mouseX = 0, mouseY = 0;
        let targetCamX = 2;

        window.addEventListener('mousemove', (e) => {
            mouseX = (e.clientX / window.innerWidth - 0.5) * 2;
            mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
        });

        const clock = new THREE.Clock();

        function animate() {
            requestAnimationFrame(animate);
            const time = clock.getElapsedTime();

            // Cockatiel Gentle Floating
            cockatielGroup.position.y = Math.sin(time * 2) * 0.3;
            cockatielGroup.rotation.y = Math.sin(time * 0.8) * 0.15;

            // Head Looking at Mouse
            headGroup.rotation.y = mouseX * 0.6;
            headGroup.rotation.x = -mouseY * 0.4;

            // Wing Flapping
            leftWingPivot.rotation.z = Math.sin(time * 3) * 0.15;
            rightWingPivot.rotation.z = -Math.sin(time * 3) * 0.15;

            // Particles Floating
            particleSystem.rotation.y = time * 0.05;

            // Smooth Camera Parallax
            camera.position.x += (targetCamX + mouseX * 1.5 - camera.position.x) * 0.05;
            camera.position.y += (-mouseY * 1.5 + 1 - camera.position.y) * 0.05;
            camera.lookAt(1, 0, 0);

            renderer.render(scene, camera);
        }

        animate();

        // --- 6. TAB SWITCHING LOGIC & CAMERA POSITIONING ---
        function switchTab(tabId, btn) {
            document.querySelectorAll('.tab-pane').forEach(pane => pane.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));

            document.getElementById(tabId).classList.add('active');
            btn.classList.add('active');

            // Shift 3D Camera Focus depending on tab
            if (tabId === 'about') {
                targetCamX = 2; // Focus on Cockatiel
            } else {
                targetCamX = 6; // Move Cockatiel to side to showcase artwork
            }
        }

        // --- 7. RESPONSIVE LISTENER ---
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
