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

    <!-- FontAwesome Icons for Social Media -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        :root {
            --pastel-blue: #a8d8ea;
            --pastel-pink: #faccff;
            --pastel-soft-pink: #ffd3e2;
            --pastel-purple: #e0c3fc;
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
            pointer-events: none; /* เพื่อให้สามารถลากเมาส์หมุนฉาก 3D ในส่วนว่างได้ */
        }

        /* Card นำเสนอผลงาน (Glassmorphism) */
        .portfolio-card {
            pointer-events: auto;
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 2px solid var(--glass-border);
            border-radius: 32px;
            padding: 40px 35px;
            max-width: 460px;
            width: 100%;
            text-align: center;
            box-shadow: 0 20px 40px var(--shadow), 0 10px 20px rgba(255, 204, 255, 0.3);
            animation: floatUp 1.2s ease-out;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .portfolio-card:hover {
            transform: translateY(-6px);
            box-shadow: 0 25px 50px rgba(250, 204, 255, 0.6);
        }

        /* ไอคอนโปรไฟล์ */
        .profile-avatar {
            width: 100px;
            height: 100px;
            margin: 0 auto 20px;
            border-radius: 50%;
            background: linear-gradient(135deg, #a8d8ea, #ffd3e2);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 2.8rem;
            border: 4px solid #ffffff;
            box-shadow: 0 8px 20px rgba(168, 216, 234, 0.5);
        }

        h1 {
            font-size: 1.8rem;
            font-weight: 700;
            color: #3d3b62;
            margin-bottom: 4px;
        }

        .pen-name {
            font-size: 1.15rem;
            font-weight: 600;
            background: linear-gradient(45deg, #7b2cbf, #ff85a1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 12px;
            letter-spacing: 0.5px;
        }

        .tag {
            display: inline-block;
            background: rgba(255, 255, 255, 0.85);
            padding: 6px 18px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            color: var(--text-sub);
            margin-bottom: 18px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.03);
            border: 1px solid rgba(168, 216, 234, 0.3);
        }

        .bio {
            font-size: 0.95rem;
            line-height: 1.6;
            color: #5c5d75;
            margin-bottom: 28px;
        }

        /* Contact Link Buttons */
        .social-title {
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: #8e8aa8;
            margin-bottom: 15px;
            font-weight: 600;
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
            padding: 12px 16px;
            border-radius: 16px;
            background: #ffffff;
            color: #4a4e69;
            text-decoration: none;
            font-size: 0.9rem;
            font-weight: 500;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.8);
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .social-btn i {
            font-size: 1.2rem;
            transition: transform 0.3s ease;
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

        /* Keyframes Animation */
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

        /* Mobile Responsive */
        @media (max-width: 480px) {
            .portfolio-card {
                padding: 30px 20px;
            }
            .social-grid {
                grid-template-columns: 1fr;
            }
            h1 {
                font-size: 1.5rem;
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
            
            <!-- Avatar Icon -->
            <div class="profile-avatar">
                <i class="fa-solid fa-palette"></i>
            </div>

            <!-- Profile Info -->
            <h1>Jaranya Tosanhuan</h1>
            <div class="pen-name">Glacier Kumu</div>
            <div class="tag">Artist & Creator</div>

            <p class="bio">
                ✨ I love Character Design and Concept art. <br>
                สร้างสรรค์ผลงานตัวละครและภาพคอนเซปต์ในโลกจินตนาการ
            </p>

            <!-- Social Contacts Grid -->
            <div class="social-title">Connect with me</div>
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

    <!-- 3D Scene Script -->
    <script>
        // 1. Setup Scene, Camera, Renderer
        const container = document.getElementById('canvas-container');
        const scene = new THREE.Scene();

        // เพิ่ม Fog แบบจางๆ ในฉากหลังให้รู้สึกนุ่มนวล
        scene.fog = new THREE.FogExp2(0xeaf6ff, 0.012);

        const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 30;

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        container.appendChild(renderer.domElement);

        // 2. Lighting (จัดแสงโทนพาสเทล ฟ้า-ชมพู)
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
        scene.add(ambientLight);

        // แสงสีฟ้าพาสเทลจากมุมซ้ายบน
        const blueLight = new THREE.DirectionalLight(0xa8d8ea, 1.2);
        blueLight.position.set(-10, 15, 10);
        scene.add(blueLight);

        // แสงสีชมพูพาสเทลจากมุมขวาล่าง
        const pinkLight = new THREE.DirectionalLight(0xfaccff, 1.2);
        pinkLight.position.set(10, -15, 10);
        scene.add(pinkLight);

        // 3. Create 3D Floating Objects
        const shapes = [];
        const pastelColors = [0xa8d8ea, 0xfaccff, 0xffd3e2, 0xe0c3fc, 0xc4faf8];

        // รูปทรงเรขาคณิตแบบผสมผสาน
        const geometries = [
            new THREE.IcosahedronGeometry(1.2, 0),
            new THREE.TorusGeometry(1, 0.4, 16, 50),
            new THREE.OctahedronGeometry(1.3, 0),
            new THREE.SphereGeometry(1, 32, 32),
            new THREE.DodecahedronGeometry(1.1, 0)
        ];

        // สร้างวัตถุ 3D จำนวน 45 ชิ้นกระจายทั่วหน้าจอ
        for (let i = 0; i < 45; i++) {
            const geom = geometries[Math.floor(Math.random() * geometries.length)];
            const color = pastelColors[Math.floor(Math.random() * pastelColors.length)];

            const material = new THREE.MeshPhongMaterial({
                color: color,
                shininess: 80,
                flatShading: true,
                transparent: true,
                opacity: 0.85
            });

            const mesh = new THREE.Mesh(geom, material);

            // สุ่มตำแหน่ง X, Y, Z
            mesh.position.x = (Math.random() - 0.5) * 55;
            mesh.position.y = (Math.random() - 0.5) * 55;
            mesh.position.z = (Math.random() - 0.5) * 35 - 5;

            // สุ่มมุมหมุนเริ่มต้น
            mesh.rotation.x = Math.random() * Math.PI;
            mesh.rotation.y = Math.random() * Math.PI;

            // เก็บค่าความเร็วสำหรับการเคลื่อนไหว
            mesh.userData = {
                rotSpeedX: (Math.random() - 0.5) * 0.012,
                rotSpeedY: (Math.random() - 0.5) * 0.012,
                floatSpeed: Math.random() * 0.015 + 0.005,
                floatOffset: Math.random() * Math.PI * 2
            };

            scene.add(mesh);
            shapes.push(mesh);
        }

        // 4. Interactive Mouse Parallax Effect
        let mouseX = 0;
        let mouseY = 0;

        window.addEventListener('mousemove', (e) => {
            mouseX = (e.clientX / window.innerWidth - 0.5) * 2;
            mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
        });

        // 5. Animation Loop
        const clock = new THREE.Clock();

        function animate() {
            requestAnimationFrame(animate);
            const elapsedTime = clock.getElapsedTime();

            // หมุนและลอยวัตถุแต่ละชิ้น
            shapes.forEach(shape => {
                shape.rotation.x += shape.userData.rotSpeedX;
                shape.rotation.y += shape.userData.rotSpeedY;

                // ให้ลอยขึ้น-ลงนุ่มนวลแบบ Sine Wave
                shape.position.y += Math.sin(elapsedTime * 1.5 + shape.userData.floatOffset) * 0.015;
            });

            // ปรับมุมกล้องให้ขยับตามเมาส์นุ่มนวล (Parallax)
            camera.position.x += (mouseX * 4 - camera.position.x) * 0.04;
            camera.position.y += (-mouseY * 4 - camera.position.y) * 0.04;
            camera.lookAt(scene.position);

            renderer.render(scene, camera);
        }

        animate();

        // 6. Handle Window Resize
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
