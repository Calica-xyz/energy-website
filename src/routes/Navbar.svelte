<script>
  import { onMount } from "svelte";

  let scrollY;

  $: if (scrollY > 100 && window.$(".nav-links").hasClass("open")) {
    window.$(".hamburger").click();
  }

  onMount(() => {
    window.$(".hamburger").click(function () {
      window.$(".nav-links").toggleClass("open");
      window.$(".nav-links li").toggleClass("fade");
    });
  });
</script>

<svelte:window bind:scrollY />

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
    <div class="hamburger">
      <div class="line" />
      <div class="line" />
      <div class="line" />
    </div>
    <ul class="nav-links">
      <li><a href="#mission">Mission</a></li>
      <li><a href="#service">Service</a></li>
      <li><a href="#signup">Sign Up</a></li>
    </ul>

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
</nav>

<style>
  .nav-links {
    visibility: hidden;
  }
  @media screen and (max-width: 768px) {
    :global(.line) {
      width: 30px;
      height: 3px;
      background: white;
      margin: 5px;
    }

    :global(.hamburger) {
      position: absolute;
      cursor: pointer;
      right: 5%;
      top: 50%;
      transform: translate(-5%, -50%);
      z-index: 2;
    }

    :global(.nav-links) {
      position: fixed;
      background: #5b78c7;
      visibility: hidden;
      bottom: 0;
      right: 0;
      top: 0;
      left: 0;
      flex-direction: column;
      clip-path: circle(10px at 83% 7%);
      -webkit-clip-path: circle(10px at 83% 7%);
      transition: all 1s ease-out;
      pointer-events: none;
    }
    :global(.nav-links.open) {
      clip-path: circle(1000px at 90% -10%);
      -webkit-clip-path: circle(1000px at 90% -10%);
      pointer-events: all;
      visibility: visible;
    }
    :global(.nav-links li) {
      opacity: 0;
    }
    :global(.nav-links li a) {
      font-size: 25px;
      color: white;
    }
    :global(.nav-links li:nth-child(1)) {
      transition: all 0.5s ease 0.2s;
    }
    :global(.nav-links li:nth-child(2)) {
      transition: all 0.5s ease 0.4s;
    }
    :global(.nav-links li:nth-child(3)) {
      transition: all 0.5s ease 0.6s;
    }
    :global(li.fade) {
      opacity: 1;
    }
  }
</style>
