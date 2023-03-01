<script setup lang="ts">
import { useDebounceFn } from '@vueuse/core';
import { computed, reactive, ref } from 'vue';
import Card from './Card.vue';
import CardBox from './CardBox.vue';


export interface cardInfo {
  type: number;
  num: number;
  position: Position
  status: number
}
export interface Position {
  top: number, left: number | string, width: string, height: string
}

const cardPoolPosition = {
  top: 10,
  left: 10
}
const props = defineProps<{ changeShow: () => void }>()
const cardPoolList = reactive<any>({})

const cardDataList = ref<Array<cardInfo>>([])

const positionList = reactive<Array<Position>>([])

const generPosition = () => {
  for (let i = 1; i < 6; i++) {
    let left = i * 200
    let cardData = { left: 150 + left, top: 600, width: '100', height: '160' };
    positionList.push(cardData);
  }
}
let count = ref(0)
const generateCardData = () => {
  //4副牌
  for (let i = 0; i < 4; i++) {
    //一副牌13张
    for (let j = 0; j < 13; j++) {
      let type = 3;

      //四色
      type = i % 4;
      let cardData = { type, num: j + 1, position: { left: 100, top: 10, width: '100%', height: '100%' }, status: 2 };
      cardDataList.value = [...cardDataList.value, cardData]
      // cardDataList.value.push(cardData);
    }
  }
  //乱序 -- 将牌打乱
  cardDataList.value.sort(() => { return Math.random() > 0.5 ? -1 : 1 })
}
const cardNum = computed(() => {
  let count = 0
  cardDataList.value.forEach((item) => {
    if (item.position.left === 100) {
      count = count + 1
    }
  })
  return count

})
const debouncedFn = useDebounceFn((e) => {
  deal(e)
}, 800)

window.addEventListener('resize', debouncedFn)

const generateId = () => {
  return `${new Date().getTime()}&${Math.random()}`
}
const createCardPool = (num = 6) => {
  let { left, top } = cardPoolPosition;
  for (let i = 0; i < num; i++) {
    let id = generateId();
    let position = { left: left + 20 * i, top };
    let prop = { id, value: { status: 2 }, position };
    cardPoolList[id] = prop;
  }


}
createCardPool()
generateCardData()
generPosition()
const deal = (e: any) => {
  console.log(111);
  const dataList = ref<Array<cardInfo>>([])
  count.value += 1
  if (cardNum.value < 5) {
    cardDataList.value.forEach((item) => {
      if (item.position.left !== 10) {
        item.position = { left: 100, top: 10, width: '100%', height: '100%' }
        item.status = 2
      }
    })
    cardDataList.value.sort(() => { return Math.random() > 0.5 ? -1 : 1 })

  }
  // const dataList = cardDataList.filter((item, index) => {
  //   if (index < count.value * 5 && index >= (count.value - 1) * 5) {
  //     item.status = 1
  //     item.position = positionList[index % 5]
  //     return item
  //   }
  // })
  // changeCardDataList = [...changeCardDataList, ...dataList]
  // console.log(changeCardDataList);

  cardDataList.value.forEach((item, index: number) => {

    if (index < count.value * 5 && index >= (count.value - 1) * 5) {
      dataList.value.push(item)
      item.status = 1
      switch (index % 5) {
        case 0:
          setTimeout(() => {
            item.position = positionList[index % 5]
          }, 200)
        case 1:
          setTimeout(() => {
            item.position = positionList[index % 5]
          }, 400)
        case 2:
          setTimeout(() => {
            item.position = positionList[index % 5]
          }, 600)
        case 3:
          setTimeout(() => {
            item.position = positionList[index % 5]
          }, 800)
        case 4:
          setTimeout(() => {
            item.position = positionList[index % 5]
          }, 1000)
      }
    } else if (index < (count.value - 1) * 5 && index >= (count.value - 2) * 5) {
      item.position = { left: 1200, top: 10, width: '100%', height: '100%' }
    }
    if (count.value > 10) {
      count.value = 0
    }

  })
  if (dataList.value.every((item) => {
    item.num === dataList.value[0].num
  })) {
    props.changeShow()
    setTimeout(() => { props.changeShow() }, 1000)
  }
}

</script>

<template>
          <div class="btn">
            <div>剩余牌数：{{ cardNum }}</div>
            <Card :id="index" :value="item" v-for="(item, index) in cardDataList" :key="index" />
            <button @click="debouncedFn($event)">发牌</button>
          </div>
          <CardBox v-for="(item, index) in positionList" :position=item :id="index" />
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

.btn{
  position: absolute;
  top: 100px;
  left: 400px;
}
</style>
