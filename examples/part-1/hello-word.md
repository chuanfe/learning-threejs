## 准备工作，主要有5部分
* 1.创建场景 scene

    `
    let scene = new THREE.Scene();
    `
* 2.生成相机 camera

    `
    let camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    `
* 3.渲染器 WebGLRenderer

    `
    let renderer = new THREE.WebGLRenderer();
    `
* 4.生成物体
  * 设置物体的形状
  * 设置物体的材质
  * 把形状和材质结合起来

    `
    let cubeGeometry = new THREE.CubeGeometry(1, 1, 1);
    let cubeMaterial = new THREE.MeshBasicMaterial({
        color: 0x00ff00
    });
    let cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
    scene.add(cube);
    `
* 5.渲染器 渲染 场景 和 相机

    `
    renderer.render(scene, camera);
    `