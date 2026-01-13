<script setup lang="ts">
import { nextTick, onMounted, ref } from 'vue'
import blessings, { jackpotBlessing, type Blessing } from './data/blessings'

declare const lucide: { createIcons: () => void }

const status = ref<'idle' | 'processing' | 'result'>('idle')
const currentBlessing = ref<Blessing | null>(null)

const allBlessings: Blessing[] = blessings

const renderIcons = () => {
  if (typeof lucide === 'undefined') return
  nextTick(() => {
    lucide.createIcons()
  })
}

const handleStart = () => {
  if (status.value === 'processing') return
  status.value = 'processing'
  renderIcons()

  window.setTimeout(() => {
    if (Math.random() < 0.01) {
      currentBlessing.value = jackpotBlessing
      status.value = 'result'
      renderIcons()
      return
    }
    const randomIndex = Math.floor(Math.random() * allBlessings.length)
    currentBlessing.value = allBlessings[randomIndex] ?? null
    status.value = 'result'
    renderIcons()
  }, 100)
}

const handleReset = () => {
  status.value = 'idle'
  currentBlessing.value = null
  renderIcons()
}

onMounted(() => {
  renderIcons()
})
</script>

<template>
  <main class="min-h-screen flex items-center justify-center p-4 relative overflow-hidden bg-slate-900 text-slate-100">
    <div class="absolute inset-0 z-0 opacity-20 pointer-events-none">
      <div
        class="absolute top-0 left-0 w-full h-full bg-[linear-gradient(to_right,#80808012_1px,transparent_1px),linear-gradient(to_bottom,#80808012_1px,transparent_1px)] bg-[size:24px_24px]">
      </div>
      <div
        class="absolute top-1/4 left-1/4 w-96 h-96 bg-cyan-500 rounded-full mix-blend-screen filter blur-[128px] opacity-20 animate-pulse">
      </div>
      <div
        class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-purple-500 rounded-full mix-blend-screen filter blur-[128px] opacity-20 animate-pulse delay-1000">
      </div>
    </div>

    <div class="relative z-10 w-full max-w-md">
      <div class="text-center mb-8">
        <div
          class="inline-flex items-center gap-2 mb-2 px-3 py-1 rounded-full bg-slate-800 border border-slate-700 text-xs text-cyan-400">
          <span class="w-2 h-2 rounded-full bg-green-500 animate-ping"></span>
          SYSTEM ONLINE
        </div>
        <h1
          class="text-3xl font-black bg-clip-text text-transparent bg-gradient-to-r from-cyan-400 to-purple-500 tracking-tighter">
          康和運算中心
        </h1>
        <p class="text-slate-400 text-sm mt-1">Fortune Message</p>
      </div>

      <div class="relative min-h-[400px] flex flex-col justify-center">
        <div v-if="status === 'idle'" class="flex flex-col items-center animate-[fadeIn_0.3s_ease-out]">
          <button
            class="group relative w-64 h-64 rounded-full bg-slate-800 border-4 border-cyan-500/30 flex flex-col items-center justify-center cursor-pointer hover:border-cyan-400 hover:shadow-[0_0_40px_rgba(34,211,238,0.4)] transition-all duration-300 active:scale-95"
            @click="handleStart">
            <div class="absolute inset-0 rounded-full border-t-4 border-cyan-500/20 animate-[spin_8s_linear_infinite]">
            </div>
            <div
              class="absolute inset-2 rounded-full border-b-4 border-purple-500/20 animate-[spin_12s_linear_infinite_reverse]">
            </div>

            <img src="/fun6.png" alt="點擊運算"
              class="w-20 h-20 mb-4 group-hover:scale-110 transition-transform duration-300" />

            <span class="text-xl font-bold text-white tracking-widest group-hover:text-cyan-300">點擊運算</span>
            <span class="text-xs text-slate-400 mt-2 group-hover:text-cyan-200/70">CLICK TO START</span>
          </button>
          <p class="mt-8 text-slate-500 text-sm text-center animate-pulse">等待輸入指令...</p>
        </div>

        <div v-if="status === 'processing'"
          class="flex flex-col items-center justify-center animate-[fadeIn_0.3s_ease-out]">
          <div class="w-24 h-24 mb-6 relative">
            <div
              class="absolute inset-0 border-4 border-t-cyan-400 border-r-transparent border-b-purple-400 border-l-transparent rounded-full animate-spin">
            </div>
            <div
              class="absolute inset-2 border-4 border-t-transparent border-r-blue-400 border-b-transparent border-l-pink-400 rounded-full animate-spin direction-reverse">
            </div>
            <i data-lucide="cpu" class="absolute inset-0 m-auto w-8 h-8 text-white animate-pulse"></i>
          </div>
          <h2 class="text-2xl font-bold text-white mb-2">正在分析大數據...</h2>
          <div class="flex flex-col gap-1 text-xs text-green-400 font-mono text-center opacity-80 h-16 overflow-hidden">
            <span class="animate-pulse">Loading neural networks...</span>
            <span class="animate-pulse delay-75">Optimizing luck parameters...</span>
            <span class="animate-pulse delay-150">Fetching bonus data...</span>
          </div>
        </div>

        <div v-if="status === 'result' && currentBlessing" class="w-full animate-[slideInBottom_0.5s_ease-out]">
          <div
            class="relative bg-slate-800/80 backdrop-blur-xl border-2 rounded-2xl p-8 text-center shadow-2xl overflow-hidden"
            :class="currentBlessing.color">
            <div class="absolute -top-10 -left-10 w-32 h-32 bg-white opacity-10 blur-3xl rounded-full"></div>

            <div class="flex justify-center mb-6">
              <div class="p-4 bg-slate-900 rounded-2xl border border-slate-700 shadow-inner relative">
                <div v-if="currentBlessing.title === '彩蛋JP'" class="flex items-center justify-center gap-3 px-2">
                  <img src="/Happy1.png" alt="彩蛋 JP 1"
                    class="w-24 h-24 object-contain drop-shadow-[0_0_28px_rgba(250,204,21,0.8)] animate-[float_3.6s_ease-in-out_infinite] sparkle" />
                  <img src="/Happy2.png" alt="彩蛋 JP 2"
                    class="w-28 h-28 object-contain drop-shadow-[0_0_34px_rgba(250,204,21,0.9)] animate-[float_3s_ease-in-out_infinite] sparkle" />
                  <img src="/Happy3.png" alt="彩蛋 JP 3"
                    class="w-24 h-24 object-contain drop-shadow-[0_0_28px_rgba(250,204,21,0.8)] animate-[float_3.2s_ease-in-out_infinite] sparkle" />
                  <div class="coin coin-1" aria-hidden="true"></div>
                  <div class="coin coin-2" aria-hidden="true"></div>
                  <div class="coin coin-3" aria-hidden="true"></div>
                  <div class="coin coin-4" aria-hidden="true"></div>
                  <div class="coin coin-5" aria-hidden="true"></div>
                  <div class="coin coin-6" aria-hidden="true"></div>
                  <div class="coin coin-7" aria-hidden="true"></div>
                  <div class="coin coin-8" aria-hidden="true"></div>
                  <div class="coin coin-9" aria-hidden="true"></div>
                  <div class="coin coin-10" aria-hidden="true"></div>
                  <div class="coin coin-11" aria-hidden="true"></div>
                  <div class="coin coin-12" aria-hidden="true"></div>
                  <div class="coin coin-13" aria-hidden="true"></div>
                  <div class="coin coin-14" aria-hidden="true"></div>
                  <div class="coin coin-15" aria-hidden="true"></div>
                  <div class="coin coin-16" aria-hidden="true"></div>
                  <div class="coin coin-17" aria-hidden="true"></div>
                  <div class="coin coin-18" aria-hidden="true"></div>
                  <div class="coin coin-19" aria-hidden="true"></div>
                  <div class="coin coin-20" aria-hidden="true"></div>
                  <div class="coin coin-21 coin-fore" aria-hidden="true"></div>
                  <div class="coin coin-22 coin-fore" aria-hidden="true"></div>
                  <div class="coin coin-23 coin-fore" aria-hidden="true"></div>
                  <div class="coin coin-24 coin-fore" aria-hidden="true"></div>
                </div>
                <img v-else-if="currentBlessing.iconType === 'image'" :src="currentBlessing.iconName"
                  :alt="currentBlessing.title" class="w-16 h-16 object-contain" />
                <i v-else :data-lucide="currentBlessing.iconName" class="w-16 h-16"
                  :class="currentBlessing.iconColor"></i>
              </div>
            </div>

            <h2 class="text-3xl font-black text-transparent bg-clip-text bg-gradient-to-r from-white to-slate-300 mb-3">
              {{ currentBlessing.title }}
            </h2>

            <div class="h-px w-full bg-gradient-to-r from-transparent via-slate-500 to-transparent my-4"></div>

            <p class="text-lg text-cyan-100 leading-relaxed font-light">
              {{ currentBlessing.desc }}
            </p>

            <div class="mt-8">
              <button
                class="w-full py-3 bg-gradient-to-r from-cyan-600 to-blue-600 hover:from-cyan-500 hover:to-blue-500 text-white font-bold rounded-lg shadow-lg flex items-center justify-center gap-2 transform active:scale-95 transition-all"
                @click="handleReset">
                <i data-lucide="refresh-cw" class="w-5 h-5"></i>
                重新計算
              </button>
            </div>
          </div>
          <p class="text-center text-xs text-slate-500 mt-4">Result Generated by 資訊部</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
@keyframes spin-reverse {
  to {
    transform: rotate(-360deg);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@keyframes slideInBottom {
  from {
    opacity: 0;
    transform: translateY(12px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-6px);
  }
}

.sparkle {
  animation: sparkle 1.2s ease-in-out infinite;
}

.coin {
  position: absolute;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, #fff4b0, #fbbf24 60%, #b45309 100%);
  box-shadow: 0 0 12px rgba(250, 204, 21, 0.8);
  animation: coin-shower 2.2s ease-in-out infinite;
}

.coin-fore {
  width: 22px;
  height: 22px;
  box-shadow: 0 0 18px rgba(250, 204, 21, 0.9);
  filter: blur(0.2px);
}

.coin-1 {
  left: 6%;
  top: -12px;
  animation-delay: 0s;
  animation-duration: 2s;
}

.coin-2 {
  left: 18%;
  top: -18px;
  animation-delay: 0.25s;
  animation-duration: 2.4s;
}

.coin-3 {
  left: 32%;
  top: -10px;
  animation-delay: 0.5s;
  animation-duration: 2.1s;
}

.coin-4 {
  left: 48%;
  top: -20px;
  animation-delay: 0.1s;
  animation-duration: 2.3s;
}

.coin-5 {
  left: 62%;
  top: -14px;
  animation-delay: 0.35s;
  animation-duration: 2.5s;
}

.coin-6 {
  left: 74%;
  top: -22px;
  animation-delay: 0.6s;
  animation-duration: 2.2s;
}

.coin-7 {
  right: 8%;
  top: -16px;
  animation-delay: 0.15s;
  animation-duration: 2.6s;
}

.coin-8 {
  right: 20%;
  top: -8px;
  animation-delay: 0.45s;
  animation-duration: 2.1s;
}

.coin-9 {
  left: 12%;
  top: -28px;
  animation-delay: 0.75s;
  animation-duration: 2.7s;
}

.coin-10 {
  left: 40%;
  top: -26px;
  animation-delay: 0.9s;
  animation-duration: 2.4s;
}

.coin-11 {
  left: 56%;
  top: -30px;
  animation-delay: 0.55s;
  animation-duration: 2.3s;
}

.coin-12 {
  right: 14%;
  top: -26px;
  animation-delay: 0.8s;
  animation-duration: 2.5s;
}

.coin-13 {
  left: 22%;
  top: -32px;
  animation-delay: 1s;
  animation-duration: 2.7s;
}

.coin-14 {
  left: 52%;
  top: -34px;
  animation-delay: 1.1s;
  animation-duration: 2.6s;
}

.coin-15 {
  right: 30%;
  top: -28px;
  animation-delay: 0.95s;
  animation-duration: 2.8s;
}

.coin-16 {
  right: 4%;
  top: -20px;
  animation-delay: 1.25s;
  animation-duration: 2.4s;
}

.coin-17 {
  left: 4%;
  top: -22px;
  animation-delay: 1.3s;
  animation-duration: 2.9s;
}

.coin-18 {
  left: 68%;
  top: -30px;
  animation-delay: 1.45s;
  animation-duration: 2.5s;
}

.coin-19 {
  right: 22%;
  top: -34px;
  animation-delay: 1.6s;
  animation-duration: 2.7s;
}

.coin-20 {
  left: 38%;
  top: -36px;
  animation-delay: 1.75s;
  animation-duration: 2.6s;
}

.coin-21 {
  left: 10%;
  top: -40px;
  animation-delay: 0.2s;
  animation-duration: 2.3s;
}

.coin-22 {
  left: 58%;
  top: -44px;
  animation-delay: 0.55s;
  animation-duration: 2.4s;
}

.coin-23 {
  right: 18%;
  top: -38px;
  animation-delay: 0.85s;
  animation-duration: 2.5s;
}

.coin-24 {
  right: 2%;
  top: -42px;
  animation-delay: 1.15s;
  animation-duration: 2.6s;
}

@keyframes sparkle {
  0%,
  100% {
    filter: drop-shadow(0 0 10px rgba(250, 204, 21, 0.6));
    transform: scale(1);
  }

  50% {
    filter: drop-shadow(0 0 22px rgba(250, 204, 21, 1));
    transform: scale(1.06);
  }
}

@keyframes coin-shower {
  0% {
    transform: translateY(0) scale(0.9) rotate(0deg);
    opacity: 0;
  }

  20% {
    opacity: 1;
  }

  70% {
    transform: translateY(46px) scale(1.05) rotate(160deg);
    opacity: 1;
  }

  100% {
    transform: translateY(80px) scale(0.9) rotate(220deg);
    opacity: 0;
  }
}

.animate-spin-reverse {
  animation: spin-reverse 1s linear infinite;
}

.direction-reverse {
  animation-direction: reverse;
}
</style>
