<template>
    <slot />
</template>

<script setup lang="ts">
import { toRefs, watch } from "vue";

const props = defineProps({
    target: {
        type: Object as () => HTMLElement | null,
        default: null,
    },
});
const { target } = toRefs(props);

const emits = defineEmits(["onAction"]);

function setIntersection() {
    const observer = new IntersectionObserver(
        (
            entries: IntersectionObserverEntry[],
            // observer: IntersectionObserver,
        ) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    emits("onAction");
                }
            });
        },
        {
            root: null,
            rootMargin: "0px",
            threshold: 0.1,
        },
    );

    observer.observe(target.value!);
}

watch(target, (to) => {
    if (to) {
        setIntersection();
    }
});
</script>

<style lang="scss"></style>
