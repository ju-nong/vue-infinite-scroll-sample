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
    trigger: {
        type: Number,
        default: 1,
    },
});
const { target, trigger } = toRefs(props);

const emits = defineEmits(["onAction"]);

function setIntersection() {
    const observer = new IntersectionObserver(
        (
            entries: IntersectionObserverEntry[],
            // observer: IntersectionObserver,
        ) => {
            entries.forEach(async (entry) => {
                if (entry.isIntersecting && target.value) {
                    observer.unobserve(target.value);

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

watch(trigger, () => {
    setIntersection();
});
</script>

<style lang="scss"></style>
