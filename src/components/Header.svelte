<header class="flex fixed w-full top-0 left-1/2 transform -translate-x-1/2 z-50 md:backdrop-blur">
    <div class="flex justify-between items-center py-3 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
        <a href="#about" onclick={(e) => handleClick(e, 'about')} class="text-2xl font-bold relative" data-aos="fade-right" data-aos-delay="200" aria-label="logo">
            <span class="logo w-8 h-8 top-1 md:w-10 md:h-10 inline-block bg-white hover:bg-(--ui-primary) relative z-20"></span>
            <div class="absolute rounded-full -top-30 left-1/2 transform -translate-x-1/2 size-[300px] z-10 bg-(--ui-primary) opacity-90 blur-[200px]"></div>
        </a>

        <!-- Hamburger Menu Button -->
        <button
            class="md:hidden text-2xl focus:outline-none z-60 flex justify-center items-center"
            aria-label="Toggle navigation"
            onclick={() => (isMobileNavOpen = !isMobileNavOpen)}
            data-aos="fade-left" data-aos-delay="200"
        >
            {#if isMobileNavOpen}
                <XIcon size="30" />
            {:else}
                <AlignRightIcon size="30" />
            {/if}
        </button>

        <!-- Desktop Navigation -->
        <nav id="desktop-nav" class="hidden md:flex" data-aos="fade-left" data-aos-delay="200">
            {#each anchors as anchor}
                <a 
                    href={`#${anchor}`}
                    data-name={anchor}
                    class={anchorClasses(anchor)}
                    onclick={(e) => handleClick(e, anchor)}
                >
                    {anchor}
                </a>
            {/each}
        </nav>

        <!-- Mobile Navigation -->
        {#if isMobileNavOpen}
            <div
                id="mobile-nav"
                class="fixed top-14 right-8 bg-opacity-80 flex flex-col items-center justify-center z-50"
            >
                {#each anchors as anchor}
                    <a
                        href={`#${anchor}`}
                        data-name={anchor}
                        class={anchorClasses(anchor, true)}
                        onclick={(e) => handleClick(e, anchor)}
                    >
                        {anchor}
                    </a>
                {/each}
            </div>
        {/if}
    </div>
</header>

<script>
    import { XIcon, AlignRightIcon } from '@lucide/svelte';

    let observer;
    let isMobileNavOpen = $state(false);
    let activeAnchor = $state('about');

    const anchors = ['about', 'services', 'projects', 'contact'];
    const defaultClasses = {
        desktop: 'text-lg font-semibold mx-4 hover:text-(--ui-primary) transition-colors duration-300',
        mobile: 'text-2xl bg-black rounded-full px-5 py-2 m-4 text-white hover:bg-(--ui-primary) transition-colors duration-300',
        
    };
    const activeClasses = {
        desktop: 'active bg-gradient-to-r from-(--ui-primary) to-indigo-600 bg-clip-text text-transparent',
        mobile: 'active bg-gradient-to-r from-(--ui-primary) to-indigo-600',   
    };

    const anchorClasses = (anchor, isMobile = false) => {
        return activeAnchor === anchor ? `${getClass(defaultClasses, isMobile)} ${getClass(activeClasses, isMobile)}` : getClass(defaultClasses, isMobile);
    };

    const getClass = (classes, isMobile) => {
        return isMobile ? classes.mobile : classes.desktop;
    };

    $effect(() => {
        observer = new IntersectionObserver(handleIntersection, {
            root: null, // Viewport
            threshold: 0.6, // 60% of section must be visible
        });

        document.querySelectorAll("section").forEach((section) => {
            observer.observe(section);
        });

        return () => {
            if (observer) observer.disconnect();
        };
    });

    function handleClick(e, name) {
        if (typeof name !== 'undefined') {
            addActiveToNav(name);
            isMobileNavOpen = false;
        }
    }

    function addActiveToNav(name) {
        activeAnchor = name;
    }

    function handleIntersection(entries) {
        entries.forEach((entry) => {
            if (entry.isIntersecting) {
                addActiveToNav(entry.target.id);
            }
        });
    }
</script>