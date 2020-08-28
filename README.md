# 3d-house-using-html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
	<meta charset="utf-8">
	<title>THREE.js code...</title>
	<link rel="stylesheet" href="style.css" />

	<!--
	
	⁉️

	How to use

	On PC:
	hold down left mouse button to rotate 	this scene;
	hold down right mouse button to drag 	this scene;
	Also use mouse wheel to change distance 	to this scene. 

	On Mobile:
	Use 1 finger to rotate this scene;
	Use 2 fingers to bring closer/move away 	from this scene. 

	⁉️ 
	
	-->

	<!--
	
	If you liked this scene, please upvote 	my code. 😊
	
	-->

  </head>
  <body>
	<canvas id="cnvs"></canvas>
	<div class="main-container">
	  <div class="loading-container">
		<div class="home-image"></div>
		<p id="code-name">3D House</p>
		<progress min='0' value="0" max="100"></progress>
		<p id="rendering-status"><span>0%</span><br>Rendering this scene...</p>
	  </div>
	</div>
	<script type="module">

	import * as THREE from 'https://threejsfundamentals.org/threejs/resources/threejs/r115/build/three.module.js';
	import {OrbitControls} from 'https://threejsfundamentals.org/threejs/resources/threejs/r115/examples/jsm/controls/OrbitControls.js';

	const mainContainer = document.querySelector('.main-container');
	const infoContainer = document.querySelector('#info');
	const progressbar = document.querySelector('progress');
	const progressbarValue = document.querySelector('span');

	window.onload = () => {
	  setTimeout(() => {
		progressbar.value = 100;
		progressbarValue.textContent = progressbar.value.toFixed(2) + '%';
	  }, 250);
	  setTimeout(() => {
		mainContainer.classList.add('run-animation');
	  }, 400);
	  setTimeout(() => {
		mainContainer.style.display = 'none';
	  }, 3400);
	};

	const main = () => {
	  const canvas = document.querySelector('#cnvs');
	  const renderer = new THREE.WebGLRenderer({canvas, antialias: true});
	  renderer.shadowMap.enabled = true;

	  renderer.setPixelRatio( window.devicePixelRatio );

	  const fov = 75;
	  const aspect = 2;
	  const near = 1;
	  const far = 150;

	  const camera = new THREE.PerspectiveCamera(fov,aspect,near,far);
	  camera.position.x = 5;
	  camera.position.y = 7;
	  camera.position.z = 25;

	  const scene = new THREE.Scene();

	  {
		const near = 1;
		const far = 100;
		const color = 0xcde7f0;
		scene.fog = new THREE.Fog(color, near, far);
		scene.background = new THREE.Color(color);
	  }

	  {
		const color = 0xFFFFFF;
		const intensity = 1;
		const light = new THREE.PointLight(color, intensity);
		light.position.set(5, 12, 15);
		light.castShadow = true;
		scene.add(light);

		const helperSize = 0.5;
		const helper = new THREE.PointLightHelper(light, helperSize);
		scene.add(helper);
	  }

	  {
		const color = 0xFFFFFF;
		const intensity = 1;
		const light = new THREE.PointLight(color, intensity);
		light.position.set(5, 21, 15);
		light.castShadow = true;
		scene.add(light);

		const helperSize = 0.5;
		const helper = new THREE.PointLightHelper(light, helperSize);
		scene.add(helper);
	  }

	  const controls = new OrbitControls(camera, canvas);
	  controls.target.set(5, 0, 16);
	  controls.update();

	  const PI = Math.PI;

	  const makePlane = (url, width, height, rotationX, rotationY, rotationZ, x, y, z, side, repeatX, repeatY, fog = true, receiveShadow = false) => {
		const planeGeo = new THREE.PlaneBufferGeometry(width, height);

		if (width == window.innerWidth) {
		  const planeTextureLoader = new THREE.TextureLoader();
		  planeTextureLoader.load(url, texture => {
			progressbar.value += 0.4;
			progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

			texture.wrapS = texture.wrapT = THREE.MirroredRepeatWrapping;
			texture.needsUpdate = true;
			texture.minFilter = THREE.LinearMipmapLinearFilter;
			texture.repeat.set( repeatX, repeatY );
			texture.anisotropy = 4;

			const planeMat = new THREE.MeshBasicMaterial({map: texture, side, fog});

			const plane = new THREE.Mesh(planeGeo, planeMat);

			plane.rotation.x = rotationX;
			plane.rotation.y = rotationY;
			plane.rotation.z = rotationZ;
			plane.position.x = x;
			plane.position.y = y;
			plane.position.z = z;

			scene.add(plane);
		  });
		} else {
		  const planeTextureLoader = new THREE.TextureLoader();
		  planeTextureLoader.load(url, texture => {
			progressbar.value += 0.4;
			progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

			texture.wrapS = texture.wrapT = THREE.MirroredRepeatWrapping;
			texture.needsUpdate = true;
			texture.minFilter = THREE.LinearMipmapLinearFilter;
			texture.repeat.set( repeatX, repeatY );
			texture.anisotropy = 4;
			texture.image.width = texture.image.width / 2;
			texture.image.height = texture.image.height / 2;

			const planeMat = new THREE.MeshPhongMaterial({map: texture, side, fog});

			const plane = new THREE.Mesh(planeGeo, planeMat);

			plane.receiveShadow = receiveShadow;
			plane.castShadow = false;

			plane.rotation.x = rotationX;
			plane.rotation.y = rotationY;
			plane.rotation.z = rotationZ;
			plane.position.x = x;
			plane.position.y = y;
			plane.position.z = z;

			scene.add(plane);
		  });
		}
		return;
	  };

	  makePlane("https://images.unsplash.com/photo-1577169841504-59773cce950a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80", window.innerWidth, window.innerHeight, - PI / 2, null, null, 5, -0.01, 16, THREE.DoubleSide, 250, 250);
	  makePlane("https://images.unsplash.com/photo-1554755229-ca4470e07232?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80", 50, 60, - PI / 2, null, null, 5, -0.0001, 16, THREE.DoubleSide, 3, 3, false, true);

	  const makeCube = (url = null, width, height, depth, x, y, z, rotationX, rotationY, rotationZ, scaleX = 1, scaleY = 1, scaleZ = 1, repeatX = 1, repeatY = 1, offsetX = 0, offsetY = 0, color = null, opacity = 1, receiveShadow = true, castShadow = true) => {
		const cubeGeo = new THREE.BoxBufferGeometry(width,height,depth);

		if (color !== null) {
		  const cubeMat = new THREE.MeshPhongMaterial({color, opacity, transparent: true, fog: false});

		  const cube = new THREE.Mesh(cubeGeo, cubeMat);

		  cube.receiveShadow = receiveShadow;
		  cube.castShadow = castShadow;

		  cube.position.x = x;
		  cube.position.y = y;
		  cube.position.z = z;
		  cube.rotation.x = rotationX;
		  cube.rotation.y = rotationY;
		  cube.rotation.z = rotationZ;
		  cube.scale.x = scaleX;
		  cube.scale.y = scaleY;
		  cube.scale.z = scaleZ;
		  scene.add(cube);
		}

		const cubeTextureLoader = new THREE.TextureLoader();
		cubeTextureLoader.load(url, texture => {
		  progressbar.value += 0.4;
		  progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

		  texture.wrapS = texture.wrapT = THREE.MirroredRepeatWrapping;
		  texture.repeat.set(repeatX, repeatY);
		  texture.offset.x = offsetX;
		  texture.offset.y = offsetY;
		  texture.needsUpdate = true;
		  texture.minFilter = THREE.LinearMipmapLinearFilter;
		  texture.anisotropy = 4;
		  texture.image.width = texture.image.width / 2;
		  texture.image.height = texture.image.height / 2;

		  const cubeMat = new THREE.MeshPhongMaterial({map: texture, fog: false});

		  const cube = new THREE.Mesh(cubeGeo, cubeMat);

		  cube.castShadow = castShadow;
		  cube.receiveShadow = receiveShadow;

		  cube.position.x = x;
		  cube.position.y = y;
		  cube.position.z = z;
		  cube.rotation.x = rotationX;
		  cube.rotation.y = rotationY;
		  cube.rotation.z = rotationZ;
		  cube.scale.x = scaleX;
		  cube.scale.y = scaleY;
		  cube.scale.z = scaleZ;

		  scene.add(cube);
		});
		return;
	  }

	  const makeCarpet = (url, width, height, rotationX, rotationY, rotationZ, x, y, z, side, repeatX, repeatY, fog, receiveShadow) => {
		makePlane(url, width, height, rotationX, rotationY, rotationZ, x, y, z, side, repeatX, repeatY, fog, receiveShadow);
	  };

	  makeCarpet('https://images.unsplash.com/photo-1592306220103-70eb5d43e1a6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=700&q=80', 16, 16, -PI / 2, null, null, 0, 0.003, 0, null, 1, 1, false, true);
	  makeCarpet('https://images.unsplash.com/photo-1562090517-bc03cc922f4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1267&q=80', 20, 20, -PI / 2, null, null, 16, 0.003, 30, null, 1, 1, false, true);

	  const makeTable = () => {
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 2, 0.3, -0.8, 1.11, 1.1, null, null, null);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 1.8, 0.3, 0.15, 2.02, 1.1, null, null, PI / 2);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 2, 0.3, 1.1, 1.11, 1.1, null, null, null);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 2, 0.3, 0.15, 0.06, 1.1, null, null, PI / 2);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 2, 0.3, -0.8, 1.11, -2.9, null, null, null);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 1.8, 0.3, 0.15, 2.02, -2.9, null, null, PI / 2);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 2, 0.3, 1.1, 1.11, -2.9, null, null, null);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.1, 2, 0.3, 0.15, 0.06, -2.9, null, null, PI / 2);
		makeCube('https://images.unsplash.com/photo-1542740521-2b73fabba26a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 3, 0.1, 5, 0.15, 2.12, -0.84, null, null, null);
		makeCube('https://images.unsplash.com/photo-1542740521-2b73fabba26a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1.8, 0.1, 5, 0.15, 0.82, -0.84, null, null, null);
	  };

	  makeTable();

	  const makeFurniture = () => {
		const makeBottomPart = (url, width, height, depth, x, y, z, rotationX, rotationY, rotationZ) => {
		  makeCube(url, width, height, depth, x, y, z, rotationX, rotationY, rotationZ);
		};

		const makeShape = (url, scaleX, scaleY, scaleZ, rotationX, rotationY, rotationZ, x, y, z, depth, receiveShadow, castShadow) => {
		  const shape = new THREE.Shape();

		  shape.moveTo(-3, 4);

		  shape.quadraticCurveTo(-5, 4, -5, 4);
		  shape.quadraticCurveTo(-6, 4, -5, 3);

		  shape.moveTo(-5.5, 3.75);

		  shape.quadraticCurveTo(-5.8, 2.5, -5.5, 2.5);
		  shape.quadraticCurveTo(-3, 2.5, -3, 2.5);
		  shape.quadraticCurveTo(-2.8, 2.5, -2.77, 3);

		  shape.moveTo(-2.8, 2.8);

		  shape.quadraticCurveTo(-2.5, 4, -3, 4);

		  const geometry = new THREE.ExtrudeBufferGeometry(shape, {depth, bevelEnabled: false});

		  const textureLoader = new THREE.TextureLoader();
		  textureLoader.load(url, texture => {
			progressbar.value += 0.4;
			progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

			texture.needsUpdate = true;
			texture.wpapS = texture.wpapT = THREE.ClampToEdgeWrapping;
			texture.repeat.set(3, 3);
			texture.minFilter = THREE.LinearMipmapLinearFilter;
			texture.anisotropy = 4;
			texture.image.width = texture.image.width / 2;
			texture.image.height = texture.image.height / 2;

			const material = new THREE.MeshPhongMaterial({map: texture, fog: false});

			const mesh = new THREE.Mesh(geometry, material);

			mesh.castShadow = castShadow;
			mesh.receiveShadow = receiveShadow;

			mesh.scale.set(scaleX, scaleY, scaleZ);
			mesh.rotation.x = rotationX;
			mesh.rotation.y = rotationY;
			mesh.rotation.z = rotationZ;
			mesh.position.x = x;
			mesh.position.y = y;
			mesh.position.z = z;

			scene.add(mesh);
		  });
		  return;
		};

		const makeSofa = () => {
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, -3, 0.25, 3.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 0.2, -4.15, 0.6, 3.7, null, null, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, -5.3, 0.25, 3.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 8.8, 0.2, -3, 0.6, -0.8, null, PI / 2, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 8.8, 0.2, -5.3, 0.6, -0.8, null, PI / 2, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, -5.3, 0.25, -0.8, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, -3, 0.25, -0.8, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, -5.3, 0.25, -5.3, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, -3, 0.25, -5.3, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 0.2, -4.15, 0.6, -5.3, null, null, PI / 2);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 1, 1, null, null, null, 0.1, -1.8, 3.3, 0.5, false, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 1, 1, null, null, null, 0.1, -1.8, -5.4, 0.5, true, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 5.7, 1, PI / 2, null, null, 0.1, 1.2, -19.3, 0.5, true, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 6.13, 1.1, PI / 2, -PI / 3, null, -3.12, -1.35, -20.722, 0.5, true, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.5, 6.15, 1, PI / 2, -PI / 2, null, -5.1, -0.75, -20.78, 0.5, false, false);
		};

		const makeArmchairs = () => {
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 3, 0.25, 3.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.1, 0.2, 3.95, 0.6, 3.7, null, null, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 4.9, 0.25, 3.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 3, 0.25, 0.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.1, 0.2, 3.95, 0.6, 0.7, null, null, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 4.9, 0.25, 0.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.8, 0.2, 2.95, 0.6, 2.2, PI / 2, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.8, 0.2, 4.9, 0.6, 2.2, PI / 2, null, null);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.8, 1, 1, null, null, null, 7.3, -1.8, 3.45, 0.5, false, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.8, 1, 1, null, null, null, 7.3, -1.8, 0.45, 0.5, false, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.8, 1.8, 1, PI / 2, null, null, 7.3, 1.2, -3.65, 0.5, true, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.55, 1.8, 1, PI / 2, PI / 2, null, 4.65, 3.8, -3.65, 0.5, false, false);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 1.8, 1.1, PI / 2, PI / 3, null, 6.63, 5.85, -3.67, 0.5, true, true);

		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 3, 0.3, -1.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.1, 0.2, 3.95, 0.65, -1.7, null, null, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 4.9, 0.3, -1.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 3, 0.3, -4.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.1, 0.2, 3.95, 0.65, -4.7, null, null, PI / 2);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 0.5, 0.2, 4.9, 0.3, -4.7, null, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.8, 0.2, 2.95, 0.65, -3.2, PI / 2, null, null);
		  makeBottomPart('https://images.pexels.com/photos/172296/pexels-photo-172296.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.8, 0.2, 4.9, 0.65, -3.2, PI / 2, null, null);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.8, 1, 1, null, null, null, 7.3, -1.8, -1.95, 0.5, false, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.8, 1, 1, null, null, null, 7.3, -1.8, -4.95, 0.5, false, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.8, 1.8, 1, PI / 2, null, null, 7.3, 1.2, -8.95, 0.5, true, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.55, 1.8, 1, PI / 2, PI / 2, null, 4.65, 3.8, -9, 0.5, true, true);
		  makeShape('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 1.8, 1.1, PI / 2, PI / 3, null, 6.63, 5.85, -9, 0.5, true, true);
		};

		makeArmchairs();
		makeSofa();
	  };

	  makeFurniture();

	  const makeSphere = (url, radius, widthSegments, heightSegments, PIstart, PIlength, thetaStart, thetaLength, x, y, z, scaleX = 1, scaleY = 1, scaleZ = 1, receiveShadow = true, castShadow = true) => {
		const geometry = new THREE.SphereBufferGeometry(radius, widthSegments, heightSegments, PIstart, PIlength, thetaStart, thetaLength);

		const textureLoader = new THREE.TextureLoader();
		textureLoader.load(url, texture => {
		  progressbar.value += 0.4;
		  progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

		  texture.needsUpdate = true;
		  texture.wrapS = texture.wrapT = THREE.RepeatWrapping;
		  texture.minFilter = THREE.LinearMipmapLinearFilter;
		  texture.anisotropy = 4;
		  texture.image.width = texture.image.width / 2;
		  texture.image.height = texture.image.height / 2;

		  const material = new THREE.MeshPhongMaterial({map: texture, side: THREE.DoubleSide, fog: false});

		  const sphere = new THREE.Mesh(geometry, material);

		  sphere.castShadow = castShadow;
		  sphere.receiveShadow = receiveShadow;

		  sphere.position.x = x;
		  sphere.position.y = y;
		  sphere.position.z = z;
		  sphere.scale.x = scaleX;
		  sphere.scale.y = scaleY;
		  sphere.scale.z = scaleZ;

		  scene.add(sphere);
		});
		return;
	  };

	  const makePlates = () => {
		makeSphere('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 0.75, PI * 0.25, 0.2, 3.17, -0.8, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 0.85, PI * 0.15, -0.6, 3.17, -2.4, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 0.85, PI * 0.15, 0.9, 3.17, -2.4, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 0.85, PI * 0.15, -0.6, 3.17, 0.75, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1557411732-1797a9171fcf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 0.85, PI * 0.15, 0.9, 3.17, 0.75, 1, 1, 1, false, true);
	  };

	  makePlates();

	  const makePeaches = () => {
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 1, PI * 1, 0, 2.37, -0.8, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 1, PI * 1, 0.3, 2.37, -0.8, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 1, PI * 1, 0, 2.42, -0.505, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 1, PI * 1, 0, 2.42, -1.095, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 1, PI * 1, 0.3, 2.42, -1.095, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0, PI * 2, PI * 1, PI * 1, 0.3, 2.42, -0.505, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0.25, PI * 2, PI * 1, PI * 1, 0.59, 2.43, -0.795, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0.25, PI * 2, PI * 1, PI * 1, 0.6, 2.49, -1.08, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0.25, PI * 2, PI * 1, PI * 1, 0.6, 2.49, -0.505, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0.25, PI * 2, PI * 1, PI * 1, -0.265, 2.49, -0.8, 0.15, 0.15, 0.15, false);
		makeSphere('https://images.unsplash.com/photo-1557682233-43e671455dfa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=486&q=80', 1, 24, 24, PI * 0.25, PI * 2, PI * 1, PI * 1, 0.15, 2.6, -0.7, 0.15, 0.15, 0.15, false);
	  };

	  makePeaches();

	  const makeTable2 = () => {
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 3, 0.1, 0.2, 11.5, 1.135, 26.5, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 3, 0.1, 0.2, 18.5, 1.135, 26.5, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 3, 0.1, 0.2, 11.5, 1.135, 33.5, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/1323712/pexels-photo-1323712.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 3, 0.1, 0.2, 18.5, 1.135, 33.5, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1550053808-52a75a05955d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.1, 10, 11, 15, 2.275, 30, null, null, PI / 2, 0.75, 0.75, 0.75);
	  };

	  makeTable2();

	  const makeChairs = () => {
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 7.8, 0.67, 30.6, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 9.2, 0.67, 30.6, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 7.8, 0.67, 29.4, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 9.2, 0.67, 29.4, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 2.5, 8.5, 1.32, 30, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 4, 7.26, 2.82, 30, PI / 2, PI / 12, null, 0.75, 0.75, 0.75);

		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 14.75, 0.67, 22.25, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 14.75, 0.67, 23.75, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 16.25, 0.67, 22.25, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 16.25, 0.67, 23.75, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 2.5, 15.5, 1.37, 23, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 4, 15.5, 2.85, 21.78, PI / 2.4, null, PI / 2, 0.75, 0.75, 0.75);

		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 14.75, 0.67, 36.25, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 14.75, 0.67, 37.75, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 16.25, 0.67, 36.25, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 16.25, 0.67, 37.75, null, PI / 2, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 2.5, 15.5, 1.37, 37, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 4, 15.5, 2.85, 38.24, -PI / 2.4, null, PI / 2, 0.75, 0.75, 0.75);

		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 20.8, 0.67, 30.6, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 22.2, 0.67, 30.6, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 20.8, 0.67, 29.4, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.unsplash.com/photo-1544840280-80e54213dfdc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1.8, 0.1, 0.2, 22.2, 0.67, 29.4, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 2.5, 21.5, 1.32, 30, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/207300/pexels-photo-207300.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.2, 2.5, 4, 22.75, 2.82, 30, -PI / 2, PI / 12, null, 0.75, 0.75, 0.75);
	  };

	  makeChairs();

	  const makeBasicMatCube = (url = null, width, height, depth, x, y, z, rotationX, rotationY, rotationZ, scaleX = 1, scaleY = 1, scaleZ = 1, repeatX = 1, repeatY = 1, offsetX = 0, offsetY = 0) => {
		const cubeGeo = new THREE.BoxBufferGeometry(width, height, depth);

		const cubeTextureLoader = new THREE.TextureLoader();
		cubeTextureLoader.load(url, texture => {
		  progressbar.value += 0.4;
		  progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

		  texture.wrapS = texture.wrapT = THREE.MirroredRepeatWrapping;
		  texture.repeat.set(repeatX, repeatY);
		  texture.offset.x = offsetX;
		  texture.offset.y = offsetY;
		  texture.needsUpdate = true;
		  texture.minFilter = THREE.LinearMipmapLinearFilter;
		  texture.anisotropy = 4;
		  texture.image.width = texture.image.width / 2;
		  texture.image.height = texture.image.height / 2;

		  const cubeMat = new THREE.MeshBasicMaterial({map: texture, fog: false});

		  const cube = new THREE.Mesh(cubeGeo, cubeMat);

		  cube.position.x = x;
		  cube.position.y = y;
		  cube.position.z = z;
		  cube.rotation.x = rotationX;
		  cube.rotation.y = rotationY;
		  cube.rotation.z = rotationZ;
		  cube.scale.x = scaleX;
		  cube.scale.y = scaleY;
		  cube.scale.z = scaleZ;

		  scene.add(cube);
		});
		return;
	  };

	  const makeWalls = () => {
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 80, 20, -19.63, 7.505, 16, -PI / 2, null, null, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 65.8, 20, 5.4, 7.505, -13.7, -PI / 2, null, PI / 2, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 34.6, 10, 29.6, 3.75, -0.6, -PI / 2, null, null, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 38.1, 10, 29.67, 3.75, 31.75, -PI / 2, null, null, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 64.8, 20, 5, 7.505, 45.7, -PI / 2, null, PI / 2, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 60, 15.75, 2, -21, 7.88, 16, PI / 2, PI / 2, PI / 2);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 52, 15.75, 2, 4, 7.88, -15.03, null, null, null);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 52, 15.75, 2, 4, 7.88, 47.03, null, null, null);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 38.1, 5.1, 29.69, 13.1, 31.75, -PI / 2, null, null, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 10, 26.03, 29.67, 11.25, 15.11, null, null, null, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 6.1, 85.35, 30.36, 13.45, 16, null, null, null, 0.75, 0.75, 0.75, 0.9, 0.3);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 10, 30.8, 30.38, 3.75, 36.48, null, null, null, 0.75, 0.75, 0.75, 0.4, 0.4);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 10, 28.5, 30.38, 3.75, -5.35, null, null, null, 0.75, 0.75, 0.75, 0.4, 0.4);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 4.9, 2.7, 30.38, 9.33, 47, null, null, null, 0.75, 0.75, 0.75, 0.1, 0.2);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 4.9, 2.7, 30.38, 9.33, -15, null, null, null, 0.75, 0.75, 0.75, 0.1, 0.2);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 12, 14, 1, 34.55, 5.26, 24.48, null, null, null, 0.75, 0.75, 0.75, 0.2, 0.6);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 12, 14, 1, 34.55, 5.26, 5.71, null, null, null, 0.75, 0.75, 0.75, 0.2, 0.6);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 12, 26.03, 1, 34.55, 10.83, 15.09, PI / 2, null, null, 0.75, 0.75, 0.75, 0.2, 1.1);
		makePlane("https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80", 9.7, 18.03, - PI / 2, null, null, 34.1, 0.01, 15.09, THREE.DoubleSide, 1, 1);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 26.03, 4.95, 1, 39.4, 9.35, 15.09, null, PI / 2, null, 0.75, 0.75, 0.75, 0.35, 0.25);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 9.9, 10, 1, 39.4, 3.75, 21.17, null, PI / 2, null, 0.75, 0.75, 0.75, 0.15, 0.5);
		makeBasicMatCube('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 9.38, 10, 1, 39.4, 3.75, 8.86, null, PI / 2, null, 0.75, 0.75, 0.75, 0.15, 0.5);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 24.9, 5.1, 29.69, 13.1, -3.99, -PI / 2, null, null, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeBasicMatCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 9.38, 10, 0.01, 29.99, 3.75, 8.86, null, PI / 2, null, 0.75, 0.75, 0.75, 0.15, 0.8);
		makeBasicMatCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 9.9, 10, 0.01, 30.05, 3.75, 21.17, null, PI / 2, null, 0.75, 0.75, 0.75, 0.15, 0.8);
		makeBasicMatCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 0.01, 10, 26.03, 30.05, 11.25, 15.11, null, null, null, 0.75, 0.75, 0.75);
	  };

	  makeWalls();

	  const makeWindows = () => {
		makeCube(null, 1, 28.2, 4.91, 29.67, 9.35, 35.46, -PI / 2, null, null, 0.75, 0.75, 0.75, null, null, null, null, 0x42dcea, 0.3, false, false);
		makeCube(null, 1, 24.9, 4.91, 29.67, 9.35, -3.99, -PI / 2, null, null, 0.75, 0.75, 0.75, null, null, null, null, 0x42dcea, 0.3, false, false);
	  };

	  makeWindows();

	  const makeDoors = () => {
		makeBasicMatCube('https://images.pexels.com/photos/277559/pexels-photo-277559.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 10, 6.77, 39.4, 3.75, 14.92, null, null, null, 0.75, 0.75, 0.75, 0.2, 0.63, 0.29, 0.14);
		makeBasicMatCube('https://images.unsplash.com/photo-1541112455343-efc2c954558f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1, 10, 6.77, 29.67, 3.75, 14.92, null, null, null, 0.75, 0.75, 0.75, 0.4, 0.55, 0.3, 0.21);
	  };

	  makeDoors();

	  const makeRoof = () => {
		makeBasicMatCube('https://images.unsplash.com/photo-1512331327491-d03cb97d92b4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 52.6, 45, 4.35, 31, 31.7, PI / 4, null, PI / 2, 1, 1, 1, 1, 2);
		makeBasicMatCube('https://images.unsplash.com/photo-1512331327491-d03cb97d92b4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 52.6, 45, 4.35, 31, 0.3, -PI / 4, null, PI / 2, 1, 1, 1, 1, 2);
		makeCube('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.01, 52.6, 44.4, 4.35, 31, 1.01, -PI / 4, null, PI / 2, 1, 1, 1, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 0.01, 52.6, 44.4, 4.35, 31, 30.99, PI / 4, null, PI / 2, 1, 1, 1, 1, 1, 0, 0, null, 1, true, false);

		const makeGeometry = (url, x, y, z, rotationX, rotationY, rotationZ, repeatX, repeatY) => {
		  const shape = new THREE.Shape();

		  shape.moveTo(-31.92, 2);
		  shape.lineTo(31.92, 2);
		  shape.lineTo(0, 34.1);

		  const geometry = new THREE.ExtrudeBufferGeometry(shape, {depth: 0.01, bevelEnabled: false});

		  if ( repeatY === 0.07 ) {
			const textureLoader = new THREE.TextureLoader();
			textureLoader.load(url, texture => {
			  progressbar.value += 0.4;
			  progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

			  texture.needsUpdate = true;
			  texture.wrapS = texture.wrapT = THREE.MirroredRepeatWrapping;
			  texture.repeat.set(repeatX, repeatY);
			  texture.anisotropy = 4;
			  texture.image.width = texture.image.width / 2;
			  texture.image.height = texture.image.height / 2;

			  const material = new THREE.MeshBasicMaterial({map: texture, fog: false});

			  const mesh = new THREE.Mesh(geometry, material);

			  mesh.position.x = x;
			  mesh.position.y = y;
			  mesh.position.z = z;
			  mesh.rotation.x = rotationX;
			  mesh.rotation.y = rotationY;
			  mesh.rotation.z = rotationZ;

			  scene.add(mesh);
			});
		  } else {
			const textureLoader = new THREE.TextureLoader();
			textureLoader.load(url, texture => {
			  progressbar.value += 0.4;
			  progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

			  texture.needsUpdate = true;
			  texture.wrapS = texture.wrapT = THREE.MirroredRepeatWrapping;
			  texture.repeat.set(repeatX, repeatY);
			  texture.anisotropy = 4;
			  texture.image.width = texture.image.width / 2;
			  texture.image.height = texture.image.height / 2;

			  const material = new THREE.MeshPhongMaterial({map: texture, fog: false});

			  const mesh = new THREE.Mesh(geometry, material);

			  mesh.receiveShadow = true;

			  mesh.position.x = x;
			  mesh.position.y = y;
			  mesh.position.z = z;
			  mesh.rotation.x = rotationX;
			  mesh.rotation.y = rotationY;
			  mesh.rotation.z = rotationZ;

			  scene.add(mesh);
			});
		  }
		  return;
		};

		makeGeometry('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260', 30.66, 13.5, 16, null, PI / 2, null, 0.02, 0.07);
		makeGeometry('https://images.pexels.com/photos/220166/pexels-photo-220166.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260', -21.97, 13.5, 16, null, PI / 2, null, 0.02, 0.07);
		makeGeometry('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 30.64, 13.49, 16, null, PI / 2, null, 0.02, 0.02);
		makeGeometry('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', -21.95, 13.49, 16, null, PI / 2, null, 0.02, 0.02);
	  };

	  makeRoof();

	  const makeFloor = () => {
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 50.6, 80.1, 11, 15.38, 16, null, null, PI / 2, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 16.03, 45.1, -13.99, 15.38, 29.1, null, null, PI / 2, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 16.03, 24.1, -13.99, 15.38, -4.99, null, null, PI / 2, 0.75, 0.75, 0.75, 1, 1, 0, 0, null, 1, true, false);
		makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 13.03, 10.85, -15.095, 15.38, 8.117, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 51.5, 80.1, 11.32, 16.1, 16, null, null, PI / 2, 0.75, 0.75, 0.75);
		makeCube('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 18.65, 45.1, -14.97, 16.1, 29.1, null, null, PI / 2, 0.75, 0.75, 0.75, 0.3, 0.3);
		makeCube('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 18.65, 24.1, -14.97, 16.1, -4.99, null, null, PI / 2, 0.75, 0.75, 0.75, 0.4, 0.4);
		makeCube('https://images.pexels.com/photos/129731/pexels-photo-129731.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 15.65, 10.85, -16.095, 16.1, 8.117, null, null, PI / 2, 0.75, 0.75, 0.75, 0.25, 0.25);
	  };

	  makeFloor();

	  const makeStairs = () => {
		let x = -5;
		let y = 0.555;
		let z = 14;
		const makeStair = () => {
		  for (let i = 1; i <= 16; i++) {
			if ( i <= 4 ) {
			  makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1.5, 1.5, 3, x, y, z, null, null, null, 0.75, 0.75, 0.75);
			  x -= 0.8;
			  y += 1.125;
			} else if ( i === 5 ) {
			  x -= 0.888;
			  y -= 0.75;
			  makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 3, 0.5, 3, x, y, z, null, null, null, 0.75, 0.75, 0.75);
			} else if ( i === 6 ) {
			  y += 0.75;
			  z -= 1.4;
			  makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1.5, 1.5, 3, x, y, z, null, PI / 2, null, 0.75, 0.75, 0.75);
			} else {
			  y += 1.125;
			  z -= 0.8;
			  makeCube('https://images.unsplash.com/photo-1551554781-c46200ea959d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1.5, 1.5, 3, x, y, z, null, PI / 2, null, 0.75, 0.75, 0.75);
			}
		  }
		};

		makeStair();
	  };

	  makeStairs();

	  const makeCylinder = (url, radiusTop, radiusBottom, height, radialSegmets, x, y, z, rotationX, rotationY, rotationZ) => {
		const geometry = new THREE.CylinderBufferGeometry(radiusTop, radiusBottom, height, radialSegmets);

		const textureLoader = new THREE.TextureLoader();
		textureLoader.load(url, texture => {
		  progressbar.value += 0.4;
		  progressbarValue.textContent = progressbar.value.toFixed(2) + '%';

		  texture.needsUpdate = true;
		  texture.wrapS = texture.wrapT = THREE.RepeatWrapping;
		  texture.minFilter = THREE.LinearMipmapLinearFilter;
		  texture.anisotropy = 4;
		  texture.image.width = texture.image.width / 2;
		  texture.image.height = texture.image.height / 2;

		  const material = new THREE.MeshPhongMaterial({map: texture, fog: false});

		  const cylinder = new THREE.Mesh(geometry, material);

		  cylinder.castShadow = cylinder.receiveShadow = true;

		  cylinder.position.x = x;
		  cylinder.position.y = y;
		  cylinder.position.z = z;
		  cylinder.rotation.x = rotationX;
		  cylinder.rotation.y = rotationY;
		  cylinder.rotation.z = rotationZ;

		  scene.add(cylinder);
		});
		return;
	  };

	  const makeFireplace = () => {
		makeCube('https://images.unsplash.com/photo-1555611453-c176c271ef0c?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80', 2, 4, 0.5, -18.25, 2.02, 0, null, null, null);
		makeCube('https://images.unsplash.com/photo-1586296128976-170d80a94b3f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 2, 3.8, 0.2, -18.25, 1.92, -0.35, null, null, null);
		makeCube('https://images.unsplash.com/photo-1555611453-c176c271ef0c?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80', 2, 5, 0.5, -18.25, 4.27, -2.25, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1586296128976-170d80a94b3f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 2, 4, 0.2, -18.25, 3.92, -2.25, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1586296128976-170d80a94b3f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 4, 3.78, 0.2, -19.25, 1.9, -2.25, null, PI / 2, null);
		makeCube('https://images.unsplash.com/photo-1555611453-c176c271ef0c?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80', 2, 4, 0.5, -18.25, 2.02, -4.5, null, null, null);
		makeCube('https://images.unsplash.com/photo-1586296128976-170d80a94b3f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 2, 3.8, 0.2, -18.25, 1.92, -4.15, null, null, null);
		makeCube('https://images.unsplash.com/photo-1586296128976-170d80a94b3f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 2, 3.6, 0.2, -18.25, 0.105, -2.25, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1585314062340-f1a5a7c9328d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 2, 3.6, 0.8, -18.25, 0.6, -2.25, PI / 2, null, null);

		const makeFireWoods = () => {
		  makeCylinder('https://images.unsplash.com/photo-1533035350251-aa8b8e208d95?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 1.2, 24, -17.6, 1.2, -3.2, PI / 2, null, null);
		  makeCylinder('https://images.unsplash.com/photo-1533035350251-aa8b8e208d95?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 1.3, 24, -18.8, 1.2, -3, PI / 2, null, null);
		  makeCylinder('https://images.unsplash.com/photo-1533035350251-aa8b8e208d95?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 1.4, 24, -18.2, 1.2, -3.1, PI / 2, null, null);
		  makeCylinder('https://images.unsplash.com/photo-1533035350251-aa8b8e208d95?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 1, 24, -18, 1.2, -1, null, null, PI / 2);
		  makeCylinder('https://images.unsplash.com/photo-1533035350251-aa8b8e208d95?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 1, 24, -18.6, 1.2, -1.8, null, PI / 4, PI / 2);
		  makeCylinder('https://images.unsplash.com/photo-1533035350251-aa8b8e208d95?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 1, 24, -17.7, 1.2, -1.8, null, -PI / 3, PI / 2);
		};

		makeFireWoods();
	  };

	  makeFireplace();

	  const makeShelfs = () => {
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 4, 0.1, 1, -2, 3.6, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.1, 1, 1, -3.95, 4.15, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.1, 1, 1, -0.05, 4.15, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 4, 0.1, 1, -2, 4.7, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1525039271310-0fd0060ff56d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 3.8, 0.6, 0.6, -2, 3.95, -12.8, null, null, null);

		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 4, 0.1, 1, 2, 5.6, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.1, 1, 1, 0.05, 6.15, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.1, 1, 1, 3.95, 6.15, -12.8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1533035353720-f1c6a75cd8ab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 4, 0.1, 1, 2, 6.7, -12.8, null, null, null);
		makeSphere('https://images.unsplash.com/photo-1528459105426-b9548367069b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=614&q=80', 0.4, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 1, 6.05, -12.5, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1546453667-8a8d2d07bc20?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.4, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 2, 6.05, -12.5, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1577574026212-dcc9622e2622?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80', 0.4, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 3, 6.05, -12.5, 1, 1, 1, false, true);
	  };

	  makeShelfs();

	  const makeCupboard = () => {
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 8, 0.1, 28.24, 4.015, -8, null, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 14, 0.1, 28.24, 8.064, -1.05, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 8, 0.1, 28.24, 4.015, 5.9, null, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 13.8, 0.1, 28.24, 0.064, -1.05, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 7, 0.1, 28.24, 4.064, -4.45, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 7.9, 0.1, 28.24, 4.07, -0.9, null, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 6.7, 0.1, 28.24, 4.064, 2.5, PI / 2, null, null);
		makeSphere('https://images.unsplash.com/photo-1545873692-64145c8c42ed?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1352&q=80', 1, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 28.23, 5.05, -4.25, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1458322493962-69c5a4ef7ddf?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 28.23, 5.05, 2.5, 1, 1, 1, false, true);
		makeSphere('https://images.unsplash.com/photo-1574660090267-06b5410c64d4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 1, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 28.23, 1.05, -4.25, 1, 1, 1, false, true);
		makeSphere('https://images.pexels.com/photos/2505693/pexels-photo-2505693.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 1, 30, 30, PI * 0, PI * 2, PI * 1, PI * 1, 28.23, 1.05, 2.5, 1, 1, 1, false, true);
	  };

	  makeCupboard();

	  const makeTV = () => {
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 12, 1, -17.24, 10, 30, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 6, 1, -17.24, 7.5, 36.5, null, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 6, 1, -17.24, 7.5, 23.5, null, null, null);
		makeCube('https://images.unsplash.com/photo-1586609143615-019f8824129f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 4, 24, 1, -17.24, 4, 30, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 3, 2, -17.24, 2.5, 35.5, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 3, 2, -17.24, 2.5, 24.5, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 2, 8, 0.5, -17.24, 1.75, 30, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1554328103-f1ab574c5a12?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=564&q=80', 1, 14, 6, -18.75, 7.5, 30, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1515799251528-8e14681f214e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1489&q=80', 1, 4, 0.5, -16.75, 2.25, 30, PI / 2, null, null);
		makeCube('https://images.unsplash.com/photo-1541816139614-b1c0b6a13f5b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 1, 2, 0.1, -16.75, 4.5, 30, PI / 2, null, null);
		makeCylinder('https://images.unsplash.com/photo-1541816139614-b1c0b6a13f5b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 0.2, 0.2, 0.3, 24, -16.75, 4.7, 30, null, null, null);
		makeCube('https://images.unsplash.com/photo-1541816139614-b1c0b6a13f5b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80', 4.5, 10, 0.5, -16.75, 7.1, 30, null, PI / 2, PI / 2);
		makeCube('https://images.unsplash.com/photo-1544952019-734321a2a151?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=701&q=80', 0.0001, 4, 9.5, -16.495, 7.1, 30, null, null, null, 1, 1, 1, 1, 1, 0, 0, null, 1, false);
	  };

	  makeTV();

	  const makeChandelier = () => {
		makeSphere('https://images.unsplash.com/photo-1553532434-5ab5b6b84993?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1267&q=80', 1, 30, 30, PI * 0, PI * 2, PI * 0, PI * 0.45, 5, 12.15, 15);
		makeCylinder('https://images.unsplash.com/photo-1517196084897-498e0abd7c2d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.2, 0.2, 0.3, 30, 5, 14.8, 15, null, null, null);
		makeCylinder('https://images.unsplash.com/photo-1517196084897-498e0abd7c2d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=634&q=80', 0.1, 0.1, 1.5, 30, 5, 13.9, 15, null, null, null);
	  };

	  makeChandelier();

	  const makeBoxes = () => {
		let x = -4;
		let y = 18.005;
		let z = -5;

		for (let i = 1; i <= 30; i++) {
		  if ( i <= 6 ) {
			makeCube('https://images.unsplash.com/photo-1519972064555-542444e71b54?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80', 3, 3, 3, x, y, z, null, null, null);
			x += 6;
		  } else if ( i > 6 && i <= 12 ) {
			z = 37;
			makeCube('https://images.unsplash.com/photo-1519972064555-542444e71b54?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80', 3, 3, 3, x, y, z, null, null, null);
			x += 6;
		  } else if ( i > 12 && i <= 17 ) {
			makeCube('https://images.pexels.com/photos/949587/pexels-photo-949587.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 6, 1, 3, x, y, z, null, null, null, 1, 1, 1, 1, 1, 0, 0, null, 1, false, true);
			y += 1;
			z += 0.2;
		  } else if ( i > 17 && i <= 24 ) {
			makeCube('https://images.unsplash.com/photo-1590593162201-f67611a18b87?ixlib=rb-1.2.1&auto=format&fit=crop&w=671&q=80', 6, 1, 3, x, y, z, null, null, null, 1, 1, 1, 1, 1, 0, 0, null, 1, false, true);
			y += 1;
			z -= 0.15;
		  } else {
			makeCube('https://images.pexels.com/photos/2911521/pexels-photo-2911521.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940', 5, 2, 12, x, y, z, null, null, null, 1, 1, 1, 0.5, 0.5, 0, 0, null, 1, false, true);
			x += 0.25;
			y += 2;
			z -= 0.3;
		  }

		  if ( i === 6 ) {
			x -= 36;
		  }

		  if ( i === 12 ) {
			z -= 34;
			y -= 1;
			x -= 7;
		  }

		  if ( i === 17 ) {
			z += 10;
			y -= 5;
		  }

		  if ( i === 24 ) {
			y -= 6.5;
			z *= 2.1;
		  }
		}
	  };

	  makeBoxes();

	  const resize = renderer => {
		const canvas = renderer.domElement;
		const width = canvas.clientWidth;
		const height = canvas.clientHeight;
		const needResize = canvas.width != width || canvas.height != height;
		if (needResize) {
		  renderer.setSize(width, height, false);
		}
		return needResize;
	  };

	  const render = () => {
		const canvas = renderer.domElement;
		camera.aspect = canvas.clientWidth / canvas.clientHeight;
		camera.updateProjectionMatrix();

		renderer.render(scene, camera);

		resize(renderer);

		requestAnimationFrame( render );
	  };

	  render();

	};

	main();

	</script>
  </body>
</html>
