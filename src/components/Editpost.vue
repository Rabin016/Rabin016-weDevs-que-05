<template>
    <div class="bg-black bg-opacity-75 fixed inset-0 z-10">
        <div class="z-20 bg-white w-10/12 md:w-6/12 mx-auto my-10 rounded-xl">
            <div class="p-5">
                <div class="flex justify-between items-center">
                    <h3 class="font-semibold text-lg">Edit Post</h3>
                    <button
                        class="text-4xl hover:text-indigo-600"
                        @click="closeModal()"
                    >
                        &times;
                    </button>
                </div>
                <form class="py-5" @submit.prevent="submitPost()">
                    <div>
                        <label class="font-semibold text-sm pl-1" for="title"
                            >Title</label
                        >
                        <input
                            v-model="newPost.title"
                            class="py-2 px-4 bg-gray-300 w-full rounded-lg placeholder-gray-700 border-2 border-transparent focus:bg-white focus:border-indigo-600 transition-all ease-in-out duration-300"
                            type="text"
                            placeholder="Title"
                            required
                        />
                    </div>
                    <div class="my-5">
                        <label class="font-semibold text-sm pl-1" for="title"
                            >Description</label
                        >
                        <input
                            v-model="newPost.msg"
                            class="py-2 px-4 bg-gray-300 w-full rounded-lg placeholder-gray-700 border-2 border-transparent focus:bg-white focus:border-indigo-600 transition-all ease-in-out duration-300"
                            name="title"
                            type="text"
                            placeholder="Enter your post description"
                            required
                        />
                    </div>
                    <div class="py-5">
                        <div class="flex items-center">
                            <p>Categories</p>
                            <div class="relative">
                                <button
                                    class="font-bold w-6 h-6 bg-gray-300 rounded-full ml-3 text-center hover:text-indigo-600 hover:bg-indigo-100 transition-all ease-in-out duration-300"
                                    @click.prevent="show = !show"
                                >
                                    &#43;
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
                                        class="absolute absolute-center2"
                                        @closeCategories="closeCategories"
                                        @reloadCategory="reloadCategory()"
                                    />
                                </transition>
                            </div>
                        </div>
                        <div
                            style="max-height:7rem"
                            class="flex items-center flex-wrap overflow-y-auto"
                        >
                            <label
                                v-for="(category, index) in allCategories"
                                :key="index"
                                class="inline-flex items-center m-5"
                            >
                                <input
                                    v-model="newPost.category_id"
                                    type="checkbox"
                                    class="form-checkbox h-5 w-5 text-indigo-600 border-2 border-indigo-600"
                                    :value="category"
                                /><span
                                    class="ml-2 text-gray-700 transition-all ease-in-out duration-300"
                                    >{{ category }}</span
                                >
                            </label>
                        </div>
                    </div>
                    <button
                        class="px-5 py-2 mr-5 bg-indigo-600 text-gray-200 font-semibold rounded-xl justify-between hover:bg-indigo-800 transition-all ease-in-out duration-300"
                        type="submit"
                    >
                        Update
                    </button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import Addcategory from "./Addcategory";
import gsap from "gsap";

export default {
    name: "Editpost",
    components: {
        Addcategory,
    },
    props: {
        // eslint-disable-next-line
        allCategories: Array,
        // eslint-disable-next-line
        editableID: String,
    },
    data: () => ({
        newPost: {
            title: null,
            msg: null,
            category_id: [],
        },
        show: false,
    }),
    async mounted() {
        if (this.editableID) {
            const allPosts = await JSON.parse(localStorage.allPosts);
            const data = allPosts.filter(item => this.editableID == item.title);
            this.newPost.title = data[0].title;
            this.newPost.msg = data[0].msg;
            this.newPost.category_id.push(...data[0].category_id);
        }
    },
    methods: {
        async submitPost() {
            try {
                if (this.newPost.category_id != []) {
                    const allPosts = await JSON.parse(localStorage.allPosts);
                    const newAllPosts = allPosts.filter(
                        item => this.editableID != item.title
                    );
                    newAllPosts.unshift(this.newPost);
                    localStorage.allPosts = JSON.stringify(newAllPosts);
                    console.log(newAllPosts);
                    this.$emit("postAlart", { sucMsg: "Post Updated!" });
                    this.closeModal();
                } else {
                    this.$emit("postAlart", {
                        dangerMsg: "Category can't be empty",
                    });
                }
            } catch (error) {
                this.$emit("postAlart", {
                    dangerMsg: "Something went wrong",
                    error,
                });
            }
        },
        closeModal() {
            this.$emit("closeModal", "edit");
        },
        closeCategories() {
            this.show = false;
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
