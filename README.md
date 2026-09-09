<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AR.js 3D Model with Animation</title>
  
  <!-- โหลด A-Frame และ AR.js -->
  <script src="https://aframe.io/releases/1.3.0/aframe.min.js"></script>
  <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
  
  <!-- โหลด aframe-extras สำหรับเล่น Animation ของ GLB/GLTF -->
  <script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@v6.1.1/dist/aframe-extras.min.js"></script>
</head>

<body style="margin: 0; overflow: hidden;">
  <a-scene 
    embedded 
    arjs="sourceType: webcam; debugUIEnabled: false; detectionMode: mono_and_matrix; matrixCodeType: 3x3;"
    renderer="logarithmicDepthBuffer: true; colorManagement: true;">
    
    <!-- โหลด Asset โมเดล 3D -->
    <a-assets>
      <a-asset-item id="epona-model" src="https://sibsansuk.github.io/epona.glb"></a-asset-item>
    </a-assets>

    <!-- กำหนด Marker (ใช้ pattern-tracker.patt ที่แปลงมาจาก tracker.png) -->
    <a-marker type="pattern" url="pattern-tracker.patt">
      <!-- 
        แสดงโมเดล 3D
        - animation-mixer : คำสั่งเล่น animation ( clip: * หมายถึงเล่นทุก animation )
        - scale : ปรับขนาดโมเดลตามต้องการ
        - position และ rotation : ปรับตำแหน่งและมุมหมุน
      -->
      <a-entity 
        gltf-model="#epona-model"
        animation-mixer="clip: *;"
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
