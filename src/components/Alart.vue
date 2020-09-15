<template>
    <div class="fixed text-xs absolute-center">
        <div
            v-if="alartMsg.sucMsg"
            class="p-1 mb-2 rounded-lg bg-green-100 text-green-600 border-2 border-green-600"
        >
            <p><span class="text-sm">&#x2714;</span> {{ alartMsg.sucMsg }}</p>
        </div>
        <div
            v-if="alartMsg.dangerMsg"
            class="p-1 mb-2 rounded-lg bg-red-100 text-red-600 border-2 border-red-600"
        >
            <p><span class="text-sm">&#9888;</span> {{ alartMsg.dangerMsg }}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "Alart",
    props: {
        alartMsg: Object,
    },
};
</script>

<style>
.absolute-center {
    left: 50%;
    transform: translateX(-50%);
}
</style>
