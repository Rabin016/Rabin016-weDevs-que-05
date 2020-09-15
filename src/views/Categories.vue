<template>
    <div class="w-10/12 md:w-8/12 mx-auto bg-gray-100 p-5 rounded-xl">
        <Alart v-if="alartMsg" :alart-msg="alartMsg" />
        <div class="flex justify-between items-center">
            <h2 class="text-lg font-semibold">List of Categories</h2>
            <div class="flex relative">
                <button
                    class="flex items-center px-3 py-2 mr-5 bg-indigo-600 text-gray-200 font-semibold rounded-xl justify-between hover:bg-indigo-800 transition-all ease-in-out duration-300"
                    @click.prevent="show = !show"
                >
                    <svg
                        viewBox="0 0 20 20"
                        fill="currentColor"
                        class="w-6 h-6"
                    >
                        <path
                            fill-rule="evenodd"
                            d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z"
                            clip-rule="evenodd"
                        ></path></svg
                    ><span class="px-1 hidden md:inline-block"
                        >Add category</span
                    >
                </button>
                <transition
                    appear
                    :css="false"
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @leave="leave"
                >
                    <Addcategory
                        v-if="show"
                        class="absolute absolute-center3"
                        @closeCategories="closeCategories"
                        @reloadCategory="reloadCategory()"
                    />
                </transition>
                <router-link
                    to="/"
                    class="flex items-center px-3 py-2 bg-indigo-600 text-gray-200 no-underline font-semibold rounded-xl justify-between hover:bg-indigo-800 transition-all ease-in-out duration-300"
                >
                    <svg
                        viewBox="0 0 20 20"
                        fill="currentColor"
                        class="view-list w-6 h-6"
                    >
                        <path
                            fill-rule="evenodd"
                            d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
                            clip-rule="evenodd"
                        ></path></svg
                    ><span class="px-1 hidden md:inline-block">View Posts</span>
                </router-link>
            </div>
        </div>
        <Categorylist
            :all-categories="allCategories"
            @triggerDelCategory="deleteCategory($event)"
        />
    </div>
</template>

<script>
import Categorylist from "../components/Categorylist";
import Alart from "../components/Alart";
import Addcategory from "../components/Addcategory";
import gsap from "gsap";

export default {
    name: "Categories",
    components: {
        Categorylist,
        Alart,
        Addcategory,
    },
    props: {
        // eslint-disable-next-line
        allCategories: Array,
    },
    data: () => ({
        show: false,
        alartMsg: null,
    }),
    methods: {
        async deleteCategory(id) {
            try {
                const newSetCategories = await this.allCategories.filter(
                    item => id != item
                );
                localStorage.allCategories = JSON.stringify(newSetCategories);
                this.$emit("reloadCategory");
                this.clearAlart();
            } catch (err) {
                this.alartMsg = { dangerMsg: "Post not deleted" };
            }
        },
        clearAlart() {
            setTimeout(() => (this.alartMsg = null), 3000);
        },
        closeCategories() {
            this.show = null;
        },
        reloadCategory() {
            this.$emit("reloadCategory");
        },
        beforeEnter(el) {
            el.style.opacity = 0;
            el.style.transform = "translate(-50%, 10px)";
        },
        enter(el, done) {
            gsap.to(el, {
                duration: 0.5,
                opacity: 1,
                y: 0,
                ease: "power2out",
                onComplete: done,
            });
        },
        leave(el, done) {
            gsap.to(el, {
                duration: 0.5,
                opacity: 0,
                y: 10,
                ease: "power2out",
                onComplete: done,
            });
        },
    },
};
</script>

<style></style>
