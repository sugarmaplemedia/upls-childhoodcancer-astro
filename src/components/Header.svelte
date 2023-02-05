<svelte:window bind:innerWidth={windowWidth} />
<header class:navOn={navToggleOn} bind:this={header}>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div 
        class="navToggle"
        class:navToggleOn={navToggleOn}
        id="navToggle"
        bind:this={navToggle}
        on:click={handleNavToggle}
    >
        <div class="navBar"></div>
        <p>CLOSE</p>
        <div class="navBar"></div>
    </div>
    <a href="/" class="menuItem initialItem" on:click={handleMenuItem}>BACK TO UPLS</a>
    <!-- svelte-ignore a11y-click-events-have-key-events --> 
    <div
        id="eventsTab"
        class="menuItem"
        bind:this={eventsTab}
        on:click={handleEventsTab}
        on:mouseenter={handleEventsHover}
        on:mouseleave={handleEventsHover}
    >
        <p class="mobile">EVENTS</p>
        <p class="desktop">↓ EVENTS</p>
        <div
            id="eventsSelector"
            bind:this={eventsSelector}
        ></div>
    </div>
    <a class="logo" href="#top"><img src="/assets/brand/logo-fullcolor.svg" alt="Map of Michigan's Upper Penisula with 'I Trekked the U.P.' text"></a>
    <a href="resources" class="menuItem initialItem" on:click={handleMenuItem}>RESOURCES</a>
    <a href="#donation" class="menuItem initialItem" on:click={handleMenuItem}>DONATE</a>
    <a href="events/itrekkedtheup" class="menuItem eventsItem" on:click={handleMenuItem}>I TREKKED THE UP</a>
    <a href="https://tourdayoopeh.com/childhood-cancer" target="_blank noreferrer" class="menuItem eventsItem" on:click={handleMenuItem}>BIKE 4 KIDS</a>
    <div
        class="eventsMenuDesktop"
        class:eventsMenuDesktopActive={eventsMenuDesktopOn}
        style="top: {eventsMenuDesktopTop}px;"
        on:mouseenter={handleEventsHover}
        on:mouseleave={handleEventsHover}
        bind:this={eventsMenuDesktop}
    >
        <h3>OUR EVENTS</h3>
        <a href="events/itrekkedtheup">I TREKKED THE UP</a>
        <a href="https://tourdayoopeh.com/childhood-cancer" target="_blank noreferrer">BIKE 4 KIDS</a>
        <h3>AFFILIATED EVENTS</h3>
        <a href="events/shf-annual-gala">SHF ANNUAL GALA</a>
        <a href="/childhoodcancer/media/menu/cqusa.pdf" target="_blank">CAMP QUALITY U.P.</a>
        <a href="/childhoodcancer/media/menu/kkc.pdf" target="_blank">KIDS KICKING CANCER</a>
    </div>
</header>

<script>
    import { onMount } from "svelte"
    import anime from "animejs"

    let windowWidth, header, navToggle, eventsTab, eventsSelector, eventsItems, eventsMenuDesktop, scrolly
    let eventsMenuTracker = false
    let navToggleOn = false
    
    let eventsMenuDesktopTop
    $: eventsMenuDesktopTop = header ? header.offsetHeight + 10 : 10
    
    onMount(() => {
        eventsItems = document.getElementsByClassName("eventsItem")
    })

    
    /* Mobile Menu*/
    
    const handleNavToggle = () => {
        if (window.innerWidth < 750) {
            navToggleOn = !navToggleOn
            eventsSelector.css('top', (eventsTab.offsetHeight + 10))

            if (eventsMenuTracker) {
                closeInitiatives()
            }
        }
    }

    const handleMenuItem = () => {
        navToggleOn = false
        checkInitiativesTracker()

        console.log("Hello!");
    }

    const handleEventsTab = () => {
        checkInitiativesTracker()
    }


    /* Functionality */
    
    const openInitiatives = () => {
        eventsMenuTracker = true
        anime({
            targets: '.initialItem',
            translateX: '-80vw',
            duration: 100,
            easing: 'easeOutCirc'
        })
        anime({
            targets: '#eventsSelector',
            rotateY: 180,
            duration: 2000
        })
        anime({
            targets: '.eventsItem',
            translateY: '-22vh',
            opacity: 1,
            duration: 200,
            ease: 'easeOutCirc'
        })
        eventsItems.forEach(item => item.classList.add("eventsItemActive"))
    }

    const closeInitiatives =() => {
        eventsMenuTracker = false
        anime({
            targets: '.initialItem',
            translateX: '0vw',
            duration: 100,
            easing: 'easeInCirc'
        })
        anime({
            targets: '#eventsSelector',
            rotateY: 0,
            duration: 2000
        })
        anime({
            targets: '.eventsItem',
            translateY: '0',
            opacity: 0,
            duration: 200,
            ease: 'easeOutCirc'
        })
        eventsItems.forEach(item => item.classList.remove("eventsItemActive"))
    }

    const checkInitiativesTracker = () => {
        if (!eventsMenuTracker)
            openInitiatives();
        else
            closeInitiatives();
    }

    let eventsMenuDesktopOn = false
    const handleEventsHover = (e) => {
        if (e.type == "mouseenter") {
            eventsMenuDesktopOn = true
            console.log(header)
        } else {
            eventsMenuDesktopOn = false
        }
    }
</script>

<style>
    header {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        padding: 2vw 0;
        height: 10vh;
        min-height: 70px;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        align-items: center;
        justify-content: center;
        background-image: radial-gradient(110% 100% ellipse at 50% 0%, rgba(255, 255, 255,1) 60%, rgba(255, 255, 255,.75) 80%, rgba(255, 255, 255,0) 100%);
        z-index: 99999;
        transition: .5s ease;
    }

    .navOn {
        height: 80vh;
        background-image: radial-gradient(110% 100% ellipse at 50% 0%, rgba(255, 255, 255,1) 80%, rgba(255, 255, 255,.75) 90%, rgba(255, 255, 255,0) 100%);
    }

    .navToggle {
        display: block;
        cursor: pointer;
        width: 40px;
        height: 20px;
        position: absolute;
        top: 25px;
        left: 25px;
        transition: 0.5s ease;
    }

    .navToggleOn {
        height: 80px;
    }

    .navToggle p {
        position: relative;
        top: 50%;
        opacity: 0;
        font-family: var(--fontb);
        font-size: 16pt;
        font-weight: 900;
        color: var(--a);
        transform: translateY(-40%);
        transition: 0.15s ease;
    }

    .navBar {
        position: absolute;
        width: 30px;
        height: 4px;
        border-radius: 1000px;
        background: var(--a);
        z-index: 9999;
    }

    .navBar:first-of-type {
        top: 0;
    }

    .navBar:last-of-type {
        bottom: 0;
    }

    .navToggleOn p {
        opacity: 1;
        transition: .5s .25s ease;
    }

    header .logo {
        position: absolute;
        top: 2vh;
        height: 15vw;
        max-height: 90px;
        margin: 0;
        z-index: 99999;
        transition: .5s ease;
    }

    header .logo:hover {
        transform: scale(1.05);
        transition: .25s cubic-bezier(.19, 1.5, .24, 1);
    }

    header .logo img {
        height: 100%;
    }

    .menuItem {
        position: relative;
        margin: 0;
        height: 0;
        font-family: var(--fontb);
        font-size: 20pt;
        font-weight: 900;
        color: var(--b);
        line-height: 30px;
        text-align: left;
        letter-spacing: -1px;
        z-index: 9999;
        opacity: 0;
        transition: .5s ease;
        pointer-events: none;
        user-select: none;
        text-decoration: none;
    }

    .navOn .menuItem {
        height: 5vh;
        margin: 3vh 0;
        pointer-events: all;
    }

    .navOn .initialItem, .navOn #eventsTab {
        opacity: 1;
    }

    .menuItem:not(:nth-of-type(2)):hover {
        transform: translateY(7px);
        transition: .25s cubic-bezier(.19, 1.5, .24, 1);
    }

    #eventsTab p.desktop {
        display: none;
    }

    #eventsSelector {
        display: block;
        position: absolute;
        width: 87px;
        height: 50px;

        background-image: url("/assets/graphics/menu-arrow.svg");
        background-position: right;
        background-size: 120px 50px;
    }

    .eventsItem {
        display: none;

    }

    .eventsItemActive {
        display: block;
    }

    .eventsMenuDesktop {
        display: none;
    }

    @keyframes introMenu {
        0% {transform: translate(-50%,200%);opacity: 0;}
        50% {transform: translate(-50%,200%);opacity: 0;}
        100% {transform: translate(-50%,0);opacity: 1;}
    }

    .menuIndicator {
        position: absolute;
        left: 50%;
        bottom: 0;
        width: 35px;
        height: 4px;
        border-radius: 1000px;
        background: var(--a);
        opacity: 0;
        z-index: 9999;
        transform: translateX(-50%);
        opacity: 1;
        animation: introMenu 1s ease;
    }

    @keyframes indicate2 {
        0% {transform: translateX(-200%);opacity: 0;}
        50% {transform: translateX(-200%);opacity: 0;}
        100% {transform: translateX(-50%);opacity: 1;}
    }

    #top {
        position: relative;
        height: 20vh;
    }

    @media only screen and (min-width: 750px) {

        /* 750px | HEADER */

        header {
            flex-wrap: nowrap;
            flex-direction: row;
            padding: 10px 0;
        }

        header .logo {
            position: relative;
            margin: 0 10px;
        }

        .navToggle {
            display: none;
        }

        .menuItem {
            opacity: 1;
            font-size: 15pt;
            pointer-events: all;
            line-height: normal;
            height: auto;
            padding: 10px 0;
            margin: 0 2vw;
        }

        .menuItem:nth-of-type(2):hover {
            transform: translateY(7px);
            transition: .25s cubic-bezier(.19, 1.5, .24, 1);
        }

        .menuIndicator {
            bottom: auto;
        }

        #eventsTab p.mobile, #eventsSelector {
            display: none;
        }

        #eventsTab p.desktop {
            display: inline;
        }

        .eventsMenuDesktop {
            display: block;
            position: absolute;
            visibility: hidden;
            opacity: 0;
            width: 300px;
            top: 0;
            margin-right: 380px;

            background-color: white;
            border-radius: 20px;
            box-shadow: 0 10px 20px -5px var(--b);

            transition: 0.1s;
        }

        .eventsMenuDesktopActive {
            visibility: visible;
            opacity: 1;

            transition: 0.1s;
        }

        .eventsMenuDesktop h3 {
            padding: 15px 10px 10px;
            color: var(--a);
            font-family: var(--fonta);
            font-size: 1.5em;
        }

        .eventsMenuDesktop a {
            display: block;
            padding: 5px 15px 5px;

            text-align: right;
            color: var(--b);
            font-family: var(--fontb);
            font-size: 1.25em;
            font-weight: 900;

            transition: 0.1s;
        }

        .eventsMenuDesktop a:not(.inactive):hover {
            font-size: 1.35em;

            transition: 0.1s;
        }

        .eventsMenuDesktop a.inactive {
            user-select: none;
        }


        .eventsMenuDesktop hr {
            height: 2px;
            margin: 0 15px 5px 100px;

            background-color: var(--b);
            border: 0px;
            border-radius: 20px;
        }

        .eventsMenuDesktop a:last-of-type {
            padding-bottom: 20px;
        }
    }
</style>