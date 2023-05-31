<script setup lang="ts">
import { onMounted, ref } from "vue";

const text = ref("loading");

function openBroJenuel() {
    window.open("https://brojenuel.com", "_blank");
}

onMounted(() => {
    console.log("here");

    setTimeout(() => {
        text.value = "Done";
    }, 2000);
});
</script>

<template>
    <div class="min-w-320px max-w-400px w-full bg-[var(--background)]">
        <div class="w-full mx-auto flex justify-between items-center p-2">
            <div
                @click="openBroJenuel()"
                class="flex items-center text-[var(--gray-lightest)] cursor-pointer text-size-20px"
            >
                <span>Bro</span>
                <span class="text-[var(--primary)] font-800">Jenuel</span>
                <span>Articles</span>
            </div>
        </div>
        <div class="pl-2 pr-2 pb-1">Articles {{ text }}</div>
    </div>
</template>
