<template>
    <div class="md:grid grid-cols-3 gap-5 py-5">
        <div
            v-for="(post, post_index) in allPosts"
            :key="post_index"
            class="p-5 rounded-xl shadow-lg post"
        >
            <div class="flex justify-between items-center">
                <h3 class="font-semibold">{{ post.title }}</h3>
                <div class="flex items-center text-center">
                    <button
                        class="hover:text-indigo-600 transition-all ease-in-out duration-300"
                        @click="editPost(post.title)"
                    >
                        <svg
                            viewBox="0 0 20 20"
                            fill="currentColor"
                            class="pencil w-4 h-4"
                        >
                            <path
                                d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"
                            ></path>
                        </svg>
                    </button>

                    <button
                        class="font-bold ml-2 text-xl w-6 h-6 hover:text-indigo-600 transition-all ease-in-out duration-300"
                        @click="deletePost(post.title)"
                    >
                        <svg
                            viewBox="0 0 20 20"
                            fill="currentColor"
                            class="x w-4 h-4"
                        >
                            <path
                                fill-rule="evenodd"
                                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                                clip-rule="evenodd"
                            ></path>
                        </svg>
                    </button>
                </div>
            </div>
            <p class="text-sm">
                {{ post.msg }}
            </p>
            <div class="pt-3 flex items-center text-xs font-semibold">
                <span
                    v-for="(category, index) in post.category_id"
                    :key="index"
                    class="py-1 px-2 bg-gray-300 mr-1 rounded-xl"
                    >{{ category }}</span
                >
            </div>
        </div>
    </div>
</template>

<script>
import gsap from "gsap";

export default {
    name: "Postlist",
    // eslint-disable-next-line
    props: { allPosts: Array },
    mounted() {
        gsap.from("post", { opacity: 0, stagger: 0.1, duration: 0.5 });
    },
    methods: {
        deletePost(id) {
            this.$emit("triggerDelPost", id);
        },
        editPost(id) {
            this.$emit("triggerEdit", id);
        },
    },
};
</script>

<style></style>
