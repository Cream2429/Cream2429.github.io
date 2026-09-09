<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom AR Marker with 3D Animation</title>
    
    <!-- A-Frame Library -->
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    
    <!-- AR.js for A-Frame -->
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
    
    <!-- A-Frame Extras (ใช้สำหรับจัดการ Animation ของ GLTF/GLB) -->
    <script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@v6.1.1/dist/aframe-extras.min.js"></script>
</head>
<body style="margin: 0px; overflow: hidden;">

    <a-scene embedded arjs="sourceType: webcam; debugUIEnabled: false;">
        
        <!-- โหลดไฟล์ 3D Model -->
        <a-assets>
            <a-asset-item 
                id="epona-model" 
                src="https://sibsansuk.github.io/epona.glb">
            </a-asset-item>
        </a-assets>

        <!-- 
            Custom Pattern Marker
            หมายเหตุ: AR.js ต้องใช้ไฟล์รหัสรูปแบบ .patt ในการตรวจจับรูปภาพ tracker.png 
            หากคุณสร้างไฟล์ tracker.patt แล้ว สามารถอัปโหลดไว้ที่ path เดียวกันและอ้างอิง URL ได้ทันที
        -->
        <a-marker type="pattern" url="https://aitutotialcourse.github.io/tracker.patt">
            
            <!-- 
                แสดง 3D Model epona.glb
                - animation-mixer: เล่นทุก Animation ที่อยู่ในโมเดลแบบวนซ้ำ (loop)
                - scale / position / rotation: ปรับขนาดและตำแหน่งให้เหมาะสม
            -->
            <a-entity 
                gltf-model="#epona-model" 
                animation-mixer="clip: *; loop: repeat;" 
                scale="0.5 0.5 0.5" 
                position="0 0 0" 
                rotation="0 0 0">
            </a-entity>

        </a-marker>

        <!-- กล้องสำหรับ AR -->
        <a-entity camera></a-entity>

    </a-scene>

</body>
</html>
