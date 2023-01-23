<script>
  import { onMount } from "svelte";

  onMount(() => {
    // Set up click and window resize callbacks, then init the nav.
    window.$(document).ready(function () {
      window.$("#nav-toggle").click(function () {
        window
          .$("#nav-toggle, #nav-overlay, #nav-fullscreen")
          .toggleClass("open");
      });

      window.$(window).resize(resizeNav);

      resizeNav();

      //   window.setTimeout(function () {
      //     window.$("#nav-toggle").click();
      //   }, 1000);
    });

    function resizeNav() {
      console.log(window.innerHeight, window.innerWidth);
      // Set the nav height to fill the window
      window.$("#nav-fullscreen").css({ height: window.innerHeight });

      // Set the circle radius to the length of the window diagonal,
      // this way we're only making the circle as big as it needs to be.
      var radius = Math.sqrt(
        Math.pow(window.innerHeight, 2) + Math.pow(window.innerWidth, 2)
      );

      var diameter = radius * 2;
      window.$("#nav-overlay").width(diameter);
      window.$("#nav-overlay").height(diameter);
      window
        .$("#nav-overlay")
        .css({ "margin-top": -radius, "margin-left": -radius });
    }

    document.body.onscroll = function () {
      let t = document.body.getBoundingClientRect().top;

      if (t < -100 && window.$("#nav-fullscreen").hasClass("open")) {
        window.$("#nav-toggle").click();
      }
    };
  });
</script>

<div id="nav-container">
  <div id="nav-overlay" />
  <nav id="nav-fullscreen">
    <ul>
      <li><a href="#">Home</a></li>
      <li><a href="#">Blog</a></li>
      <li><a href="#">About</a></li>
      <li><a href="#">Contact</a></li>
    </ul>
  </nav>

  <a id="nav-toggle">
    <span />
    <span />
    <span />
  </a>
</div>

<!-- 
<nav
  class="bg-gray-800 border-gray-200 px-6 top-6 w-full sm:px-8 py-4 rounded absolute"
>
  <div
    class="container flex flex-wrap items-center justify-between mx-auto z-10 relative"
  >
    <a href="https://flowbite.com/" class="flex items-center">
      <img
        class="block h-8 w-auto lg:hidden"
        src="/logo.png"
        alt="Calica Logo"
      />
      <img
        class="hidden h-8 w-auto lg:block"
        src="/logo.png"
        alt="Calica Logo"
      />
      <span class="self-center text-3xl ml-1 font-semibold whitespace-nowrap"
        >Calica</span
      >
    </a>
    <div
      data-collapse-toggle="navbar-default"
      id="nav-container"
      class="z-10 md:hidden"
      aria-controls="navbar-default"
      aria-expanded="false"
    >
      <div id="nav-overlay" />
      <nav id="nav-fullscreen">
        <ul>
          <li><a href="#mission">Mission</a></li>
          <li><a href="#service">Service</a></li>
          <li><a href="#signup">Sign Up</a></li>
        </ul>
      </nav>

      <a id="nav-toggle" class="z-10">
        <span class="bg-black" />
        <span />
        <span />
      </a>
    </div>

    <div class="hidden w-full md:block md:w-auto" id="navbar-default">
      <ul
        class="flex flex-col gap-x-8 p-4 mt-4 border border-gray-100 rounded-lg  md:flex-row md:space-x-8 md:mt-0 md:text-sm md:font-medium md:border-0"
      >
        <li>
          <a
            href="#mission"
            class="block py-2 pl-3 pr-4 text-xl text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 "
            >Mission</a
          >
        </li>
        <li>
          <a
            href="#service"
            class="block py-2 pl-3 pr-4 text-xl text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 "
            >Service</a
          >
        </li>
        <li>
          <a
            href="#signup"
            class="block py-2 pl-3 pr-4 text-xl text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0"
            >Sign Up</a
          >
        </li>
      </ul>
    </div>
  </div>
</nav> -->
<style>
  :global(#nav-container) {
    margin-left: auto;
    position: relative;
  }

  :global(#nav-toggle) {
    display: block;
    width: 32px;
    height: 32px;
    cursor: pointer;
    transform: rotate(0deg);
  }

  :global(#nav-toggle span) {
    background-color: black;
    width: 100%;
    height: 4px;
    border-radius: 1px;
    display: block;
    position: absolute;
    left: 0;
    content: "";
    transition: 0.5s ease-in-out;
  }

  :global(#nav-toggle span:nth-child(1)) {
    top: 4px;
    transform-origin: left center;
  }

  :global(#nav-toggle span:nth-child(2)) {
    top: 14px;
    transform-origin: left center;
  }

  :global(#nav-toggle span:nth-child(3)) {
    top: 24px;
    transform-origin: left center;
  }

  :global(#nav-toggle.open span:nth-child(1)) {
    transform: rotate(45deg);
    top: 3px;
    left: 4px;
  }

  :global(#nav-toggle.open span:nth-child(2)) {
    width: 0%;
    opacity: 0;
  }

  :global(#nav-toggle.open span:nth-child(3)) {
    transform: rotate(-45deg);
    top: 25px;
    left: 4px;
  }

  :global(#nav-overlay) {
    position: absolute;
    top: 16px;
    left: 16px;
    z-index: -1;
    background: rgba(0, 0, 0, 0.8);
    border-radius: 50%;
    transition: 1s;
    transform: scale3d(0, 0, 0);
  }

  :global(#nav-overlay.open) {
    transform: scale3d(1, 1, 1);
  }

  :global(#nav-fullscreen) {
    width: 100%;
    position: fixed;
    left: 0;
    top: 0;
    transition: ease-in-out 0.25s;
    transition-delay: 0s;
    visibility: hidden;
    opacity: 0;
  }
  :global(#nav-fullscreen ul) {
    list-style: none;
  }
  :global(#nav-fullscreen a) {
    color: white;
    text-decoration: none;
  }

  :global(#nav-fullscreen.open) {
    visibility: visible;
    opacity: 1;
    transition: ease-in-out 0.5s;
    transition-delay: 0.25s;
  }

  :global(#nav-overlay) {
    background: linear-gradient(180deg, #ff512f 10%, #ff512f 50%, #dd2476 90%);
  }

  :global(#nav-toggle span) {
    background-color: black;
  }

  :global(#nav-fullscreen) {
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-family: sans-serif;
    font-size: 10vh;
  }
  :global(#nav-fullscreen li:hover) {
    list-style: disc;
  }
</style>
