<template>
    <TheHeader />

    <main>
        <h1 class="text-4xl pt-4 pb-10">게시글이요</h1>
        <h3 class="text-xl py-4">보여주세요</h3>

        <ul class="flex flex-col gap-y-10 pt-10">
            <li
                v-for="(
                    { title, content, writer }, index
                ) in infiniteConfig.list"
                :key="index"
            >
                <h3>{{ title }}</h3>
                <p>{{ content }}, {{ writer }}</p>
            </li>
        </ul>

        <div v-if="!infiniteConfig.isEnd" class="flex justify-center py-4">
            <span
                class="loader"
                style="--loader-color: #000"
                ref="$loader"
            ></span>
        </div>

        <!-- v-if="!infiniteConfig.isEnd" -->
        <div v-if="false" class="flex justify-center py-4">
            <InfiniteBasic :target="$loader" @on-action="handleAction">
                <!-- ref="$loader" -->
                <span class="loader" style="--loader-color: #000"></span>
            </InfiniteBasic>
        </div>
    </main>
</template>

<script setup lang="ts">
import { ref, reactive, watch } from "vue";
import { useElementVisibility } from "@vueuse/core";

import { TheHeader } from "@components/layout";
import { InfiniteBasic } from "@components/infinite";

const $loader = ref<null | HTMLElement>(null);

const loaderIsVisible = useElementVisibility($loader);

type Page = {
    title: string;
    content: string;
    writer: string;
};

const infiniteConfig = reactive<{
    pageNo: number;
    list: Page[];
    isEnd: boolean;
}>({
    pageNo: 1,
    list: [],
    isEnd: false,
});

function handleAction() {
    const { list, pageNo } = infiniteConfig;

    const tempArr: Page[] = Array(20).fill({
        title: `제목입니다 ${pageNo}`,
        content: `내용입니다 ${pageNo}`,
        writer: `이준용${pageNo}`,
    });

    infiniteConfig.list = [...list, ...tempArr];

    if (pageNo < 10) {
        infiniteConfig.pageNo++;
    } else {
        infiniteConfig.isEnd = true;
    }
}

watch(loaderIsVisible, (to) => {
    if (to) {
        handleAction();
    }
});
</script>

<style lang="scss" scoped></style>
