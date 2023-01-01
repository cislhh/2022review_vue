<template>
    <div class="water-fall">
        <div
            class="water-items"
            :style="{
                height: item.height + 'px',
                left: item.left + 'px',
                top: item.top + 'px',
                background: item.background,
            }"
            v-for="item in waterList"
        ></div>
    </div>
</template>

<script setup lang="ts">
import { onMounted, reactive } from "vue";

const props = defineProps<{
    list: any[];
}>();

const waterList = reactive<any[]>([]);


const init = () => {
    const heightList: number[] = [];
    const width = 130;
    // 获取可视区域的宽度
    const pageWidth = document.body.clientWidth;
    // 求可视区域可以放多少列
    const pageListNum = Math.floor(pageWidth / width);

    props.list?.forEach((item, i) => {
        if (i < pageListNum) {
            props.list[i].left = i * width;
            props.list[i].top = 20;
            waterList.push(props.list[i]);
            heightList.push(props.list[i].height+20);
        } else {
            let current = heightList[0];
            let index = 0;
            heightList?.forEach((h, idx) => {
                if (current > h) {
                    current = h;
                    index = idx;
                }
            });
            props.list[i].top = current + 20;
            props.list[i].left = index * width;
            heightList[index] = heightList[index] + props.list[i].height + 20;
            waterList.push(props.list[i]);
        }
    });
};

onMounted(() => {
    window.onresize = () => init()
    init();
});
</script>

<style lang="less" scoped>
.water-fall {
    position: relative;
    height: 100%;
    .water-items {
        position: absolute;
        width: 120px;
    }
}
</style>
