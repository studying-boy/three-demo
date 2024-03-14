<template>
  <div>
  </div>
</template>

<script>
  import * as THREE from 'three.js';
  // 导入轨道控制器
  // import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
  import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
  // 导入动画库
  import GSAP from 'gsap';
  // 导入配置
  import  * as dat from 'dat.gui';
  export default {
    name: 'App',
    mounted() {
      console.log(THREE);
      /**
       * 创建场景对象Scene
       */
      var scene = new THREE.Scene();
      /**
       * 创建网格模型
       */
      var geometry_qiu = new THREE.SphereGeometry(60, 40, 40); //创建一个球体几何对象
      var material_qiu = new THREE.MeshLambertMaterial({
        color: 0x0000ff,
        transparent: true,
        opacity: 0.6
      }); //材质对象Material
      var mesh_qiu = new THREE.Mesh(geometry_qiu, material_qiu); //网格模型对象Mesh
      scene.add(mesh_qiu); //网格模型添加到场景中

      var geometry_zheng = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry
      // 遍历几何体的face属性
      geometry_zheng.faces.forEach(face => {
        face.vertexColors = [
          new THREE.Color(0xffff00),
          new THREE.Color(0xff00ff),
          new THREE.Color(0x00ffff),
        ]
      })
      var material_zheng = new THREE.MeshPhongMaterial({
        // color: 0xff00ff,
        vertexColors: THREE.FaceColors,
        specular:0x4488ee,
        shininess:12
      }); //材质对象Material
      var mesh_zheng = new THREE.Mesh(geometry_zheng, material_zheng); //网格模型对象Mesh
      mesh_zheng.translateY(150);
      mesh_zheng.translateX(150);
      // scene.add(mesh_zheng); //网格模型添加到场景中

      /**
       * 光源设置
       */
      //点光源
      var point = new THREE.PointLight(0xffffff);
      point.position.set(200, 0, 0); //点光源位置
      scene.add(point); //点光源添加到场景中
      //环境光
      var ambient = new THREE.AmbientLight(0x444444);
      scene.add(ambient);
      // console.log(scene)
      // console.log(scene.children)

      /**
       * 相机设置
       */
      var width = window.innerWidth; //窗口宽度
      var height = window.innerHeight; //窗口高度
      var k = width / height; //窗口宽高比
      var s = 200; //三维场景显示范围控制系数，系数越大，显示的范围越大
      //创建相机对象
      var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
      camera.position.set(50, 50, 50); //设置相机位置
      camera.lookAt(scene.position); //设置相机方向(指向的场景对象)

      /**
       * 创建渲染器对象
       */
      var renderer = new THREE.WebGLRenderer();
      renderer.setSize(width, height);//设置渲染区域尺寸
      renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
      document.body.appendChild(renderer.domElement); //body元素中插入canvas对象

      // 创建轨道控制器
      console.log(THREE);
      const controls = new OrbitControls(camera, render.domElement);
      controls.update();

      // 添加辅助坐标轴
      const axisHelper = new THREE.AxisHelper(300);
      scene.add(axisHelper);

      // 添加调试框
      const guiBox = new dat.GUI();
      // guiBox.add(geometry_qiu.position, 'x', 0, 5);
      

      // 设置时钟
      const clock = new THREE.Clock();

      // 设置动画
      console.log(GSAP);
      // GSAP.to(mesh_zheng.position, { x: 5, y: 10, duration: 5, onComplete: () => {
      //   console.log('动画完成1');
      // }});
      // GSAP.to(mesh_zheng.rotation, { x: 2 * Math.PI, duration: 10, onComplete: () => {
      //   console.log('动画完成2');
      // } })

      // 渲染函数
      function render() {
          renderer.render(scene,camera);//执行渲染操作
          // mesh.rotateY(0.01);//每次绕y轴旋转0.01弧度
          requestAnimationFrame(render);//请求再次执行渲染函数render

          controls.update();
      }
      render()
    }
  }
</script>
