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
        .get("https://brojenuel.com/articles.json")
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
    <div class="w-400px bg-[var(--background)]">
        <div
            class="w-full mx-auto flex justify-between items-center p-2 fixed bg-[var(--background)] z-50"
        >
            <div
                @click="openBroJenuel()"
                class="flex items-center text-[var(--gray-lightest)] cursor-pointer text-size-20px"
            >
                <span>Bro</span>
                <span class="text-[var(--primary)] font-800">Jenuel</span>
                <span>Articles</span>
            </div>
        </div>
        <div class="pl-2 pr-2 pb-1 pt-10">
            <div class="min-h-[100vh]">
                <div
                    class="grid grid-cols-1 gap-3 sm:pl-0 pl-20px"
                    v-if="DATA.blogList.length"
                >
                    <div
                        v-for="blog in DATA.blogList"
                        :key="blog.id"
                        @click="
                            clickLInk(`https://brojenuel.com/blog/${blog.slug}`)
                        "
                        class="p-10px rounded-md transform translate-y-1 hover:translate-y-0 transition-transform cursor-pointer group hover:bg-[var(--background-secondary)] flex flex-col gap-3 ml-4"
                    >
                        <div
                            v-if="
                                blog.cover_img &&
                                !(blog.cover_img.indexOf('youtube') > -1)
                            "
                            class="rounded-2xl overflow-hidden"
                        >
                            <img
                                :src="blog.cover_img"
                                class="float-right w-full"
                            />
                        </div>
                        <div class="relative group md:order-1">
                            <div
                                class="absolute h-10px w-10px bg-gray-600 -left-5 group-hover:bg-[var(--primary)] opacity-50 group-hover:opacity-100 transition-all duration-300 rounded-lg delay top-[50%] transform translate-y-[-50%] translate-x-[-50%]"
                            ></div>
                            <div
                                class="absolute h-1px w-1px group-hover:h-50px bg-[var(--primary)] transition-all duration-500 -left-5 top-[50%] opacity-0 group-hover:opacity-100 transform translate-y-[-50%] translate-x-[-50%]"
                            ></div>
                            <div class="absolute top-40%"></div>
                            <div>
                                <span
                                    class="group-hover:text-[var(--primary)] text-size-18px font-800"
                                >
                                    {{ blog.title }}.
                                </span>
                                <span
                                    class="opacity-80 font-poly text-size-15px"
                                    >{{ blog.summary }}</span
                                >
                            </div>
                            <div class="flex gap-2 my-1">
                                <ul
                                    class="flex gap-1 flex-wrap text-size-13px p-0"
                                >
                                    <li
                                        v-for="tags in blog.tags"
                                        :key="tags"
                                        :class="`tag-${tags}`"
                                        class="tag"
                                    >
                                        #{{ tags }}
                                    </li>
                                </ul>
                            </div>
                            <div>
                                <span
                                    class="italic font-500 opacity-50 whitespace-nowrap flex gap-20px"
                                >
                                    {{
                                        dayjs(blog.created_at).format(
                                            "MMM. DD, YYYY"
                                        )
                                    }}
                                    <span class="flex items-center gap-7px">
                                        <Icon
                                            icon="ic:baseline-remove-red-eye"
                                        />
                                        {{
                                            blog.blog_meta
                                                ? commafy(
                                                      blog.blog_meta.view_count
                                                  )
                                                : 0
                                        }}
                                    </span>
                                </span>
                            </div>
                        </div>
                    </div>
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
