<script setup lang="ts">
import { onMounted, ref } from 'vue'
import rawQuotes from './社畜厭世語錄.txt?raw'

type Fortune = {
  id: string
  tag: string
  text: string
  color: string
  item: string
}

const fortunes: Fortune[] = [...rawQuotes.matchAll(/社畜厭世語錄(\d+)：\s*\n([^\n]+)/g)]
  .map((match) => {
    const index = match[1]
    const text = match[2]
    if (!index || !text) return null
    return {
      id: `S${index.padStart(2, '0')}`,
      tag: '社畜厭世語錄',
      text: text.trim(),
      color: '上班灰',
      item: '咖啡',
    }
  })
  .filter((fortune): fortune is Fortune => Boolean(fortune))

if (fortunes.length === 0) {
  fortunes.push({
    id: 'S01',
    tag: '社畜厭世語錄',
    text: '再努力一下，今天就快下班了。',
    color: '上班灰',
    item: '咖啡',
  })
}

const current = ref<Fortune>(fortunes[0]!)
const isCracking = ref(false)
const lastIndex = ref(-1)

const pickIndex = () => {
  if (fortunes.length === 1) return 0
  let next = Math.floor(Math.random() * fortunes.length)
  while (next === lastIndex.value) {
    next = Math.floor(Math.random() * fortunes.length)
  }
  return next
}

const drawFortune = () => {
  if (isCracking.value) return
  isCracking.value = true
  const nextIndex = pickIndex()
  window.setTimeout(() => {
    current.value = fortunes[nextIndex]!
    lastIndex.value = nextIndex
    isCracking.value = false
  }, 720)
}

onMounted(() => {
  drawFortune()
})
</script>

<template>
  <main class="page">
    <header class="hero">
      <p class="eyebrow">2026 社畜語錄</p>
      <h1>社畜餅乾</h1>
      <p class="sub">
        給我錢就好～～～
      </p>
    </header>

    <section class="fortune-area">
      <div class="cookie-stage" :class="{ cracking: isCracking }">
        <div
          class="cookie"
          role="button"
          tabindex="0"
          aria-label="抽一張新的社畜厭世語錄"
          @click="drawFortune"
          @keydown.enter.prevent="drawFortune"
          @keydown.space.prevent="drawFortune"
        >
          <div class="cookie-half left"></div>
          <div class="cookie-half right"></div>
          <div class="cookie-strip"></div>
        </div>
        <div class="crumbs">
          <span></span>
          <span></span>
          <span></span>
        </div>
        <p class="cookie-tip">按一聲，你的心聲就出來</p>
      </div>

      <div class="message-card" :class="{ reveal: !isCracking }">
        <div class="message-top">
          <span class="tag">{{ current.tag }}</span>
          <span class="serial">No. {{ current.id }}</span>
        </div>
        <p class="message">{{ current.text }}</p>
        <div class="meta">
        </div>
      </div>
    </section>

    <div class="actions">
      <button class="draw" type="button" :disabled="isCracking" @click="drawFortune">
        <img src="/Happy3.png" alt="再抽一次" class="draw-image" />
      </button>
      <p class="hint">點一下，換一個新社畜心聲</p>
    </div>
  </main>
</template>

<style scoped>
.page {
  position: relative;
  max-width: 1100px;
  margin: 0 auto;
  padding: 64px 20px 80px;
  display: grid;
  gap: 36px;
}

.hero {
  text-align: left;
  display: grid;
  gap: 14px;
}

.eyebrow {
  font-family: var(--font-display);
  letter-spacing: 0.2em;
  font-size: 0.75rem;
  text-transform: uppercase;
  color: var(--accent-2);
}

.hero h1 {
  font-family: var(--font-display);
  font-size: clamp(2.6rem, 5vw, 4rem);
  margin: 0;
}

.sub {
  font-size: 1.05rem;
  max-width: 520px;
  color: var(--muted);
}

.fortune-area {
  display: grid;
  gap: 28px;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  align-items: center;
}

.cookie-stage {
  position: relative;
  background: var(--surface);
  border-radius: 28px;
  padding: 32px 24px 40px;
  box-shadow: var(--shadow-soft);
  overflow: hidden;
}

.cookie {
  position: relative;
  width: 220px;
  height: 160px;
  margin: 0 auto 20px;
  display: grid;
  place-items: center;
  background: url('/fun6.png') center / contain no-repeat;
  cursor: pointer;
}

.cookie-half {
  display: none;
  position: absolute;
  width: 130px;
  height: 100px;
  border-radius: 100px 100px 80px 80px;
  background: linear-gradient(135deg, #f4c36f, #f2a951);
  box-shadow: inset 0 -10px 20px rgba(160, 90, 20, 0.2);
}

.cookie-half.left {
  transform: rotate(-18deg) translateX(-28px);
}

.cookie-half.right {
  transform: rotate(16deg) translateX(28px);
}

.cookie-strip {
  display: none;
  width: 140px;
  height: 36px;
  border-radius: 12px;
  background: #fff6e4;
  box-shadow: 0 10px 20px rgba(160, 90, 20, 0.2);
  z-index: 1;
  position: relative;
}

.crumbs span {
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #f4c36f;
  opacity: 0.6;
}

.crumbs span:nth-child(1) {
  top: 26px;
  right: 24px;
}

.crumbs span:nth-child(2) {
  bottom: 44px;
  left: 30px;
}

.crumbs span:nth-child(3) {
  bottom: 20px;
  right: 60px;
}

.cookie-tip {
  text-align: center;
  font-size: 0.9rem;
  color: var(--muted);
}

.message-card {
  background: #fff;
  border-radius: 28px;
  padding: 28px;
  box-shadow: var(--shadow-strong);
  min-height: 220px;
  display: grid;
  gap: 18px;
  opacity: 0.4;
  transform: translateY(10px);
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.message-card.reveal {
  opacity: 1;
  transform: translateY(0);
}

.message-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.tag {
  padding: 6px 14px;
  border-radius: 999px;
  background: var(--accent);
  color: #fff;
  font-size: 0.75rem;
  letter-spacing: 0.08em;
}

.serial {
  font-size: 0.85rem;
  color: var(--muted);
}

.message {
  font-size: 1.3rem;
  margin: 0;
  line-height: 1.6;
}

.meta {
  display: grid;
  grid-template-columns: repeat(2, minmax(120px, 1fr));
  gap: 16px;
}

.label {
  display: block;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--muted);
  margin-bottom: 4px;
}

.value {
  font-size: 1rem;
  font-weight: 600;
}

.actions {
  display: grid;
  gap: 10px;
  justify-items: start;
}

.draw {
  border: none;
  border-radius: 999px;
  padding: 10px 18px;
  font-size: 1rem;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(120deg, #ff7a59, #f2a951);
  box-shadow: 0 16px 28px rgba(255, 122, 89, 0.35);
  cursor: pointer;
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.draw:hover {
  transform: translateY(-2px);
  box-shadow: 0 20px 30px rgba(255, 122, 89, 0.45);
}

.draw:disabled {
  opacity: 0.6;
  cursor: default;
  transform: none;
}

.draw-image {
  display: block;
  width: 140px;
  height: auto;
}

.hint {
  color: var(--muted);
  font-size: 0.9rem;
}

.cookie-stage.cracking .cookie-half.left {
  animation: crack-left 0.7s ease;
}

.cookie-stage.cracking .cookie-half.right {
  animation: crack-right 0.7s ease;
}

.cookie-stage.cracking .cookie-strip {
  animation: pop-up 0.7s ease;
}

@keyframes crack-left {
  0% {
    transform: rotate(-18deg) translateX(-28px);
  }
  40% {
    transform: rotate(-26deg) translate(-44px, -10px);
  }
  100% {
    transform: rotate(-18deg) translateX(-28px);
  }
}

@keyframes crack-right {
  0% {
    transform: rotate(16deg) translateX(28px);
  }
  40% {
    transform: rotate(26deg) translate(44px, -10px);
  }
  100% {
    transform: rotate(16deg) translateX(28px);
  }
}

@keyframes pop-up {
  0% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0);
  }
}

@media (max-width: 720px) {
  .page {
    padding: 48px 16px 60px;
  }

  .actions {
    justify-items: stretch;
  }

  .draw {
    width: 100%;
  }
}
</style>
