<template>
    <div class="shadow-xl flex flex-col justify-center items-center">
        <div class="arrow-up"></div>
        <div class="p-2 mt-1 bg-gray-400 rounded-lg ">
            <form class="flex items-center" @submit.prevent="submitCategory()">
                <input
                    v-model="newCategory"
                    class="py-1 px-2 text-sm bg-gray-100 rounded-lg placeholder-gray-700 border-2 border-transparent  focus:border-indigo-600 transition-all ease-in-out duration-300"
                    type="text"
                    placeholder="New Category Name"
                />
                <button
                    class="px-3 py-1 ml-2 text-sm bg-indigo-600 text-gray-200 font-semibold rounded-lg justify-between hover:bg-indigo-800 transition-all ease-in-out duration-300"
                    type="submit"
                >
                    Add
                </button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: "Addcategory",
    data: () => ({
        newCategory: null,
        allMsg: null,
    }),
    methods: {
        async submitCategory() {
            try {
                if (this.newCategory) {
                    if (localStorage.allCategories) {
                        const allCategories = await JSON.parse(
                            localStorage.allCategories
                        );
                        allCategories.push(this.newCategory);
                        localStorage.allCategories = await JSON.stringify(
                            allCategories
                        );
                    } else {
                        localStorage.allCategories = JSON.stringify([
                            this.newCategory,
                        ]);
                    }
                    this.$emit("reloadCategory");
                    this.closeCategories();
                } else {
                    this.allMsg = { dangerMsg: "Category not added" };
                }
            } catch (err) {
                this.allMsg = { dangerMsg: "Category not added", err };
            }
        },
        closeCategories() {
            this.$emit("closeCategories");
        },
    },
};
</script>

<style>
.arrow-up {
    width: 0;
    height: 0;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-bottom: 10px solid #5a67d8;
    border-radius: 10px;
}
</style>
