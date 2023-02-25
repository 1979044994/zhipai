<script setup lang="ts">
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
  top: number, left: number, width: string, height: string
}
const containerNum = 5
const cardPoolPosition = {
  top: 10,
  left: 10
}
let winWidth = window.document.body.offsetWidth
let winHeight = window.document.body.offsetHeight
const containerList = new Map()
const cardPoolList = reactive<any>({})
const countCard = ref(52)
const cardDataList = ref<Array<cardInfo>>([])
let changeCardDataList = reactive<Array<cardInfo>>([])
const positionList = reactive<Array<Position>>([])
const generPosition = () => {
  for (let i = 1; i < 6; i++) {
    let left = i * 200
    let cardData = { left: left, top: 600, width: '100', height: '160' };
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
    if (item.position.top === 10) {
      count = count + 1
    }
  })
  return count

})


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
  console.log(cardPoolList);

}
const createContainer = () => {
  let space = 10;
  let cellW = winWidth / containerNum;
  let containerTop = 180;
  for (let i = 0; i < containerNum; i++) {
    let position = {
      top: containerTop,
      left: cellW * i + space,
      width: cellW - space * 2,
      height: winHeight - containerTop
    };
    let id = generateId();
    let props = { id, position, cardIds: [] };
    containerList.set([id], { ...props })
  }
}
createCardPool()
generateCardData()
generPosition()
const deal = (e: any) => {

  count.value += 1
  if (cardNum.value < 5) {
    cardDataList.value.forEach((item) => {
      if (item.position.top !== 10) {
        item.position = { left: 100, top: 10, width: '100%', height: '100%' }
        item.status = 2
      }
    })
    cardDataList.value.sort(() => { return Math.random() > 0.5 ? -1 : 1 })
    console.log(cardDataList.value);
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
  console.log(count.value);

  cardDataList.value.forEach((item, index: number) => {

    if (index < count.value * 5 && index >= (count.value - 1) * 5) {
      item.status = 1
      item.position = positionList[index % 5]
      console.log(item);

    }
    if (count.value > 10) {
      count.value = 0
    }
  })
}
</script>

<template>
    <div>
      <div>{{ cardNum }}</div>
      <Card :id="index" :value="item" v-for="(item, index) in cardDataList" :key="index" />
    </div>
    <CardBox v-for="(item, index) in positionList" :position=item :id="index" />
    <button @click="deal($event)">发牌</button>
</template>

<style scoped>
.read-the-docs {
  color: #888;

}
</style>
