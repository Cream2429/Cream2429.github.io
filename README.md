# Jaranya Tosanguan
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glacier Kumu - 3D Portfolio</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@300;400;500;600;700&family=Mitr:wght@300;400;500;600&display=swap" rel="stylesheet">
    
    <style>
        :root {
            /* Pastel Color Palette */
            --bg-pastel: #f3f8fe;
            --pastel-blue-light: #e0f2fe;
            --pastel-blue: #a5f3fc;
            --pastel-blue-deep: #38bdf8;
            --pastel-pink-light: #fce7f3;
            --pastel-pink: #fbcfe8;
            --pastel-pink-deep: #f472b6;
            --text-dark: #334155;
            --text-muted: #64748b;
            --glass-bg: rgba(255, 255, 255, 0.65);
            --glass-border: rgba(255, 255, 255, 0.8);
            --shadow-soft: 0 20px 40px -15px rgba(244, 114, 182, 0.25);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Fredoka', 'Mitr', sans-serif;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background: linear-gradient(135deg, #f0f7ff 0%, #fff0f6 100%);
            color: var(--text-dark);
        }

        /* 3D Canvas Layer */
        #webgl-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* Interactive UI Layer Overlay */
        .ui-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 2;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 2.5rem 4rem;
        }

        .interactive {
            pointer-events: auto;
        }

        /* Navigation Header */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .brand {
            display: flex;
            flex-direction: column;
        }

        .pen-name {
            font-size: 1.8rem;
            font-weight: 700;
            letter-spacing: 1px;
            background: linear-gradient(135deg, #0284c7, #db2777);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-transform: capitalize;
        }

        .real-name {
            font-size: 0.9rem;
            color: var(--text-muted);
            font-weight: 400;
        }

        nav {
            display: flex;
            gap: 1.8rem;
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            padding: 0.75rem 2rem;
            border-radius: 40px;
            border: 1px solid var(--glass-border);
            box-shadow: 0 10px 30px rgba(0,0,0,0.03);
        }

        nav a {
            color: var(--text-dark);
            text-decoration: none;
            font-size: 1rem;
            font-weight: 500;
            transition: all 0.3s ease;
        }

        nav a:hover {
            color: var(--pastel-pink-deep);
            transform: translateY(-2px);
        }

        /* Hero Content */
        .hero-container {
            max-width: 580px;
            margin-top: auto;
            margin-bottom: auto;
        }

        .badge {
            display: inline-block;
            padding: 0.4rem 1.2rem;
            background: rgba(255, 255, 255, 0.8);
            border: 1.5px solid var(--pastel-pink);
            color: #db2777;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            letter-spacing: 0.5px;
            margin-bottom: 1.25rem;
            box-shadow: 0 4px 15px rgba(251, 207, 232, 0.5);
        }

        .hero-title {
            font-size: 3.5rem;
            line-height: 1.15;
            font-weight: 700;
            margin-bottom: 1.25rem;
            color: var(--text-dark);
        }

        .hero-title .pink-text {
            color: #f472b6;
            background: linear-gradient(135deg, #f472b6, #fb7185);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-title .blue-text {
            color: #38bdf8;
            background: linear-gradient(135deg, #38bdf8, #818cf8);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-subtitle {
            font-size: 1.15rem;
            line-height: 1.7;
            color: var(--text-muted);
            margin-bottom: 2.25rem;
            font-weight: 400;
        }

        .cta-group {
            display: flex;
            gap: 1.25rem;
            align-items: center;
        }

        .btn-primary {
            padding: 0.95rem 2.4rem;
            background: linear-gradient(135deg, #38bdf8, #f472b6);
            color: #ffffff;
            border: none;
            border-radius: 30px;
            font-size: 1rem;
            font-weight: 600;
            text-decoration: none;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 10px 25px -5px rgba(244, 114, 182, 0.4);
            cursor: pointer;
        }

        .btn-primary:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 15px 30px -5px rgba(56, 189, 248, 0.5);
        }

        .btn-secondary {
            padding: 0.95rem 2.4rem;
            background: var(--glass-bg);
            color: var(--text-dark);
            border: 1.5px solid var(--glass-border);
            border-radius: 30px;
            font-size: 1rem;
            font-weight: 600;
            text-decoration: none;
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.03);
        }

        .btn-secondary:hover {
            border-color: var(--pastel-blue-deep);
            color: #0284c7;
            background: #ffffff;
            transform: translateY(-3px);
        }

        /* Showcase Cards Overlay */
        .art-cards {
            display: flex;
            gap: 1.25rem;
            margin-top: 2rem;
        }

        .card {
            background: var(--glass-bg);
            border: 1.5px solid var(--glass-border);
            backdrop-filter: blur(12px);
            padding: 1rem 1.4rem;
            border-radius: 20px;
            width: 190px;
            transition: all 0.3s ease;
            box-shadow: 0 10px 25px rgba(0,0,0,0.03);
        }

        .card:hover {
            border-color: var(--pastel-pink);
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(244, 114, 182, 0.15);
        }

        .card h4 {
            font-size: 0.95rem;
            color: var(--text-dark);
            margin-bottom: 0.25rem;
        }

        .card p {
            font-size: 0.8rem;
            color: var(--text-muted);
        }

        /* Footer Controls Info */
        footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        .controls-hint {
            display: flex;
            align-items: center;
            gap: 0.6rem;
            background: var(--glass-bg);
            padding: 0.6rem 1.4rem;
            border-radius: 20px;
            border: 1.5px solid var(--glass-border);
            box-shadow: 0 4px 15px rgba(0,0,0,0.03);
        }

        .hint-dot {
            width: 10px;
            height: 10px;
            background: linear-gradient(135deg, #38bdf8, #f472b6);
            border-radius: 50%;
            box-shadow: 0 0 10px rgba(244, 114, 182, 0.6);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(0.9); opacity: 0.6; }
            50% { transform: scale(1.2); opacity: 1; }
            100% { transform: scale(0.9); opacity: 0.6; }
        }

        /* Responsive */
        @media (max-width: 1024px) {
            .ui-layer { padding: 2rem; }
            .hero-title { font-size: 2.8rem; }
            .art-cards { display: none; }
        }

        @media (max-width: 768px) {
            nav { display: none; }
            .hero-title { font-size: 2.3rem; }
            .hero-subtitle { font-size: 1rem; }
            .cta-group { flex-direction: column; align-items: stretch; }
            .btn-primary, .btn-secondary { text-align: center; }
        }
    </style>
</head>
<body>

    <!-- 3D Canvas Rendering Layer -->
    <div id="webgl-container"></div>

    <!-- UI Overlay Layer -->
    <div class="ui-layer">
        <!-- Header -->
        <header class="interactive">
            <div class="brand">
                <span class="pen-name">Glacier Kumu</span>
                <span class="real-name">Jaranya Tosanhuan</span>
            </div>
            <nav>
                <a href="#about">เกี่ยวกับ</a>
                <a href="#character-design">Character Design</a>
                <a href="#concept-art">Concept Art</a>
                <a href="#contact">ติดต่อ</a>
            </nav>
        </header>

        <!-- Hero Section -->
        <section class="hero-container">
            <div class="badge">🎨 Pastel Concept & Character Gallery</div>
            <h1 class="hero-title">Crafting <span class="pink-text">Characters</span> & Dreamy <span class="blue-text">Worlds</span></h1>
            <p class="hero-subtitle">
                สวัสดีครับ/ค่ะ! ฉันคือ <strong>Jaranya Tosanhuan</strong> (นามปากกา <strong>Glacier Kumu</strong>)<br>
                ผู้หลงใหลในการสร้างสรรค์ <em>Character Design</em> และ <em>Concept Art</em> ถ่ายทอดเรื่องราวผ่านงานอาร์ตโทนพาสเทลและบรรยากาศอันนุ่มนวล
            </p>
            <div class="cta-group interactive">
                <a href="#character-design" class="btn-primary">Explore Portfolio</a>
                <a href="#contact" class="btn-secondary">Contact Me</a>
            </div>

            <!-- Mini Concept Feature Cards -->
            <div class="art-cards interactive">
                <div class="card">
                    <h4>✨ Character Design</h4>
                    <p>Expressive & Cute Pastel Aesthetics</p>
                </div>
                <div class="card">
                    <h4>🌌 Concept Art</h4>
                    <p>Atmospheric & Dreamy World Building</p>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer>
            <div class="controls-hint">
                <span class="hint-dot"></span>
                <span>Drag to Orbit • Scroll to Zoom 3D Pastel Heart Crystal</span>
            </div>
            <div>© 2026 Glacier Kumu. All rights reserved.</div>
        </footer>
    </div>

    <!-- Three.js Library Suite -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>

    <script>
        // --- 1. Scene & Renderer Setup ---
        const container = document.getElementById('webgl-container');
        
        const scene = new THREE.Scene();
        scene.fog = new THREE.FogExp2(0xf0f7ff, 0.035);

        const camera = new THREE.PerspectiveCamera(
            60, 
            window.innerWidth / window.innerHeight, 
            0.1, 
            1000
        );
        camera.position.set(2.2, 1.0, 5.5);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 1.1;
        container.appendChild(renderer.domElement);

        // --- 2. Orbit Controls ---
        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.maxDistance = 12;
        controls.minDistance = 2.5;
        controls.enablePan = false;

        // --- 3. Lighting Setup (Soft Pastel Lighting) ---
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.9);
        scene.add(ambientLight);

        // Pastel Blue Key Light
        const blueLight = new THREE.DirectionalLight(0x7dd3fc, 1.8);
        blueLight.position.set(5, 5, 4);
        scene.add(blueLight);

        // Pastel Pink Fill Light
        const pinkLight = new THREE.PointLight(0xf472b6, 2.2, 15);
        pinkLight.position.set(-4, -2, 3);
        scene.add(pinkLight);

        // Warm Soft Highlight
        const topLight = new THREE.PointLight(0xfef08a, 1.5, 12);
        topLight.position.set(0, 4, -2);
        scene.add(topLight);

        // --- 4. Central 3D Pastel Sculpture (Stylized Heart Crystal & Rings) ---
        const coreGroup = new THREE.Group();

        // Pastel Main Geometry (Icosahedron / Crystal)
        const crystalGeo = new THREE.IcosahedronGeometry(1.4, 0);
        const crystalMat = new THREE.MeshPhysicalMaterial({
            color: 0xffffff,
            emissive: 0xfbcfe8,
            emissiveIntensity: 0.2,
            roughness: 0.15,
            metalness: 0.1,
            transmission: 0.6, // Glasslike effect
            thickness: 0.8,
            clearcoat: 1.0,
            clearcoatRoughness: 0.1
        });
        const crystal = new THREE.Mesh(crystalGeo, crystalMat);
        coreGroup.add(crystal);

        // Soft Inner Core (Pastel Blue Glow)
        const innerGeo = new THREE.OctahedronGeometry(0.8, 0);
        const innerMat = new THREE.MeshStandardMaterial({
            color: 0x38bdf8,
            roughness: 0.3,
            metalness: 0.2,
            emissive: 0x38bdf8,
            emissiveIntensity: 0.5
        });
        const innerMesh = new THREE.Mesh(innerGeo, innerMat);
        coreGroup.add(innerMesh);

        // Floating Pastel Orbit Rings
        const ringGeo1 = new THREE.TorusGeometry(2.1, 0.03, 16, 100);
        const ringMat1 = new THREE.MeshStandardMaterial({ 
            color: 0x38bdf8, 
            metalness: 0.3, 
            roughness: 0.2,
            emissive: 0x7dd3fc,
            emissiveIntensity: 0.4
        });
        const ring1 = new THREE.Mesh(ringGeo1, ringMat1);
        ring1.rotation.x = Math.PI / 3;
        coreGroup.add(ring1);

        const ringGeo2 = new THREE.TorusGeometry(2.5, 0.025, 16, 100);
        const ringMat2 = new THREE.MeshStandardMaterial({ 
            color: 0xf472b6, 
            metalness: 0.3, 
            roughness: 0.2,
            emissive: 0xfbcfe8,
            emissiveIntensity: 0.4
        });
        const ring2 = new THREE.Mesh(ringGeo2, ringMat2);
        ring2.rotation.y = Math.PI / 4;
        coreGroup.add(ring2);

        // Floating Cute Pastel Orbs
        const orbCount = 14;
        const orbGroup = new THREE.Group();
        const orbGeo = new THREE.SphereGeometry(0.1, 16, 16);

        const orbMatPink = new THREE.MeshStandardMaterial({ color: 0xf472b6, roughness: 0.2, emissive: 0xf472b6, emissiveIntensity: 0.3 });
        const orbMatBlue = new THREE.MeshStandardMaterial({ color: 0x38bdf8, roughness: 0.2, emissive: 0x38bdf8, emissiveIntensity: 0.3 });

        for (let i = 0; i < orbCount; i++) {
            const mat = i % 2 === 0 ? orbMatPink : orbMatBlue;
            const orb = new THREE.Mesh(orbGeo, mat);
            const angle = (i / orbCount) * Math.PI * 2;
            const radius = 2.7 + (i % 3) * 0.2;
            orb.position.set(
                Math.cos(angle) * radius,
                (Math.random() - 0.5) * 1.6,
                Math.sin(angle) * radius
            );
            orbGroup.add(orb);
        }
        coreGroup.add(orbGroup);

        // Position core group to the right
        coreGroup.position.set(1.3, 0, 0);
        scene.add(coreGroup);

        // --- 5. Atmospheric Pastel Sparkles (Background Particles) ---
        const particleCount = 600;
        const particleGeo = new THREE.BufferGeometry();
        const positions = new Float32Array(particleCount * 3);
        const colors = new Float32Array(particleCount * 3);

        const colorBlue = new THREE.Color(0x38bdf8);
        const colorPink = new THREE.Color(0xf472b6);

        for (let i = 0; i < particleCount; i++) {
            positions[i * 3] = (Math.random() - 0.5) * 20;
            positions[i * 3 + 1] = (Math.random() - 0.5) * 20;
            positions[i * 3 + 2] = (Math.random() - 0.5) * 20;

            const mixedColor = Math.random() > 0.5 ? colorBlue : colorPink;
            colors[i * 3] = mixedColor.r;
            colors[i * 3 + 1] = mixedColor.g;
            colors[i * 3 + 2] = mixedColor.b;
        }

        particleGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
        particleGeo.setAttribute('color', new THREE.BufferAttribute(colors, 3));

        const particleMat = new THREE.PointsMaterial({
            size: 0.05,
            vertexColors: true,
            transparent: true,
            opacity: 0.7,
            blending: THREE.NormalBlending
        });

        const particleSystem = new THREE.Points(particleGeo, particleMat);
        scene.add(particleSystem);

        // --- 6. Mouse Interaction Parallax ---
        let mouseX = 0;
        let mouseY = 0;
        let targetX = 0;
        let targetY = 0;

        const windowHalfX = window.innerWidth / 2;
        const windowHalfY = window.innerHeight / 2;

        document.addEventListener('mousemove', (event) => {
            mouseX = (event.clientX - windowHalfX) / 100;
            mouseY = (event.clientY - windowHalfY) / 100;
        });

        // --- 7. Animation Loop ---
        const clock = new THREE.Clock();

        function animate() {
            requestAnimationFrame(animate);

            const elapsedTime = clock.getElapsedTime();

            // Smooth Mouse Follow
            targetX += (mouseX - targetX) * 0.05;
            targetY += (mouseY - targetY) * 0.05;

            // Core Rotations
            crystal.rotation.y = elapsedTime * 0.2;
            crystal.rotation.x = Math.sin(elapsedTime * 0.3) * 0.15;

            innerMesh.rotation.y = -elapsedTime * 0.4;
            innerMesh.rotation.z = elapsedTime * 0.2;

            ring1.rotation.z = elapsedTime * 0.12;
            ring1.rotation.x = Math.cos(elapsedTime * 0.2) * 0.4;

            ring2.rotation.y = -elapsedTime * 0.18;
            ring2.rotation.z = Math.sin(elapsedTime * 0.25) * 0.3;

            orbGroup.rotation.y = -elapsedTime * 0.08;

            // Floating bobbing motion
            coreGroup.position.y = Math.sin(elapsedTime * 0.8) * 0.12 + (targetY * 0.1);
            coreGroup.position.x = 1.3 + (targetX * 0.1);

            // Particles slow rotation
            particleSystem.rotation.y = elapsedTime * 0.015;

            controls.update();
            renderer.render(scene, camera);
        }

        animate();

        // --- 8. Window Resize Handling ---
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();

            // Reposition core group on mobile screens
            if (window.innerWidth <= 768) {
                coreGroup.position.set(0, 0.7, 0);
            } else {
                coreGroup.position.set(1.3, 0, 0);
            }

            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        });

        // Initial responsive check
        if (window.innerWidth <= 768) {
            coreGroup.position.set(0, 0.7, 0);
        }
    </script>
</body>
</html>
