<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glacier Kumu | 3D Cockatiel Portfolio</title>

    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600&family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

    <!-- FontAwesome Icons for Social Media -->
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
            --glass-bg: rgba(255, 255, 255, 0.55);
            --glass-border: rgba(255, 255, 255, 0.8);
            --shadow: rgba(168, 216, 234, 0.4);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Kanit', sans-serif;
        }

        body {
            overflow: hidden;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, var(--pastel-bg-1) 0%, var(--pastel-bg-2) 100%);
            color: var(--text-main);
        }

        /* Canvas พื้นหลัง 3D */
        #canvas-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* Overlay UI Container */
        .ui-wrapper {
            position: relative;
            z-index: 2;
            width: 100%;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            pointer-events: none;
        }

        /* Card นำเสนอผลงาน (Glassmorphism) */
        .portfolio-card {
            pointer-events: auto;
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 2px solid var(--glass-border);
            border-radius: 32px;
            padding: 35px 30px;
            max-width: 440px;
            width: 100%;
            text-align: center;
            box-shadow: 0 20px 40px var(--shadow), 0 10px 20px rgba(255, 204, 255, 0.3);
            animation: floatUp 1.2s ease-out;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .portfolio-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 25px 50px rgba(250, 204, 255, 0.6);
        }

        /* ไอคอนโปรไฟล์ */
        .profile-avatar {
            width: 90px;
            height: 90px;
            margin: 0 auto 15px;
            border-radius: 50%;
            background: linear-gradient(135deg, #a8d8ea, #ffd3e2);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 2.5rem;
            border: 4px solid #ffffff;
            box-shadow: 0 8px 20px rgba(168, 216, 234, 0.5);
        }

        h1 {
            font-size: 1.7rem;
            font-weight: 700;
            color: #3d3b62;
            margin-bottom: 2px;
        }

        .pen-name {
            font-size: 1.1rem;
            font-weight: 600;
            background: linear-gradient(45deg, #7b2cbf, #ff85a1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 10px;
            letter-spacing: 0.5px;
        }

        .tag {
            display: inline-block;
            background: rgba(255, 255, 255, 0.85);
            padding: 5px 16px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            color: var(--text-sub);
            margin-bottom: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.03);
            border: 1px solid rgba(168, 216, 234, 0.3);
        }

        .bio {
            font-size: 0.9rem;
            line-height: 1.5;
            color: #5c5d75;
            margin-bottom: 22px;
        }

        /* Contact Link Buttons */
        .social-title {
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: #8e8aa8;
            margin-bottom: 12px;
            font-weight: 600;
        }

        .social-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .social-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 10px 14px;
            border-radius: 14px;
            background: #ffffff;
            color: #4a4e69;
            text-decoration: none;
            font-size: 0.85rem;
            font-weight: 500;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.8);
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .social-btn i {
            font-size: 1.1rem;
        }

        /* Hover Colors แยกตาม Social Media */
        .social-btn.fb:hover {
            background: #1877f2;
            color: #ffffff;
            box-shadow: 0 8px 18px rgba(24, 119, 242, 0.35);
        }

        .social-btn.ig:hover {
            background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888);
            color: #ffffff;
            box-shadow: 0 8px 18px rgba(220, 39, 67, 0.35);
        }

        .social-btn.x:hover {
            background: #000000;
            color: #ffffff;
            box-shadow: 0 8px 18px rgba(0, 0, 0, 0.25);
        }

        .social-btn.tiktok:hover {
            background: #000000;
            color: #00f2fe;
            box-shadow: 0 8px 18px rgba(0, 242, 254, 0.35);
        }

        .social-btn:hover {
            transform: translateY(-3px) scale(1.03);
        }

        @keyframes floatUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @media (max-width: 480px) {
            .portfolio-card {
                padding: 25px 18px;
            }
            .social-grid {
                grid-template-columns: 1fr;
            }
            h1 {
                font-size: 1.4rem;
            }
        }
    </style>
</head>
<body>

    <!-- 3D Canvas Background Container -->
    <div id="canvas-container"></div>

    <!-- UI Overlay -->
    <div class="ui-wrapper">
        <div class="portfolio-card">
            
            <div class="profile-avatar">
                <i class="fa-solid fa-feather-pointed"></i>
            </div>

            <h1>Jaranya Tosanhuan</h1>
            <div class="pen-name">Glacier Kumu</div>
            <div class="tag">Character Design & Concept Art</div>

            <p class="bio">
                I love Character Design and Concept art. ✨<br>
                ยินดีต้อนรับสู่พื้นที่สร้างสรรค์ผลงานภาพวาดและตัวละคร
            </p>

            <div class="social-title">Contact & Follow Me</div>
            <div class="social-grid">
                <a href="https://www.facebook.com/glacier.kumu/" target="_blank" rel="noopener noreferrer" class="social-btn fb">
                    <i class="fa-brands fa-facebook"></i>
                    <span>Facebook</span>
                </a>

                <a href="https://www.instagram.com/kuximumu_" target="_blank" rel="noopener noreferrer" class="social-btn ig">
                    <i class="fa-brands fa-instagram"></i>
                    <span>Instagram</span>
                </a>

                <a href="https://x.com/kuximumu_" target="_blank" rel="noopener noreferrer" class="social-btn x">
                    <i class="fa-brands fa-x-twitter"></i>
                    <span>Twitter (X)</span>
                </a>

                <a href="https://www.tiktok.com/@kuximumu_" target="_blank" rel="noopener noreferrer" class="social-btn tiktok">
                    <i class="fa-brands fa-tiktok"></i>
                    <span>TikTok</span>
                </a>
            </div>

        </div>
    </div>

    <!-- Import Three.js Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <script>
        // 1. Setup Scene, Camera, Renderer
        const container = document.getElementById('canvas-container');
        const scene = new THREE.Scene();

        scene.fog = new THREE.FogExp2(0xeaf6ff, 0.012);

        const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(0, 2, 22);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        container.appendChild(renderer.domElement);

        // 2. Lighting (จัดแสงโทนพาสเทล ฟ้า-ชมพู)
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.85);
        scene.add(ambientLight);

        const blueLight = new THREE.DirectionalLight(0xa8d8ea, 1.2);
        blueLight.position.set(-10, 15, 10);
        scene.add(blueLight);

        const pinkLight = new THREE.DirectionalLight(0xfaccff, 1.2);
        pinkLight.position.set(10, -15, 10);
        scene.add(pinkLight);

        // 3. Create Pastel Cockatiel (สร้างโมเดลนกคอกคาเทล 3D)
        const cockatielGroup = new THREE.Group();

        // วัสดุโทนสีพาสเทล
        const bodyMat = new THREE.MeshPhongMaterial({ color: 0xfff5e1, flatShading: true }); // ตัวสีครีมพาสเทล
        const yellowMat = new THREE.MeshPhongMaterial({ color: 0xffea85, flatShading: true }); // หัว/หงอนสีเหลือง
        const cheekMat = new THREE.MeshPhongMaterial({ color: 0xffa3a5, flatShading: true }); // แก้มส้มพาสเทล
        const beakMat = new THREE.MeshPhongMaterial({ color: 0xe0c3fc, flatShading: true }); // ปากสีม่วงพาสเทลอ่อน
        const eyeMat = new THREE.MeshPhongMaterial({ color: 0x4a4e69 }); // ตา
        const wingMat = new THREE.MeshPhongMaterial({ color: 0xa8d8ea, flatShading: true }); // ปีกสีฟ้าพาสเทล
        const tailMat = new THREE.MeshPhongMaterial({ color: 0xfaccff, flatShading: true }); // หางสีชมพูพาสเทล

        // Body (ลำตัว)
        const bodyGeom = new THREE.SphereGeometry(2, 16, 16);
        bodyGeom.scale(1, 1.3, 0.9);
        const body = new THREE.Mesh(bodyGeom, bodyMat);
        cockatielGroup.add(body);

        // Head (หัว)
        const headGeom = new THREE.SphereGeometry(1.4, 16, 16);
        const head = new THREE.Mesh(headGeom, yellowMat);
        head.position.set(0, 2.2, 0.3);
        cockatielGroup.add(head);

        // Beak (ปาก)
        const beakGeom = new THREE.ConeGeometry(0.4, 0.8, 4);
        const beak = new THREE.Mesh(beakGeom, beakMat);
        beak.position.set(0, 2.0, 1.6);
        beak.rotation.x = Math.PI / 3;
        cockatielGroup.add(beak);

        // Crest / Feathers on Head (หงอนเอกลักษณ์ของคอกคาเทล)
        for (let i = 0; i < 3; i++) {
            const crestGeom = new THREE.ConeGeometry(0.18, 1.5 - i * 0.2, 4);
            const crest = new THREE.Mesh(crestGeom, yellowMat);
            crest.position.set(0, 3.5 + i * 0.2, 0.1 - i * 0.2);
            crest.rotation.x = -0.2 - i * 0.15;
            cockatielGroup.add(crest);
        }

        // Cheeks (แก้มส้มป๊อก)
        const cheekGeom = new THREE.CylinderGeometry(0.45, 0.45, 0.1, 12);
        const leftCheek = new THREE.Mesh(cheekGeom, cheekMat);
        leftCheek.position.set(-1.1, 2.0, 1.0);
        leftCheek.rotation.z = Math.PI / 2;
        leftCheek.rotation.y = -Math.PI / 6;

        const rightCheek = leftCheek.clone();
        rightCheek.position.set(1.1, 2.0, 1.0);
        rightCheek.rotation.y = Math.PI / 6;

        cockatielGroup.add(leftCheek);
        cockatielGroup.add(rightCheek);

        // Eyes (ดวงตา)
        const eyeGeom = new THREE.SphereGeometry(0.18, 8, 8);
        const leftEye = new THREE.Mesh(eyeGeom, eyeMat);
        leftEye.position.set(-0.85, 2.3, 1.2);

        const rightEye = leftEye.clone();
        rightEye.position.set(0.85, 2.3, 1.2);

        cockatielGroup.add(leftEye);
        cockatielGroup.add(rightEye);

        // Wings (ปีกซ้าย-ขวา)
        const wingGeom = new THREE.ConeGeometry(1.2, 3.5, 4);
        
        // Left Wing Pivot
        const leftWingPivot = new THREE.Group();
        leftWingPivot.position.set(-1.8, 0.8, 0);
        const leftWing = new THREE.Mesh(wingGeom, wingMat);
        leftWing.position.set(-0.3, -1.2, 0);
        leftWing.rotation.z = 0.3;
        leftWingPivot.add(leftWing);
        cockatielGroup.add(leftWingPivot);

        // Right Wing Pivot
        const rightWingPivot = new THREE.Group();
        rightWingPivot.position.set(1.8, 0.8, 0);
        const rightWing = new THREE.Mesh(wingGeom, wingMat);
        rightWing.position.set(0.3, -1.2, 0);
        rightWing.rotation.z = -0.3;
        rightWingPivot.add(rightWing);
        cockatielGroup.add(rightWingPivot);

        // Tail (หงอนหาง)
        const tailGeom = new THREE.BoxGeometry(0.8, 3.5, 0.1);
        const tail = new THREE.Mesh(tailGeom, tailMat);
        tail.position.set(0, -2.2, -1.0);
        tail.rotation.x = -Math.PI / 6;
        cockatielGroup.add(tail);

        // ปรับตำแหน่งนกให้ลอยเด่นในฉาก
        cockatielGroup.position.set(0, 0, -2);
        cockatielGroup.scale.set(1.6, 1.6, 1.6);
        scene.add(cockatielGroup);

        // 4. Floating Pastel Shapes ในฉากหลัง
        const backgroundShapes = [];
        const pastelColors = [0xa8d8ea, 0xfaccff, 0xffd3e2, 0xe0c3fc, 0xc4faf8];
        const geometries = [
            new THREE.IcosahedronGeometry(1, 0),
            new THREE.TorusGeometry(0.8, 0.3, 16, 30),
            new THREE.SphereGeometry(0.8, 16, 16)
        ];

        for (let i = 0; i < 35; i++) {
            const geom = geometries[Math.floor(Math.random() * geometries.length)];
            const col = pastelColors[Math.floor(Math.random() * pastelColors.length)];

            const mat = new THREE.MeshPhongMaterial({
                color: col,
                shininess: 60,
                flatShading: true,
                transparent: true,
                opacity: 0.7
            });

            const mesh = new THREE.Mesh(geom, mat);
            mesh.position.x = (Math.random() - 0.5) * 50;
            mesh.position.y = (Math.random() - 0.5) * 50;
            mesh.position.z = (Math.random() - 0.5) * 30 - 10;

            mesh.userData = {
                rotSpeedX: (Math.random() - 0.5) * 0.01,
                rotSpeedY: (Math.random() - 0.5) * 0.01,
                floatSpeed: Math.random() * 0.015 + 0.005,
                floatOffset: Math.random() * Math.PI * 2
            };

            scene.add(mesh);
            backgroundShapes.push(mesh);
        }

        // 5. Mouse Parallax Movement
        let mouseX = 0;
        let mouseY = 0;

        window.addEventListener('mousemove', (e) => {
            mouseX = (e.clientX / window.innerWidth - 0.5) * 2;
            mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
        });

        // 6. Animation Loop
        const clock = new THREE.Clock();

        function animate() {
            requestAnimationFrame(animate);
            const elapsedTime = clock.getElapsedTime();

            // Cockatiel Floating & Wing Flapping Animation
            cockatielGroup.position.y = Math.sin(elapsedTime * 2) * 0.5;
            cockatielGroup.rotation.y = Math.sin(elapsedTime * 0.8) * 0.3;

            // Wing Flapping (ขยับปีกนกนุ่มนวล)
            leftWingPivot.rotation.z = Math.sin(elapsedTime * 4) * 0.2;
            rightWingPivot.rotation.z = -Math.sin(elapsedTime * 4) * 0.2;

            // Background Floating Shapes Animation
            backgroundShapes.forEach(shape => {
                shape.rotation.x += shape.userData.rotSpeedX;
                shape.rotation.y += shape.userData.rotSpeedY;
                shape.position.y += Math.sin(elapsedTime * 1.5 + shape.userData.floatOffset) * 0.012;
            });

            // Parallax Camera Motion
            camera.position.x += (mouseX * 3 - camera.position.x) * 0.04;
            camera.position.y += (-mouseY * 3 + 2 - camera.position.y) * 0.04;
            camera.lookAt(0, 0, 0);

            renderer.render(scene, camera);
        }

        animate();

        // 7. Handle Window Resize
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
