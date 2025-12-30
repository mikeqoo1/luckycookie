<script setup lang="ts">
import { onMounted, ref } from 'vue'

type Fortune = {
  id: string
  tag: string
  text: string
  color: string
  item: string
}

const fortunes: Fortune[] = [
  {
    id: 'A01',
    tag: '人際運',
    text: '今晚你說的那句玩笑會成為全場最有溫度的記憶。',
    color: '焦糖橙',
    item: '小卡片',
  },
  {
    id: 'A02',
    tag: '財運',
    text: '把紅包握緊一秒，你的運氣會回握你十分。',
    color: '金砂色',
    item: '金屬筆',
  },
  {
    id: 'A03',
    tag: '團隊運',
    text: '你最懂的那個人，剛好也最懂你。',
    color: '湖水綠',
    item: '隊徽貼紙',
  },
  {
    id: 'A04',
    tag: '創意運',
    text: '靈感像雪花落下，別急著拍掉。',
    color: '霧白',
    item: '便利貼',
  },
  {
    id: 'A05',
    tag: '魅力值',
    text: '今晚的笑容帶有磁場，會吸引好消息靠近。',
    color: '珊瑚紅',
    item: '香氛卡',
  },
  {
    id: 'A06',
    tag: '好運',
    text: '機會正在排隊，你剛好被叫到號。',
    color: '薑黃',
    item: '幸運號碼',
  },
  {
    id: 'A07',
    tag: '勇氣',
    text: '你想做的那件事，今天就能用一句話啟動。',
    color: '深墨綠',
    item: '筆記本',
  },
  {
    id: 'A08',
    tag: '驚喜',
    text: '一個意外的邀請，會讓你多一位新盟友。',
    color: '薄荷藍',
    item: '聯絡資訊',
  },
  {
    id: 'A09',
    tag: '效率',
    text: '你手上的事情會提前完成，留給你一點任性。',
    color: '奶油黃',
    item: '計時器',
  },
  {
    id: 'A10',
    tag: '幸福感',
    text: '最好的位置不是舞台中央，而是你喜歡的那裡。',
    color: '杏桃',
    item: '合照',
  },
  {
    id: 'A11',
    tag: '突破',
    text: '你一直想突破的關卡，今晚會用笑聲打開。',
    color: '焦糖棕',
    item: '禮物袋',
  },
  {
    id: 'A12',
    tag: '安定',
    text: '好運會用很低調的方式站在你身後。',
    color: '暖灰',
    item: '保溫杯',
  },
]

const current = ref<Fortune>(fortunes[0])
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
    current.value = fortunes[nextIndex]
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
      <p class="eyebrow">2025 尾牙特別場</p>
      <h1>幸運餅乾 Lucky Cookie</h1>
      <p class="sub">
        開場就有好運氣，隨機抽一張訊息，今晚主場就是你。
      </p>
    </header>

    <section class="fortune-area">
      <div class="cookie-stage" :class="{ cracking: isCracking }">
        <div class="cookie">
          <div class="cookie-half left"></div>
          <div class="cookie-half right"></div>
          <div class="cookie-strip"></div>
        </div>
        <div class="crumbs">
          <span></span>
          <span></span>
          <span></span>
        </div>
        <p class="cookie-tip">咔嚓一聲，新訊息掉出來</p>
      </div>

      <div class="message-card" :class="{ reveal: !isCracking }">
        <div class="message-top">
          <span class="tag">{{ current.tag }}</span>
          <span class="serial">No. {{ current.id }}</span>
        </div>
        <p class="message">{{ current.text }}</p>
        <div class="meta">
          <div>
            <span class="label">幸運色</span>
            <span class="value">{{ current.color }}</span>
          </div>
          <div>
            <span class="label">幸運物</span>
            <span class="value">{{ current.item }}</span>
          </div>
        </div>
      </div>
    </section>

    <div class="actions">
      <button class="draw" type="button" :disabled="isCracking" @click="drawFortune">
        再抽一次
      </button>
      <p class="hint">點一下，換一個新的好運提示</p>
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
  width: 200px;
  height: 140px;
  margin: 0 auto 20px;
  display: grid;
  place-items: center;
}

.cookie-half {
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
  padding: 14px 26px;
  font-size: 1rem;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(120deg, #ff7a59, #f2a951);
  box-shadow: 0 16px 28px rgba(255, 122, 89, 0.35);
  cursor: pointer;
  transition: transform 0.25s ease, box-shadow 0.25s ease;
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
