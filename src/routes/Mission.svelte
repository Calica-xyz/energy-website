<script>
  import { onMount } from "svelte";

  onMount(() => {
    const illoElem = document.querySelector(".zdog-canvas");

    let parent = illoElem.parentNode.getBoundingClientRect();

    illoElem.setAttribute("width", parent.width);
    illoElem.setAttribute("height", parent.width);

    const TAU = Zdog.TAU;
    let zoom = (parent.width - 300) * 0.001 + 0.45;

    let illo = new Zdog.Illustration({
      element: ".zdog-canvas",
      dragRotate: false,
      rotate: { x: TAU / -40 },
      zoom,
    });
    let windmill = new Zdog.Group({
      addTo: illo,
      updateSort: true,
      translate: { y: -140, z: 50 },
    });
    let bladeGroup = new Zdog.Group({
      addTo: windmill,
      updateSort: true,
    });

    let box = new Zdog.Cylinder({
      addTo: bladeGroup,
      length: 30,
      diameter: 20,
      translate: { x: 0, y: 0, z: -10 },
      stroke: 1,
      color: "#EA0",
      frontface: "#c1c1c1",
      fill: true,
    });

    let blade = new Zdog.Group({
      addTo: bladeGroup,
      updateSort: true,
    });
    let bladeTop = new Zdog.Rect({
      addTo: blade,
      width: 40,
      translate: { y: -80, x: 25, z: -8 },
      rotate: { y: TAU / -20 },
      height: 100,
      stroke: 5,
      fill: true,
      color: "#ffcb86",
    });
    let brace = new Zdog.Shape({
      addTo: bladeTop,
      path: [
        { x: -5, y: -50 },
        { x: -5, y: 70 },
      ],
      translate: { x: -20 },
      stroke: 5,
      color: "#a34c13",
    });

    blade.copyGraph({
      rotate: { z: TAU / 4 },
    });

    blade.copyGraph({
      rotate: { z: TAU / 2 },
    });

    blade.copyGraph({
      rotate: { z: TAU / -4 },
    });

    let dome = new Zdog.Hemisphere({
      addTo: windmill,
      diameter: 120,
      translate: { z: -85, y: -25 },
      rotate: { x: TAU / 4 },
      stroke: false,
      color: "#eb6444",
      backface: "#C25",
    });

    let base = new Zdog.Cylinder({
      addTo: windmill,
      length: 150,
      diameter: 120,
      translate: { z: -85, y: 60 },
      rotate: { x: TAU / 4 },
      stroke: 1,
      color: "#c1c1c1",
      backface: "#b1b1b1",
      fill: true,
    });
    let circle = new Zdog.Ellipse({
      addTo: windmill,
      diameter: 120,
      stroke: 10,
      color: "#fbfbfb",
      translate: { z: -85, y: -20 },
      rotate: { x: TAU / 4 },
    });
    let circleBottom = new Zdog.Ellipse({
      addTo: windmill,
      diameter: 130,
      stroke: 30,
      color: "#fbfbfb",
      translate: { z: -85, y: 150 },
      rotate: { x: TAU / 4 },
    });
    let baseBottom = new Zdog.Cylinder({
      addTo: windmill,
      length: 80,
      diameter: 155,
      translate: { z: -85, y: 205 },
      rotate: { x: TAU / 4 },
      stroke: 1,
      color: "#c1c1c1",
      backface: "#b1b1b1",
      fill: true,
    });

    let island = new Zdog.Hemisphere({
      addTo: windmill,
      diameter: 400,
      stroke: false,
      color: "#fb8f72",
      backface: "#a7ddb8",
      rotate: { x: TAU / -4 },
      translate: { z: -85, y: 260 },
    });
    let circleIsland = new Zdog.Ellipse({
      addTo: windmill,
      diameter: 405,
      stroke: 20,
      color: "#a7ddb8",
      fill: true,
      translate: { z: -85, y: 250 },
      rotate: { x: TAU / 4 },
    });

    /* cloude */
    let clouds = new Zdog.Group({
      addTo: windmill,
      translate: { z: -100, y: -110, x: -100 },
    });
    let cloud = new Zdog.Shape({
      addTo: clouds,
      stroke: 60,
      color: "#fff",
    });
    cloud.copy({
      translate: { x: 40 },
      stroke: 20,
    });
    cloud.copy({
      translate: { x: -50 },
      stroke: 80,
    });

    function animate() {
      bladeGroup.rotate.z -= 0.02;
      //illo.rotate.y -= 0.01;
      illo.updateRenderGraph();
      requestAnimationFrame(animate);
    }
    animate();
  });
</script>

<section
  id="mission"
  class="flex flex-col justify-center text-white min-h-[100vh]"
>
  <div
    class="p-10 py-20 sm:p-20 relative flex flex-wrap justify-center items-center gap-14 h-full"
  >
    <div class="flex-1 text-md sm:text-xl flex flex-col gap-14 max-w-[800px]">
      <h2 class="text-5xl sm:text-7xl">Our Mission</h2>
      <p>
        We're empowering the next generation of clean energy consumption. We
        avoid greenwashing at all costs and incentivize real renewable
        development.
        <br /><br />
        By leveraging the power of blockchain and smart contracts, we're democratizing
        the clean energy market. For the first time, consumers can establish a direct
        relationship with clean energy suppliers.
      </p>
    </div>

    <div
      class="flex-1 grid justify-center content-center max-w-[500px] max-h-[500px] min-w-[300px] min-h-[300px]"
    >
      <canvas class="zdog-canvas" />
    </div>
  </div>
</section>

<style>
  section {
    background: linear-gradient(
      to bottom,
      rgb(31 41 55) 0%,
      rgb(31 41 55) 100vh,
      #1976b5 100vh,
      #1976b5 100%
    );
  }
</style>
