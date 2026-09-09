# Jaranya Tosanguan
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glacier Kumu - 3D Portfolio</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,600;0,800;1,400&family=Prompt:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --bg-color: #0b0f19;
            --accent-cyan: #64ffda;
            --accent-blue: #7928ca;
            --accent-pink: #ff0080;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --glass-bg: rgba(15, 23, 42, 0.65);
            --glass-border: rgba(255, 255, 255, 0.1);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Plus Jakarta Sans', 'Prompt', sans-serif;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: var(--bg-color);
            color: var(--text-main);
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
            font-size: 1.6rem;
            font-weight: 800;
            letter-spacing: 1.5px;
            background: linear-gradient(135deg, var(--accent-cyan), #38bdf8);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-transform: uppercase;
        }

        .real-name {
            font-size: 0.85rem;
            color: var(--text-muted);
            letter-spacing: 1px;
            font-weight: 400;
        }

        nav {
            display: flex;
            gap: 2rem;
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            padding: 0.75rem 2rem;
            border-radius: 40px;
            border: 1px solid var(--glass-border);
        }

        nav a {
            color: var(--text-main);
            text-decoration: none;
            font-size: 0.95rem;
            font-weight: 500;
            transition: all 0.3s ease;
        }

        nav a:hover {
            color: var(--accent-cyan);
            text-shadow: 0 0 10px rgba(100, 255, 218, 0.5);
        }

        /* Hero Content */
        .hero-container {
            max-width: 620px;
            margin-top: auto;
            margin-bottom: auto;
        }

        .badge {
            display: inline-block;
            padding: 0.35rem 1rem;
            background: rgba(100, 255, 218, 0.1);
            border: 1px solid rgba(100, 255, 218, 0.3);
            color: var(--accent-cyan);
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            letter-spacing: 1px;
            margin-bottom: 1.25rem;
            text-transform: uppercase;
        }

        .hero-title {
            font-size: 3.8rem;
            line-height: 1.1;
            font-weight: 800;
            margin-bottom: 1.25rem;
        }

        .hero-title .gradient-text {
            background: linear-gradient(135deg, #ffffff 30%, var(--accent-cyan));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-subtitle {
            font-size: 1.2rem;
            line-height: 1.7;
            color: var(--text-muted);
            margin-bottom: 2.25rem;
            font-weight: 300;
        }

        .cta-group {
            display: flex;
            gap: 1.25rem;
            align-items: center;
        }

        .btn-primary {
            padding: 0.9rem 2.25rem;
            background: linear-gradient(135deg, var(--accent-cyan), #0284c7);
            color: #042f2e;
            border: none;
            border-radius: 30px;
            font-size: 0.95rem;
            font-weight: 700;
            text-decoration: none;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 10px 25px -5px rgba(100, 255, 218, 0.3);
            cursor: pointer;
        }

        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 30px -5px rgba(100, 255, 218, 0.5);
        }

        .btn-secondary {
            padding: 0.9rem 2.25rem;
            background: var(--glass-bg);
            color: var(--text-main);
            border: 1px solid var(--glass-border);
            border-radius: 30px;
            font-size: 0.95rem;
            font-weight: 600;
            text-decoration: none;
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
        }

        .btn-secondary:hover {
            border-color: var(--accent-cyan);
            color: var(--accent-cyan);
            background: rgba(15, 23, 42, 0.85);
        }

        /* Interactive Showcase Cards Overlay */
        .art-cards {
            display: flex;
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .card {
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            backdrop-filter: blur(10px);
            padding: 1rem 1.5rem;
            border-radius: 16px;
            width: 180px;
            transition: all 0.3s ease;
        }

        .card:hover {
            border-color: var(--accent-cyan);
            transform: translateY(-5px);
        }

        .card h4 {
            font-size: 0.9rem;
            color: var(--text-main);
            margin-bottom: 0.25rem;
        }

        .card p {
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        /* Footer Controls Info */
        footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: var(--text-muted);
            font-size: 0.85rem;
        }

        .controls-hint {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            background: var(--glass-bg);
            padding: 0.5rem 1.25rem;
            border-radius: 20px;
            border: 1px solid var(--glass-border);
        }

        .hint-dot {
            width: 8px;
            height: 8px;
            background-color: var(--accent-cyan);
            border-radius: 50%;
            box-shadow: 0 0 10px var(--accent-cyan);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 0.4; }
            50% { opacity: 1; }
            100% { opacity: 0.4; }
        }

        /* Responsive */
        @media (max-width: 1024px) {
            .ui-layer { padding: 2rem; }
            .hero-title { font-size: 3rem; }
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
                <a href="#about">About</a>
                <a href="#character-design">Character Design</a>
                <a href="#concept-art">Concept Art</a>
                <a href="#contact">Contact</a>
            </nav>
        </header>

        <!-- Hero Section -->
        <section class="hero-container">
            <div class="badge">Portfolio & Art Gallery</div>
            <h1 class="hero-title">Bringing Worlds & <span class="gradient-text">Characters</span> to Life</h1>
            <p class="hero-subtitle">
                สวัสดีครับ/ค่ะ! ฉันคือ <strong>Jaranya Tosanhuan</strong> (นามปากกา <strong>Glacier Kumu</strong>)<br>
                หลงใหลการสร้างสรรค์ <em>Character Design</em> และ <em>Concept Art</em> ถ่ายทอดเรื่องราวผ่านจินตนาการและงานดีไซน์ที่เป็นเอกลักษณ์
            </p>
            <div class="cta-group interactive">
                <a href="#character-design" class="btn-primary">Explore Characters</a>
                <a href="#contact" class="btn-secondary">Get in Touch</a>
            </div>

            <!-- Mini Concept Feature Cards -->
            <div class="art-cards interactive">
                <div class="card">
                    <h4>Character Art</h4>
                    <p>Expressive & Unique Designs</p>
                </div>
                <div class="card">
                    <h4>Concept Art</h4>
                    <p>World Building & Atmosphere</p>
                </div>
            </div>
        </section>

        <!-- Footer -->
        <footer>
            <div class="controls-hint">
                <span class="hint-dot"></span>
                <span>Drag to Orbit • Scroll to Zoom 3D Character Crystal</span>
            </div>
            <div>© 2026 Glacier Kumu. All rights reserved.</div>
        </footer>
    </div>

    <!-- Three.js Library Suite -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tween.js/18.6.4/tween.umd.js"></script>

    <script>
        // --- 1. Scene & Renderer Setup ---
        const container = document.getElementById('webgl-container');
        
        const scene = new THREE.Scene();
        scene.fog = new THREE.FogExp2(0x0b0f19, 0.035);

        const camera = new THREE.PerspectiveCamera(
            60, 
            window.innerWidth / window.innerHeight, 
            0.1, 
            1000
        );
        camera.position.set(2.5, 1.2, 5.5);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 1.2;
        container.appendChild(renderer.domElement);

        // --- 2. Orbit Controls ---
        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.maxDistance = 12;
        controls.minDistance = 2.5;
        controls.enablePan = false;

        // --- 3. Lighting Setup ---
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
        scene.add(ambientLight);

        // Key Light (Cyan)
        const keyLight = new THREE.DirectionalLight(0x64ffda, 2.5);
        keyLight.position.set(5, 5, 4);
        scene.add(keyLight);

        // Fill Light (Pink/Purple)
        const fillLight = new THREE.PointLight(0xff0080, 3, 15);
        fillLight.position.set(-4, -2, -3);
        scene.add(fillLight);

        // Blue Rim Light
        const rimLight = new THREE.PointLight(0x38bdf8, 2.5, 12);
        rimLight.position.set(0, 4, -4);
        scene.add(rimLight);

        // --- 4. Central 3D Sculpture (Artistic Character Crystal Gem) ---
        const coreGroup = new THREE.Group();

        // Main Crystal Geometry
        const crystalGeo = new THREE.OctahedronGeometry(1.4, 2);
        const crystalMat = new THREE.MeshPhysicalMaterial({
            color: 0x0f172a,
            emissive: 0x1e293b,
            roughness: 0.1,
            metalness: 0.9,
            clearcoat: 1.0,
            clearcoatRoughness: 0.1,
            reflectivity: 0.9,
            wireframe: false
        });
        const crystal = new THREE.Mesh(crystalGeo, crystalMat);
        coreGroup.add(crystal);

        // Floating Metallic Orbit Rings
        const ringGeo1 = new THREE.TorusGeometry(2.1, 0.025, 16, 100);
        const ringMat1 = new THREE.MeshStandardMaterial({ 
            color: 0x64ffda, 
            metalness: 0.9, 
            roughness: 0.1,
            emissive: 0x14b8a6,
            emissiveIntensity: 0.3
        });
        const ring1 = new THREE.Mesh(ringGeo1, ringMat1);
        ring1.rotation.x = Math.PI / 3;
        coreGroup.add(ring1);

        const ringGeo2 = new THREE.TorusGeometry(2.5, 0.02, 16, 100);
        const ringMat2 = new THREE.MeshStandardMaterial({ 
            color: 0xff0080, 
            metalness: 0.8, 
            roughness: 0.2,
            emissive: 0xbe185d,
            emissiveIntensity: 0.4
        });
        const ring2 = new THREE.Mesh(ringGeo2, ringMat2);
        ring2.rotation.y = Math.PI / 4;
        coreGroup.add(ring2);

        // Floating Abstract Creative Orbs around the Crystal
        const orbCount = 12;
        const orbGroup = new THREE.Group();
        const orbGeo = new THREE.IcosahedronGeometry(0.12, 1);
        const orbMat = new THREE.MeshStandardMaterial({
            color: 0x64ffda,
            roughness: 0.2,
            metalness: 0.8,
            emissive: 0x64ffda,
            emissiveIntensity: 0.5
        });

        for (let i = 0; i < orbCount; i++) {
            const orb = new THREE.Mesh(orbGeo, orbMat);
            const angle = (i / orbCount) * Math.PI * 2;
            const radius = 2.8 + Math.random() * 0.5;
            orb.position.set(
                Math.cos(angle) * radius,
                (Math.random() - 0.5) * 1.8,
                Math.sin(angle) * radius
            );
            orbGroup.add(orb);
        }
        coreGroup.add(orbGroup);

        // Position core group slightly to the right for UI balance
        coreGroup.position.set(1.2, 0, 0);
        scene.add(coreGroup);

        // --- 5. Atmospheric Background Particles (Magical Concept Sparkles) ---
        const particleCount = 1000;
        const particleGeo = new THREE.BufferGeometry();
        const positions = new Float32Array(particleCount * 3);
        const scales = new Float32Array(particleCount);

        for (let i = 0; i < particleCount * 3; i += 3) {
            positions[i] = (Math.random() - 0.5) * 25;
            positions[i + 1] = (Math.random() - 0.5) * 25;
            positions[i + 2] = (Math.random() - 0.5) * 25;
            scales[i / 3] = Math.random();
        }

        particleGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));

        const particleMat = new THREE.PointsMaterial({
            size: 0.04,
            color: 0x64ffda,
            transparent: true,
            opacity: 0.6,
            blending: THREE.AdditiveBlending
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
            crystal.rotation.y = elapsedTime * 0.25;
            crystal.rotation.x = Math.sin(elapsedTime * 0.3) * 0.2;

            ring1.rotation.z = elapsedTime * 0.15;
            ring1.rotation.x = Math.cos(elapsedTime * 0.2) * 0.5;

            ring2.rotation.y = -elapsedTime * 0.2;
            ring2.rotation.z = Math.sin(elapsedTime * 0.25) * 0.4;

            orbGroup.rotation.y = -elapsedTime * 0.1;

            // Floating bobbing motion
            coreGroup.position.y = Math.sin(elapsedTime * 0.8) * 0.15 + (targetY * 0.1);
            coreGroup.position.x = 1.2 + (targetX * 0.1);

            // Particles slow rotation
            particleSystem.rotation.y = elapsedTime * 0.02;

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
                coreGroup.position.set(0, 0.8, 0);
            } else {
                coreGroup.position.set(1.2, 0, 0);
            }

            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        });

        // Initial responsive check
        if (window.innerWidth <= 768) {
            coreGroup.position.set(0, 0.8, 0);
        }
    </script>
</body>
</html>
