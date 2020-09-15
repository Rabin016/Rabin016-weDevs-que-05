<template>
    <div class="md:grid grid-cols-3 gap-5 py-5">
        <div
            v-for="(category, index) in allCategories"
            :key="index"
            class="p-5 rounded-xl shadow-lg"
        >
            <div class="flex justify-between items-center">
                <p class="font-semibold">{{ category }}</p>
                <div class="flex items-center text-center">
                    <button
                        class="font-bold ml-2 text-xl w-6 h-6 hover:text-indigo-600 transition-all ease-in-out duration-300"
                        @click="deleteCategory(category)"
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
        </div>
    </div>
</template>

<script>
export default {
    name: "Categorylist",
    props: {
        allCategories: Array,
    },
    methods: {
        deleteCategory(id) {
            this.$emit("triggerDelCategory", id);
        },
    },
};
</script>

<style></style>
