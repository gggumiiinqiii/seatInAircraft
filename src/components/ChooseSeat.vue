<template>
    <div class="container" :style="dynamicStyles">
      <div class="wrapper">
        <div class="front">FRONT</div>
        <div class="wrapperRow" v-for="(seatArray, index) in data" :key="index">
          <div class="seatBox indexBox">{{ index + 1 }}</div>
          <div :class="['seatBox', getClass(item)?.class]" v-for="item in seatArray" @click="clickItem(item)">{{
            item.seatIndex
          }}</div>
        </div>
      </div>
      <div class="wraperComputed">
        <h2>Booking Details</h2>
        <div>Selected Seats ({{ selectedComputed.length }}):</div>
        <ul class="ulList">
          <li v-for="item in selectedComputed">
            {{ item.type == 'first' ? 'First Class' : 'Second Class' }} Seat#{{ item.seatIndex }}
            <a @click="clickItem(item)">cancel</a></li
          >
        </ul>
        <div>Total:${{ totalComputed }}</div>
        <button style="cursor: pointer">CheckOut</button>
        <div class="divText">
          <div class="seatBox firstSeatBox"> </div>
          <span class="spanText">First Class</span>
        </div>
        <div class="divText">
          <div class="seatBox secondSeatBox"> </div>
          <span class="spanText">Second Class</span>
        </div>
        <div class="divText">
          <div class="seatBox brokenSeatBox"> </div>
          <span class="spanText">Broken</span>
        </div>
        <div class="divText">
          <div class="seatBox bookedSeatBox"> </div>
          <span class="spanText">booked</span>
        </div>
        <div class="divText">
          <div class="seatBox selectedSeatBox"> </div>
          <span class="spanText"> Selected</span>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
    interface itemParams {
      seatIndex: number | null
      type: string
      status: string
      price: number
      isRoad: boolean
    }
    import { ref,computed } from 'vue'
    //座位编号，座位等级，座位状态（已经预定booked、损坏:broken、空闲empty、选中selected），
    // 5个座位
    //
    const data = ref<itemParams[][]>([
      [
        {
          seatIndex: 1,
          type: 'first',
          status: 'selected',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: 2,
          type: 'first',
          status: 'broken',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: null,
          type: '',
          status: '',
          price: 0,
          isRoad: true,
        },
        {
          seatIndex: 3,
          type: 'first',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: 4,
          type: 'first',
          status: 'booked',
          price: 30,
          isRoad: false,
        },
      ],
      [
        {
          seatIndex: 5,
          type: 'first',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: 6,
          type: 'first',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: null,
          type: '',
          status: '',
          price: 0,
          isRoad: true,
        },
        {
          seatIndex: 7,
          type: 'first',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: 8,
          type: 'first',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
      ],
      [
        {
          seatIndex: 5,
          type: 'second',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: 6,
          type: 'second',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: null,
          type: '',
          status: '',
          price: 0,
          isRoad: true,
        },
        {
          seatIndex: 7,
          type: 'second',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
        {
          seatIndex: 8,
          type: 'second',
          status: 'empty',
          price: 30,
          isRoad: false,
        },
      ],
    ])
    function getClass(item: itemParams) {
      if (item.isRoad) {
        return {
          class: '',
        }
      } else if (item.status == 'empty') {
        if (item.type == 'first') {
          return {
            class: 'firstSeatBox',
          }
        } else if (item.type == 'second') {
          return {
            class: 'secondSeatBox',
          }
        }
      } else if (item.status == 'booked') {
        return {
          class: 'bookedSeatBox',
        }
      } else if (item.status == 'broken') {
        return {
          class: 'brokenSeatBox',
        }
      } else if (item.status == 'selected') {
        return {
          class: 'selectedSeatBox',
        }
      }
    }
    const dynamicStyles = computed(() => ({
      '--computed-width': `${(data.value[0].length + 1) * 32}px`,
      '--box-wh': '24px',
    }))
    function clickItem(item: itemParams) {
      if (item.status == 'selected') {
        item.status = 'empty'
      } else if (item.status == 'empty') {
        item.status = 'selected'
      }
    }
    const selectedComputed = computed(() => {
      let map = data.value.flatMap(seatArray => {
        return seatArray.filter(item => item.status == 'selected')
      })
      return map
    })
    const totalComputed = computed(() => {
      return selectedComputed.value.reduce((pre, cur) => {
        return pre + cur.price
      }, 0)
    })
  </script>
  <style scoped lang="less">
    .container {
      display: flex;
      justify-content: center;
      margin-top: 10px;
    }
    .wrapper {
      width: var(--computed-width);
      height: 300px;
      box-sizing: border-box;
      padding-right: 24px;
      border-right: 1px dotted #ccc;
    }
    .wrapperRow {
      display: flex;
    }
    .front {
      background-color: #f6f6f6;
      color: #adadad;
      text-align: center;
      margin-left: 36px;
      margin-right: 4px;
      margin-bottom: 16px;
    }
    .seatBox {
      width: var(--box-wh);
      height: var(--box-wh);
      border-radius: 4px;
      line-height: var(--box-wh);
      text-align: center;
      color: white;
      cursor: pointer;
      margin: 4px;
      display: inline-block;
    }
    .indexBox {
      background-color: #fff;
      color: black;
    }
    .firstSeatBox {
      background-color: #3a78c3;
      &:hover {
        background-color: #76b474;
      }
    }
    .secondSeatBox {
      background-color: #b9dea0;
      &:hover {
        background-color: #76b474;
      }
    }
    .bookedSeatBox {
      background-color: #472b34;
      cursor: not-allowed;
    }
    .brokenSeatBox {
      background-color: #e9250b;
      cursor: not-allowed;
    }
    .selectedSeatBox {
      background-color: #e6cac4;
    }
    .wraperComputed {
      padding-left: 24px;
      width: 200px;
    }
    .ulList {
      max-height: 100px;
      overflow: auto;
    }
    .divText {
      display: flex;
      align-items: center;
    }
    .spanText {
      margin-left: 4px;
    }
    a {
        color: blue;
        cursor: pointer;
    }
  </style>
  