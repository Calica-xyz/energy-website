<script>
  import { onMount } from "svelte";

  var illo;
  var head;
  var starGroup;
  var rotationSpeed = 0.004;

  let bottomShapes = [];
  let allShapes = [];
  let shapeColors = [];
  let bottomRemoved = false;
  let convertedToClouds = false;

  // boolean flag for spin
  let isSpinning = true;

  let initZoom;
  let initX;
  let initY;

  let scrollY;
  let innerHeight;
  let innerWidth;

  $: if (innerHeight && scrollY) {
    scrollEarth();
  }

  function scrollEarth() {
    if (scrollY > innerHeight) scrollY = innerHeight;

    let zoomScale = 1 + (scrollY / innerHeight) * 2.75;

    // console.log(scrollY, innerHeight, zoomScale);

    if (zoomScale <= 1.2) {
      if (bottomRemoved) toggleBottomShapes();
      if (convertedToClouds) toggleClouds();
      changeRotationSpeed(0.004);
    } else if (zoomScale < 2.7) {
      if (!bottomRemoved) toggleBottomShapes();
      if (convertedToClouds) toggleClouds();
      changeRotationSpeed(0.004);
    } else {
      if (!bottomRemoved) toggleBottomShapes();
      if (!convertedToClouds) toggleClouds();
      changeRotationSpeed(0.001);
    }

    illo.zoom = initZoom * zoomScale;

    let xScale = (scrollY / innerHeight) * initX;
    let yScale = (scrollY / innerHeight) * Math.abs(initY);

    illo.translate = { x: initX - xScale, y: initY + yScale };

    window.$("#header-text").css("opacity", 1 - scrollY / innerHeight);
  }

  function toggleBottomShapes() {
    if (bottomRemoved) {
      for (let bottomShape of bottomShapes) {
        head.addChild(bottomShape);
      }
    } else {
      for (let bottomShape of bottomShapes) {
        bottomShape.remove();
      }
    }
    bottomRemoved = !bottomRemoved;
  }

  function toggleClouds() {
    if (convertedToClouds) {
      for (let i = 0; i < allShapes.length; i++) {
        allShapes[i].color = shapeColors[i];
      }
    } else {
      for (let shape of allShapes) {
        shape.color = "rgba(255, 255, 255, 0.1)";
      }
    }

    convertedToClouds = !convertedToClouds;
  }

  function changeRotationSpeed(newRotationSpeed) {
    rotationSpeed = newRotationSpeed;
  }

  function drawEarth() {
    // setting up Zdog illustration element
    const illoElem = document.querySelector(".illo");
    const illoSize = 50;

    initZoom = Math.floor((innerHeight / illoSize) * 1);
    if (innerWidth <= 768) initZoom *= 0.85;

    illoElem.setAttribute("width", innerWidth);

    illoElem.setAttribute("height", innerHeight * 2);

    // Zdog math variables
    const TAU = Zdog.TAU;

    initX = innerWidth <= 768 ? 0 : 10;
    initY = innerWidth <= 768 ? -illoSize / 2 - 5 : -illoSize / 2;

    // illustration base
    illo = new Zdog.Illustration({
      element: illoElem,
      zoom: initZoom,
      dragRotate: true,
      rotate: { y: TAU / 4 },
      translate: { x: initX, y: initY },
    });

    starGroup = new Zdog.Group({
      addTo: illo,
    });

    // The stars in the background
    for (let i = 0; i < 300; i++) {
      let x = Math.random() * 100 - 50;
      let y = Math.random() * 100 - 50;
      let z = Math.random() * 100 - 50;

      new Zdog.Shape({
        addTo: starGroup,
        stroke: Math.random() * 0.3 + 0.1,
        color: `hsla(0, 0%, 100%, ${Math.random() * 9.9 + 0.1})`,
        translate: {
          x,
          y,
          z,
        },
      });

      // Counter balance group z-index
      new Zdog.Shape({
        addTo: starGroup,
        visible: false,
        translate: {
          x: -x,
          y: -y,
          z: -z,
        },
      });
    }

    // frontside earth
    head = new Zdog.Shape({
      addTo: illo,
      stroke: 40,
      color: "#1976B5",
    });

    // light green lands
    const land1 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -2, y: 0, z: 5 },
      ],
      translate: { x: 17, y: 5, z: 4 },
      color: "#6FCC50",
      closed: false,
      fill: true,
      stroke: 7,
      addTo: head,
    });
    let land1Copy = land1.copy({
      scale: { x: -1 },
      translate: { x: -17, y: 0, z: 4 },
    });
    allShapes.push(land1);
    allShapes.push(land1Copy);

    const land2 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -10, y: 0, z: 4 },
      ],
      translate: { x: 13, y: -10, z: 10 },
      color: "#6FCC50",
      closed: false,
      fill: true,
      stroke: 7,
      addTo: head,
    });
    allShapes.push(land2);

    const land8 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: 5, y: 0, z: 4 },
      ],
      translate: { x: -13, y: -10, z: 10 },
      color: "#6FCC50",
      closed: false,
      fill: true,
      stroke: 7,
      addTo: head,
    });
    allShapes.push(land8);

    const land3 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -2, y: 0, z: 3 },
      ],
      translate: { x: 13, y: 13, z: 7 },
      color: "#6FCC50",
      closed: false,
      fill: true,
      stroke: 4,
      addTo: head,
    });
    let bottomLand3 = land3.copy({
      scale: { x: -1 },
      translate: { x: -13, y: 13, z: 7 },
    });
    bottomShapes.push(land3);
    bottomShapes.push(bottomLand3);
    allShapes.push(land3);
    allShapes.push(bottomLand3);

    const land4 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -7, y: 0, z: 3 },
      ],
      translate: { x: 10, y: -3, z: 16 },
      color: "#6FCC50",
      closed: false,
      fill: true,
      stroke: 4,
      addTo: head,
    });
    const land4Copy = land4.copy({
      scale: { x: -1 },
      translate: { x: -10, y: 3, z: 16 },
    });
    allShapes.push(land4);
    allShapes.push(land4Copy);

    // light clouds
    const cloud1 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -2, y: 0, z: 5 },
      ],
      translate: { x: 20, y: 7, z: 4 },
      color: "#fff",
      closed: false,
      fill: true,
      stroke: 5,
      addTo: head,
    });
    allShapes.push(cloud1);

    const cloud8 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: 3, y: 0, z: 1 },
      ],
      translate: { x: -5, y: 7, z: 20 },
      color: "#fff",
      closed: false,
      fill: true,
      stroke: 5,
      addTo: head,
    });
    allShapes.push(cloud8);

    const cloud2 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -5, y: 0, z: 10 },
      ],
      translate: { x: 23, y: 2, z: 4 },
      color: "#fff",
      closed: false,
      fill: true,
      stroke: 4,
      addTo: head,
    });
    const cloud2Copy = cloud2.copy({
      scale: { x: -1 },
      translate: { x: -23, y: 2, z: 4 },
    });
    allShapes.push(cloud2);
    allShapes.push(cloud2Copy);

    const cloud3 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -2, y: 0, z: 5 },
      ],
      translate: { x: 20, y: -2, z: 7 },
      color: "#fff",
      closed: false,
      fill: true,
      stroke: 2,
      addTo: head,
    });
    const cloud3Copy = cloud3.copy({
      scale: { x: -1 },
      translate: { x: -20, y: -2, z: 7 },
    });
    allShapes.push(cloud3);
    allShapes.push(cloud3Copy);

    const cloud4 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -2, y: 0, z: 5 },
      ],
      translate: { x: 18, y: -10, z: 7 },
      color: "#fff",
      closed: false,
      fill: true,
      stroke: 3,
      addTo: head,
    });
    const cloud4Copy = cloud4.copy({
      scale: { x: -1 },
      translate: { x: -18, y: -10, z: 7 },
    });
    allShapes.push(cloud4);
    allShapes.push(cloud4Copy);

    // dark clouds
    const cloud5 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: 0, y: 0, z: -3 },
      ],
      translate: { x: 20, y: -8, z: -3 },
      color: "#A6B3DA",
      closed: false,
      fill: true,
      stroke: 3,
      addTo: head,
    });
    const cloud5Copy = cloud5.copy({
      scale: { x: -1 },
      translate: { x: -20, y: -8, z: -3 },
    });
    allShapes.push(cloud5);
    allShapes.push(cloud5Copy);

    const cloud6 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -4, y: 0, z: -7 },
      ],
      translate: { x: 20, y: 0, z: -8 },
      color: "#A6B3DA",
      closed: false,
      fill: true,
      stroke: 6,
      addTo: head,
    });
    const cloud6Copy = cloud6.copy({
      scale: { x: -1 },
      translate: { x: -20, y: 0, z: -8 },
    });
    allShapes.push(cloud6);
    allShapes.push(cloud6Copy);

    const cloud7 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -3, y: 0, z: -3 },
      ],
      translate: { x: 15, y: 15, z: -8 },
      color: "#A6B3DA",
      closed: false,
      fill: true,
      stroke: 3,
      addTo: head,
    });
    bottomShapes.push(cloud7);
    allShapes.push(cloud7);

    let bottomCloud7 = cloud7.copy({
      scale: { x: -1 },
      translate: { x: -15, y: 15, z: -8 },
    });
    bottomShapes.push(bottomCloud7);
    allShapes.push(bottomCloud7);

    const cloud9 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: -5, y: 0, z: 0 },
      ],
      translate: { x: -5, y: -4, z: -18 },
      color: "#A6B3DA",
      fill: true,
      stroke: 8,
      addTo: head,
    });
    allShapes.push(cloud9);

    // dark green lands
    const land5 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: 2.5, y: 0, z: 2 },
      ],
      translate: { x: 7, y: 7, z: -16 },
      color: "#489268",
      fill: true,
      stroke: 4,
      addTo: head,
    });
    allShapes.push(land5);

    let bottomLand5 = land5.copy({
      scale: { x: -1 },
      translate: { x: -7, y: 12, z: -16 },
    });
    bottomShapes.push(bottomLand5);
    allShapes.push(bottomLand5);

    const land6 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: 10, y: 0, z: 9 },
      ],
      translate: { x: 5, y: -2, z: -17 },
      color: "#489268",
      fill: true,
      stroke: 8,
      addTo: head,
    });
    allShapes.push(land6);

    const land7 = new Zdog.Shape({
      path: [
        { x: 0, y: 0, z: 0 },
        { x: 2.5, y: 0, z: 5 },
      ],
      translate: { x: 13, y: -10, z: -10 },
      color: "#489268",
      fill: true,
      stroke: 4,
      addTo: head,
    });
    allShapes.push(land7);

    const otherLand = land5.copy({
      scale: { x: -1 },
      translate: { x: -13, y: -10, z: -10 },
      stroke: 5,
    });
    allShapes.push(otherLand);

    for (let shape of allShapes) {
      shapeColors.push(shape.color);
    }
  }

  // spinning animation
  function animate() {
    head.rotate.y += isSpinning ? rotationSpeed : 0;
    starGroup.rotate.y -= 0.0001;
    illo.updateRenderGraph();
    requestAnimationFrame(animate);
  }

  onMount(() => {
    drawEarth();

    animate();

    scrollEarth();
  });
</script>

<svelte:window bind:scrollY bind:innerHeight bind:innerWidth />

<section class="h-[100vh] w-full relative block bg-gray-800">
  <canvas class="illo absolute z-0 m-auto right-0 left-0 touch-auto" />

  <div
    id="header-text"
    class="absolute bottom-[30px] sm:bottom-[5vw] left-[6vw] text-white"
  >
    <h2 class="text-[10vw] sm:text-[6vw]">Own the</h2>
    <h1 class="text-[11vw] sm:text-[7vw]">Energy Transition</h1>
  </div>
</section>

<style>
</style>
