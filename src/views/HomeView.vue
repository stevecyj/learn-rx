<script setup>
import { from, fromEvent, useSubscription } from '@vueuse/rxjs'
import { interval, scan } from 'rxjs'
import { map, takeUntil, withLatestFrom } from 'rxjs/operators'
import { shallowRef, useTemplateRef } from 'vue'

const count = shallowRef(0)
const button = useTemplateRef('buttonRef')

// 計數器
// useSubscription(
//   interval(1000)
//     .pipe(
//       map(() => 1),
//       takeUntil(fromEvent(button, 'click', { passive: true })),
//       withLatestFrom(
//         from(count, {
//           immediate: true,
//           deep: false,
//         }),
//       ),
//       map(([curr, total]) => curr + total),
//     )
//     .subscribe((val) => {
//       count.value = val
//       console.log('計數值：', val)
//     }), // 直接在 subscribe 回呼中更新 count 並印出 log
// )

// 建立訂閱
// useSubscription(
//   fromEvent(document, 'click', { passive: true }).subscribe(() => {
//     console.log('Clicked!')
//   }),
// )

useSubscription(
  fromEvent(document, 'click')
    .pipe(scan((count) => count + 1, 0))
    .subscribe((count) => {
      console.log('計數值：', count)
    }),
)
</script>

<template>
  <main>
    <!-- {{ count }}
    <button ref="buttonRef">Click me</button> -->
  </main>
</template>
