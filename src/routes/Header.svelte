<script>
  import { onMount } from "svelte";

  var illo;
  var head;

  let bottomShapes = [];
  let bottomRemoved = false;

  // boolean flag for spin
  let isSpinning = true;

  let initZoom;

  onMount(() => {
    drawEarth();

    document.body.onscroll = function () {
      let t = document.body.getBoundingClientRect().top;

      if (Math.abs(t) > window.innerHeight) {
        return;
      }

      let zoomScale = 1 + (Math.abs(t) / window.innerHeight) * 2.55;

      // console.log(zoomScale);

      if (zoomScale > 1.2) {
        for (let bottomShape of bottomShapes) {
          bottomShape.remove();
        }
        bottomRemoved = true;
      } else if (bottomRemoved) {
        for (let bottomShape of bottomShapes) {
          head.addChild(bottomShape);
        }
        bottomRemoved = false;
      }

      illo.zoom = initZoom * zoomScale;

      let xScale = (Math.abs(t) / window.innerHeight) * 10;
      let yScale = (Math.abs(t) / window.innerHeight) * 34;

      illo.translate = { x: 10 - xScale, y: -30 + yScale };

      // console.log(illo);
    };

    window.$(window).resize(resizeNav);

    function resizeNav() {
      drawEarth();
    }

    function drawEarth() {
      // setting up Zdog illustration element
      const illoElem = document.querySelector(".illo");
      const illoSize = 50;
      const minWindowSize = Math.min(
        window.innerWidth - 20,
        window.innerHeight - 60
      );
      initZoom = Math.floor((minWindowSize / illoSize) * 1);
      illoElem.setAttribute("width", window.innerWidth);
      illoElem.setAttribute("height", window.innerHeight * 2 - 40);

      // Zdog math variables
      const TAU = Zdog.TAU;

      // illustration base
      illo = new Zdog.Illustration({
        element: illoElem,
        zoom: initZoom,
        dragRotate: true,
        onDragStart: function () {
          isSpinning = false;
        },
        rotate: { y: TAU / 4 },
        translate: { x: 10, y: -30 },
      });

      // frontside earth
      head = new Zdog.Hemisphere({
        addTo: illo,
        diameter: 40,
        stroke: false,
        color: "#1976B5",
      });

      // backside earth
      new Zdog.Hemisphere({
        addTo: head,
        diameter: 40,
        stroke: false,
        color: "#1976B5",
        rotate: { x: TAU / 2 },
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
      land1.copy({
        scale: { x: -1 },
        translate: { x: -17, y: 0, z: 4 },
      });

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

      new Zdog.Shape({
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
      land4.copy({
        scale: { x: -1 },
        translate: { x: -10, y: 3, z: 16 },
      });

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

      new Zdog.Shape({
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
      cloud2.copy({
        scale: { x: -1 },
        translate: { x: -23, y: 2, z: 4 },
      });

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
      cloud3.copy({
        scale: { x: -1 },
        translate: { x: -20, y: -2, z: 7 },
      });

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
      cloud4.copy({
        scale: { x: -1 },
        translate: { x: -18, y: -10, z: 7 },
      });

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
      cloud5.copy({
        scale: { x: -1 },
        translate: { x: -20, y: -8, z: -3 },
      });

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
      cloud6.copy({
        scale: { x: -1 },
        translate: { x: -20, y: 0, z: -8 },
      });

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

      let bottomCloud7 = cloud7.copy({
        scale: { x: -1 },
        translate: { x: -15, y: 15, z: -8 },
      });
      bottomShapes.push(bottomCloud7);

      new Zdog.Shape({
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

      let bottomLand5 = land5.copy({
        scale: { x: -1 },
        translate: { x: -7, y: 12, z: -16 },
      });
      bottomShapes.push(bottomLand5);

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
      land5.copy({
        scale: { x: -1 },
        translate: { x: -13, y: -10, z: -10 },
        stroke: 5,
      });

      animate();
    }

    // spinning animation
    function animate() {
      illo.rotate.y += isSpinning ? -0.01 : 0;
      illo.updateRenderGraph();
      requestAnimationFrame(animate);
    }
  });
</script>

<section class="h-[100vh] mt-[-68px] w-full relative block bg-gray-800">
  <canvas class="illo absolute z-0 m-auto right-0 left-0" />

  <div class="absolute bottom-[5vw] left-[6vw] text-white">
    <h2 class="text-[6vw]">Own the</h2>
    <h1 class="text-[7vw]">Energy Transition</h1>
  </div>
</section>

<style>
  .illo {
    cursor: move;
  }
</style>
