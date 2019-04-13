# 基础部分 Base part
- [hello-world](https://github.com/chuanfe/learning-threejs/tree/master/examples/part-1)
## 必须元素
* 1.场景(Scene)：是物体、光源等元素的容器
* 2.相机（Camera）：控制视角的位置、范围以及视觉焦点的位置,一个3D环境中只能存在一个相机
* 3.光源（Light）：包括全局光、平行光、点光源
* 4.渲染器（Renderer）:指定渲染方式，如webGL\canvas2D\Css2D\Css3D等。

### 1.场景 Scene
   物体、光源、控制器的添加必须使用secen.add(object)添加到场景中才能渲染出来。  
  一个3D项目中可同时存在多个scene，通过切换render的scene来切换显示场景

    ```javascript
    let scene = new THREE.Scene();
    ```

### 2.相机 Camera
    相机根据投影方式分两种：正交投影相机和透视投影相机
> 正交投影相机:THREE.OrthographicCamera(left, right, top, bottom, near, far),大小不因远近而变化  
> 透视投影相机:THREE.PerspectiveCamera(fov, aspect, near, far)，遵循近大远小的空间规则

    ```javascript
    let camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    ```
### 3.光源 Light
> 全局光：THREE.AmbientLight，影响整个scene的光源，一般是为了弱化阴影或调整整体色调，可设置光照颜色，以颜色的明度确定光源亮度  
平行光：THREE.DirectionalLight，模拟类似太阳的光源，所有被照射的区域亮度是一致的，可设置光照颜色、光照方向（通过向量确定方向），以颜色的明度确定光源亮度  
点光源：THREE.PointLight：单点发光，照射所有方向，可设置光照强度，光照半径和光颜色

以下分别在 scene中添加了全局光，1个平行光及5个点光源，项目中平行光作为主要光源、点光源主要用于暗部补光。
```javascript
    var ambient = new THREE.AmbientLight( 0xcccccc );
    scene.add( ambient );
    var directionalLight = new THREE.DirectionalLight( 0xcccccc );
    directionalLight.position.set( -2, 9, 1).normalize();//设置平行光方向
    scene.add( directionalLight );
    var pointlight = new THREE.PointLight(0xffffff, 1, 3000); 
    pointlight.position.set(0, 0, 2000);//设置点光源位置
    scene.add( pointlight );
    var pointlight2 = new THREE.PointLight(0xffffff, 1, 2000);
    pointlight2.position.set(-1000, 1000, 1000);
    scene.add( pointlight2 );
    var pointlight_back = new THREE.PointLight(0xffffff, 1, 2000); 
    pointlight_back.position.set(0, 0, -2000);
    scene.add( pointlight_back );
    var pointlight_left = new THREE.PointLight(0xffffff, 1, 2000); 
    pointlight_left.position.set( -2000, 0,0);
    scene.add( pointlight_left );
    var pointlight_right = new THREE.PointLight(0xffffff, 1, 2000); 
    pointlight_right.position.set(0,  -2000, 0);
    scene.add( pointlight_right );
```

### 渲染器 Renderer
一般情况下我们使用的是WebGL渲染器

```javascript
var renderer = new THREE.WebGLRenderer();
renderer.setPixelRatio( window.devicePixelRatio );//设置canvas的像素比为当前设备的屏幕像素比，避免高分屏下模糊
renderer.setSize( _container.offsetWidth, _container.offsetHeight );//设置渲染器大小,即canvas画布的大小
container.appendChild( renderer.domElement );//在页面中添加canvas
```

## 非必须元素
* 1.物体对象（Object）：包括二维物体（点、线、面）、三维物体、粒子
* 2.控制器(Control): 相机控件，可通过键盘、鼠标控制相机的移动
* 3.加载器（Loader）:特殊的物体例如模型需要使用加载器，而且不同格式的模型需要不同的加载器
* 4.光线投射(Raycaster): 点击射线，用于点击事件

### 控制器 Controls
> FlyControls:飞行控制，用键盘和鼠标控制相机的移动和转动  
OrbitControls::轨道控制器，模拟轨道中的卫星，绕某个对象旋转平移，用键盘和鼠标控制相机位置  
PointerLockControls:指针锁定，鼠标离开画布依然能被捕捉到鼠标交互，主要用于游戏  
TrackballControls：轨迹球控制器，通过键盘和鼠标控制前后左右平移和缩放场景  
TransformControls:变换物体控制器，可以通过鼠标对物体的进行拖放等操作  

项目中使用的是轨道控制器OrbitControls,并限制了上下旋转的角度范围和滚轮控制相机离中心点的最大距离和最小距离

```javascript
    var controls = new THREE.OrbitControls(camera,_container);
    controls.maxPolarAngle=1.5;//上下两极的可视区域的最大角度
    controls.minPolarAngle=1;//上下两极的可视区域最小角度
    controls.enableDamping=true;//允许远近拉伸
    controls.enableKeys=false;//禁止键盘控制
    controls.enablePan=false;//禁止平移
    controls.dampingFactor = 0.1;//鼠标滚动一个单位时拉伸幅度
    controls.rotateSpeed=0.1;//旋转速度
//  controls.enabled = false;//禁用控制器
    controls.minDistance=1000;//离中心物体的最近距离
    controls.maxDistance=3000;//离中心物体的最远距离
```

* 1.场景 scene
    物体、光源、控制器的添加必须使用secen.add(object)添加到场景中才能渲染出来。
    一个3D项目中可同时存在多个scene，通过切换render的scene来切换显示场景
    ```javascript
    let scene = new THREE.Scene();
    ```
* 2.相机 camera
    相机根据投影方式分两种：正交投影相机和透视投影相机
    > 正交投影相机:THREE.OrthographicCamera(left, right, top, bottom, near, far),大小不因远近而变化
    > 透视投影相机:THREE.PerspectiveCamera(fov, aspect, near, far)，遵循近大远小的空间规则
    ```javascript
    let camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    ```
* 3.渲染器 WebGLRenderer
    ```javascript
    let renderer = new THREE.WebGLRenderer();
    ```
* 4.生成物体
  * 设置物体的形状
  * 设置物体的材质
  * 把形状和材质结合起来
  ```javascript
  let cubeGeometry = new THREE.CubeGeometry(1, 1, 1);
  let cubeMaterial = new THREE.MeshBasicMaterial({
      color: 0x00ff00
  });
  let cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
  scene.add(cube);
  ```
* 5.渲染器 渲染 场景 和 相机
    ```javascript
    renderer.render(scene, camera);
    ```