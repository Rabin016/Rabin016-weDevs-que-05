<template>
    <div
        id="app"
        class="font-mon bg-gray-200 text-gray-800 font-medium min-h-screen transition-all ease-in-out duration-300 pb-5 relative"
    >
        <Navbar />
        <transition
            mode="out-in"
            appear
            :css="false"
            @before-enter="beforeEnter"
            @enter="enter"
            @leave="leave"
        >
            <router-view
                :all-categories="allCategories"
                @reloadCategory="loadCategories()"
            />
        </transition>
    </div>
</template>

<script>
import Navbar from "./components/Navbar";
import gsap from "gsap";

export default {
    name: "App",
    components: {
        Navbar,
    },
    data: () => ({
        allCategories: null,
    }),
    mounted() {
        this.loadCategories();
    },
    methods: {
        async loadCategories() {
            try {
                if (localStorage.allCategories) {
                    this.allCategories = await JSON.parse(
                        localStorage.allCategories
                    );
                }
            } catch (err) {
                if (err) {
                    throw err;
                }
            }
        },
        beforeEnter(el) {
            gsap.from(el, { opacity: 0 });
        },
        enter(el, done) {
            gsap.to(el, {
                duration: 0.5,
                opacity: 1,
                ease: "power2out",
                onComplete: done,
            });
        },
        leave(el, done) {
            gsap.to(el, {
                duration: 0.5,
                opacity: 0,
                ease: "power2out",
                onComplete: done,
            });
        },
    },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap");

.font-mon {
    font-family: "Montserrat", sans-serif;
}
.absolute-center2 {
    left: 50%;
    top: 0;
    transform: translateX(-50%);

    margin-top: 2rem;
    margin-left: 0.3rem;
}
.absolute-center3 {
    left: 25%;
    top: 50%;
    transform: translateX(-50%);

    margin-top: 1.5rem;
}
</style>
