<script>
    import { fly } from 'svelte/transition';
	import { expoInOut } from 'svelte/easing';
    import { count, zindex } from './store.js';
    import { slides } from './slider.js';

    import Logo from './Logo.svelte';

    let showMenu = false;
    let checkBox;

    let m = { x: 0, y: 0 };
    let image = "";
    let menuImg = "";
    let menuItem = [];

    function declareImage() {
        image = document.getElementById("menu-image");
    }

    function mouseOverMenu(num) {
        menuImg = `/img/printscreen-0${num}.jpg`;
        menuItem[num - 1].style.zIndex = 9998;
        menuItem[num - 1].style.color = "rgba(255, 255, 255, 1)"
        image.style.transform = "translateX(-50%) translateY(-50%) scale(1)";
    }
    function mouseMoveMenu(e) {
        m.x = e.clientX;
		m.y = e.clientY;

        image.style.top = e.clientY + 'px';
        image.style.left = e.clientX + 'px';
    }
    function mouseLeaveMenu(num) {
        menuItem[num - 1].style.color = "rgba(255, 255, 255, 0.6)"
        image.style.transform = "translateX(-50%) translateY(-50%) scale(0)";
        menuImg = "";
        menuItem[num - 1].style.zIndex = 1;
    }

    function goToSlide(num) {
        showMenu = !showMenu;
        zindex.update(n => n + 1);
		count.set(num);
        checkBox.checked = false;
	}

    
</script>

{#if showMenu}
    <div class="menu" transition:fly={{y: "-100vh", opacity: 1, easing: expoInOut, duration: 800}} on:introend="{declareImage}">
        <img id="menu-image" src="{menuImg}">
        <ul>
            {#each slides as slide, i}
                <li class="item"  bind:this={menuItem[i]} 
                    in:fly={{y: 50, delay: 180, easing: expoInOut, duration: 800}} 
                    out:fly={{y: -50, easing: expoInOut, duration: 600}} 
                    on:click={() => {goToSlide(i + 1)}} 
                    on:mouseover={() => { mouseOverMenu(i + 1) }} 
                    on:mousemove={mouseMoveMenu} 
                    on:mouseleave={() => {mouseLeaveMenu(i + 1)}}>
                    {slide.title}
                </li>
            {/each}
        </ul>
    </div>
{/if}
<header in:fly={{y: -300, easing: expoInOut, duration: 1200 }} >
    <div class="logo">
        <Logo />
    </div>
    <div class="burger">
        <input bind:this={checkBox} class="menu-trigger hidden" id="togglenav" type="checkbox" on:click={() => showMenu = !showMenu}/>
        <label class="burger-wrapper" for="togglenav">
            <div class="hamburger"></div>
        </label>
    </div>
</header>

<style>
    #menu-image {
        position: absolute;
        width: 40%;
        object-fit: contain;
        transform: translateX(-50%) translateY(-50%) scale(0);
        top: 50%;
        left: 50%;
        z-index: 10;
        transition: transform 200ms ease-in-out;
    }
    .img-animation {
        transform: translateX(-50%) translateY(-50%) scale(0) !important;
    }
    .menu {
        width: 100vw;
        height: 100vh;
        position: absolute;
        z-index: 998;
        background: rgba(0, 0, 0, 0.9);
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .menu ul {
        list-style-type: none;
        margin: 0;
        padding: 0;
        position: relative;
    }
    .menu ul li {
        color: rgba(255, 255, 255, 0.6);
        text-align: center;
        font-size: clamp(24px, calc(1.5rem + ((1vw - 4px) * 6.2222)), 80px);
        min-height: 0vw;
        font-weight: 700;
        margin-bottom: 10px;
        position: relative;
        cursor: pointer;
    }
	header {
		position: absolute;
		width: 100vw;
		display: flex;
		justify-content: space-between;
		z-index: 999;
        margin: 40px 0;
        left: 50%;
        transform: translateX(-50%);
	}
    .logo {
        width: 50px;
        opacity: 0.5;
        margin-left: 20px;
    }
    img {
        width: 100%;
    }
    .burger {
        width: 40px;
        height: 40px;
        margin-right: 20px;
    }
    .burger-wrapper {
        cursor: pointer;
        margin: auto;
        width: 40px;
        height: 40px;
        display: flex;
    }
    .burger-wrapper .hamburger {
        background: rgba(255, 255, 255, 0.6);
        width: 40px;
        height: 4px;
        position: relative;
        transition: background 10ms 300ms ease;
        transform: translateY(20px);
    }
    .burger-wrapper .hamburger:before, .burger-wrapper .hamburger:after {
        transition: top 300ms 350ms ease, transform 300ms 50ms ease;
        position: absolute;
        background: rgba(255, 255, 255, 0.6);
        width: 40px;
        height: 4px;
        content: "";
    }
    .burger-wrapper .hamburger:before {
        top: -10px;
    }
    .burger-wrapper .hamburger:after {
        top: 10px;
    }

    .menu-trigger:checked ~ .burger-wrapper .hamburger {
        background: transparent;
    }
    .menu-trigger:checked ~ .burger-wrapper .hamburger:after, .menu-trigger:checked ~ .burger-wrapper .hamburger:before {
        transition: top 300ms 50ms ease, transform 300ms 350ms ease;
        top: 0;
    }
    .menu-trigger:checked ~ .burger-wrapper .hamburger:before {
        transform: rotate(45deg);
        background: rgba(255, 255, 255, 1);
    }
    .menu-trigger:checked ~ .burger-wrapper .hamburger:after {
        transform: rotate(-45deg);
        background: rgba(255, 255, 255, 1);
    }
    .hidden {
        display: none;
    }

    @media screen and (min-width: 1300px) {
        .logo {
            margin-left: 80px;
        }
        .burger {
            margin-right: 80px;
        }
    }
</style>
