<template>
    <div ref="card" class="card" />
</template>

<script setup lang="ts">
import { computed, nextTick, ref, watch } from 'vue';
import type { cardInfo } from './HelloWorld.vue';
const props = defineProps<{
    id: number, value: cardInfo,
    // cardMousedown: Function,
    // cardMouseup: Function,
    // cardClick: Function,
}>()

let newValue: any = undefined
const img = {
    //背景图
    cardBack: new URL("../assets/cardBack.jpg", import.meta.url).href,
    //花色
    decor: ''
}

watch(
    props.value,
    (newValue, oldValue) => {
        if (newValue.position) {
            if (newValue.position.left) {
                card.value.style.left = `${newValue.position.left}`;
            }
            if (newValue.position.top) {
                card.value.style.top = `${newValue.position.top}`;
            }
        }
        showInfo(newValue);
    },
    { deep: true }
)
const card = ref<InstanceType<typeof HTMLElement>>() as any
nextTick(() => {
    if (props.value.position) {
        if (props.value.position.left) {
            card.value.style.left = `${props.value.position.left}`;
        }
        if (props.value.position.top) {
            card.value.style.top = `${props.value.position.top}`;
        }
    }


    showInfo(props.value);
})
const valuePreview: any = computed(() => props.value)


const showInfo = (value: any) => {
    if (value !== undefined) {
        newValue = value;
    }
    let backgroundImage: string;
    if (value.status === 0 || value.status === 2) {
        backgroundImage = `url(${img.cardBack})`
    } else {
        draw();
    }
    nextTick(() => {
        card._value.style.backgroundImage = backgroundImage;
    })
}
// /**
//  * 绘制
//  */
const draw = () => {
    //花色图片
    // img.decor = require(`@/assets/card/type${valuePreview.type}.png`);
    let type
    switch (valuePreview.value.type) {
        case 0:
            type = "d"
            break;
        case 1:
            type = "c"
            break;
        case 2:
            type = "h"
            break;
        case 3:
            type = "s"
            break;
    }
    img.decor = new URL(`../assets/card/img/${valuePreview.value.num}_${type}.png`, import.meta.url).href;
    nextTick(() => {
        card.value.style.repeat = 'no-repeat'
        card.value.style.backgroundImage = `url(${img.decor})`;
    })
}
// const mousedown = (e: any) => {
//     props.cardMousedown(props.id, this, e);
// }
// const mouseup = (e: any) => {
//     props.cardMouseup(props.id, this, e);
// }
// const click = (e: any) => {
//     props.cardClick(props.id, this, e);
// }

</script>

<style scoped>
.card {
    width: 100px;
    height: 160px;
    text-align: center;
    background-color: white;
    background-size: 100%;
    border-radius: 5px;
    /* box-shadow: 1px 1px 2px 2px rgba(50, 50, 50, 0.3); */
    position: fixed;
    top: 10px;
    z-index: 2;
    transition: left 0.2s, top 0.2s;
}
</style>
