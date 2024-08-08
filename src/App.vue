<script setup lang="ts">
import axios from "axios";
import dayjs from "dayjs";
import { onMounted, ref, reactive } from "vue";
import { Icon } from "@iconify/vue";

const loading = ref(false);
const DATA = reactive<{
    blogList: Array<{
        id: string;
        slug: string;
        cover_img: string;
        title: string;
        summary: string;
        tags: Array<any>;
        created_at: any;
        blog_meta: any;
        updated_at: any;
    }>;
}>({
    blogList: [],
});
const text = ref("loading");

function openBroJenuel() {
    window.open("https://brojenuel.com/blog", "_blank");
}

function clickLInk(url: string) {
    window.open(url, "_blank");
}

function commafy(num: number) {
    var str = num.toString().split(".");
    if (str[0].length >= 5) {
        str[0] = str[0].replace(/(\d)(?=(\d{3})+$)/g, "$1,");
    }
    if (str[1] && str[1].length >= 5) {
        str[1] = str[1].replace(/(\d{3})/g, "$1 ");
    }
    return str.join(".");
}

function getArticles() {
    axios
        .get("https://www.jenuel.dev/articles.json")
        .then(({ data }) => {
            DATA.blogList = data;
        })
        .catch((e) => {
            console.log("error!");
        });
}

onMounted(() => {
    getArticles();
});
</script>

<template>
    <div class="w-400px bg-[var(--background)] mx-auto">
        <div
            class="sticky top-0 w-full max-w-800px mx-auto z-9999 bg-white bg-opacity-50 backdrop-blur-md p-3 flex items-center justify-between mx-3 text-center"
        >
            <a href="/" class="text-18px font-800">
                <span>Jenuel</span
                ><span class="text-[var(--c-blue-500)]">Dev</span>
            </a>
        </div>
        <div class="pl-2 pr-2 pb-1 pt-10">
            <div class="min-h-[100vh]">
                <div class="flex flex-col gap-10" v-if="DATA.blogList.length">
                    <a
                        v-for="blog in DATA.blogList"
                        :key="blog.id"
                        :href="`https://www.jenuel.dev/blog/${blog.slug}`"
                        target="_blank"
                        class="flex flex-col gap-5 group"
                    >
                        <img
                            v-if="blog.cover_img"
                            :src="blog.cover_img"
                            class="rounded-lg w-full mx-auto col-span-2 w-full"
                            width="300"
                            height="200"
                            :alt="blog.title"
                        />
                        <div class="content-summary leading-5 col-span-6">
                            <div class="text-xs sm:mb-1">
                                {{
                                    dayjs(blog.updated_at).format(
                                        "DD MMM, YYYY"
                                    )
                                }}
                            </div>
                            <h4
                                class="sm:mb-2 group-hover:text-[var(--c-blue-500)]"
                            >
                                {{ blog.title }}
                            </h4>
                            <p
                                :class="`line-clamp-6 text-sm ${
                                    blog.cover_img && 'line-clamp-3'
                                }`"
                            >
                                {{ blog.summary }}
                            </p>
                        </div>
                    </a>
                </div>
                <div
                    v-show="loading"
                    class="text-center text-[var(--primary)] pt-20px"
                >
                    <div style="font-size: 50px">
                        <Icon icon="svg-spinners:bars-scale-middle" />
                    </div>
                    <div>loading</div>
                </div>
            </div>
        </div>
    </div>
</template>
