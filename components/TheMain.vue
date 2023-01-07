<script setup lang="ts">
const lazerEnergy = ref(0)
const chargeTime = 1_000
const score = ref(0)

let timeOrigin = 0;

/** 経過時間によってエネルギーをチャージする。 */
const charge = (time: number) => {
  if (timeOrigin === 0) timeOrigin = time
  const dist = time - timeOrigin
  lazerEnergy.value = Math.min(100, Math.floor(100 * dist / chargeTime))

  if (dist < chargeTime) requestAnimationFrame(charge)
}

onMounted(() => {
  requestAnimationFrame(charge)
})

/** レーザーを発射する。 */
const fire = () => {
  if (lazerEnergy.value < 100) return

  console.log('fire')
  score.value += 1
  timeOrigin = 0
  requestAnimationFrame(charge)
}
</script>

<template>
  <div class="p-2">
    <div>
      <ul class="list-disc list-inside">
        <li>Energy が 100% 充填されるとレーザーが打てる（FIRE）</li>
        <li>レーザーを打つと Energy が 0% になる</li>
        <li>Energy は時間経過により再充填される</li>
      </ul>
    </div>
    <div class="w-fit mx-auto p-2 rounded bg-gray-100 uppercase cursor-pointer" @click="fire">fire</div>
    <p>Score: {{ score }}</p>
    <p>Energy: {{ lazerEnergy }} %</p>
  </div>
</template>