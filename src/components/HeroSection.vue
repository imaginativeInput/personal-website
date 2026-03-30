<template>
  <section id="home" class="hero">
    <!-- Background -->
    <div class="hero-bg" aria-hidden="true">
      <div class="grid"></div>
      <div class="glow g1"></div>
      <div class="glow g2"></div>
    </div>

    <div class="container hero-body">
      <div class="hero-left">
        <div class="hero-badge reveal">
          <span class="badge-dot"></span>
          Available for freelance projects
        </div>

        <h1 class="hero-name reveal reveal-delay-1">
          Piotr<br>
          <span class="gradient-text">Łatyński</span>
        </h1>

        <p class="hero-role reveal reveal-delay-2">
          <span class="role-prefix">$ whoami&nbsp;&gt;&nbsp;</span>
          <span class="role-value">{{ displayedRole }}</span>
          <span class="cursor" :class="{ blink: !isTyping }">▋</span>
        </p>

        <p class="hero-tagline reveal reveal-delay-2">
          Turning complex problems into clean automation, tested APIs, and polished full-featured applications.
        </p>

        <div class="hero-actions reveal reveal-delay-3">
          <button class="btn btn-primary" @click="$emit('navigate', 'projects')">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M13 10V3L4 14h7v7l9-11h-7z"/></svg>
            View projects
          </button>
          <button class="btn btn-secondary" @click="$emit('navigate', 'contact')">
            Get in touch
          </button>
        </div>

        <div class="hero-stack reveal reveal-delay-3">
          <span class="stack-label">stack //</span>
          <div class="stack-tags">
            <span class="tag" v-for="t in stack" :key="t">{{ t }}</span>
          </div>
        </div>
      </div>

      <!-- Profile picture -->
      <div class="hero-right reveal reveal-delay-2">
        <div class="pic-frame">
          <!-- Corner brackets -->
          <span class="cb tl" aria-hidden="true"></span>
          <span class="cb tr" aria-hidden="true"></span>
          <span class="cb bl" aria-hidden="true"></span>
          <span class="cb br" aria-hidden="true"></span>

          <div class="pic-inner">
            <img
              src="/profile-pic.png"
              alt="Piotr Łatyński"
              class="pic-img"
              loading="eager"
            />
            <div class="pic-overlay" aria-hidden="true"></div>
          </div>

          <!-- HUD label beneath image -->
          <div class="pic-hud" aria-hidden="true">
            <span class="hud-dot"></span>
            <span class="hud-text">PIOTR&nbsp;ŁATYŃSKI</span>
            <span class="hud-sep">//</span>
            <span class="hud-text">DEV</span>
          </div>
        </div>
      </div>
    </div>

    <button class="scroll-cue" @click="$emit('navigate', 'about')" aria-label="Scroll to about">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 5v14M5 12l7 7 7-7"/></svg>
    </button>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

defineEmits(['navigate'])

const roles = ['Software Developer', 'Automation Engineer', 'QA Specialist', 'Freelancer']
const stack = ['Python', 'Vue.js', 'FastAPI', 'Playwright', 'PostgreSQL']

const displayedRole = ref('')
const isTyping = ref(true)
let roleIdx = 0
let charIdx = 0
let dir = 1
let timeout = null

function tick() {
  const word = roles[roleIdx]
  if (dir === 1) {
    isTyping.value = true
    displayedRole.value = word.slice(0, charIdx)
    charIdx++
    if (charIdx > word.length) {
      isTyping.value = false
      timeout = setTimeout(() => { dir = -1; tick() }, 2200)
      return
    }
    timeout = setTimeout(tick, 80)
  } else {
    displayedRole.value = word.slice(0, charIdx)
    charIdx--
    if (charIdx < 0) {
      roleIdx = (roleIdx + 1) % roles.length
      dir = 1
      charIdx = 0
      timeout = setTimeout(tick, 200)
      return
    }
    timeout = setTimeout(tick, 42)
  }
}

onMounted(() => tick())
onUnmounted(() => clearTimeout(timeout))
</script>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 100px 0 80px;
  overflow: hidden;
}

/* Background */
.hero-bg {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}

.grid {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(99, 102, 241, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(99, 102, 241, 0.05) 1px, transparent 1px);
  background-size: 60px 60px;
}

.glow {
  position: absolute;
  border-radius: 50%;
  filter: blur(140px);
  pointer-events: none;
}
.g1 {
  width: 700px;
  height: 700px;
  background: radial-gradient(circle, #7c3aed 0%, transparent 70%);
  opacity: 0.08;
  top: -250px;
  right: -150px;
}
.g2 {
  width: 450px;
  height: 450px;
  background: radial-gradient(circle, #4c1d95 0%, transparent 70%);
  opacity: 0.12;
  bottom: 0;
  left: -50px;
}

/* Two-column layout */
.hero-body {
  position: relative;
  z-index: 1;
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
  gap: 64px;
}

/* Left side */
.hero-left {
  min-width: 0;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 5px 14px;
  background: rgba(74, 222, 128, 0.06);
  border: 1px solid rgba(74, 222, 128, 0.2);
  border-radius: 2px;
  font-family: var(--font-mono);
  font-size: 0.7rem;
  color: var(--terminal-green);
  margin-bottom: 28px;
  letter-spacing: 0.05em;
}

.badge-dot {
  width: 7px;
  height: 7px;
  background: var(--terminal-green);
  border-radius: 50%;
  animation: dot-pulse 2s ease-in-out infinite;
  box-shadow: 0 0 6px var(--terminal-green);
}

@keyframes dot-pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.4; transform: scale(0.7); }
}

.hero-name {
  font-family: var(--font-heading);
  font-size: clamp(3.5rem, 9vw, 7.5rem);
  font-weight: 700;
  line-height: 0.95;
  letter-spacing: -0.03em;
  margin-bottom: 24px;
}

.hero-role {
  font-family: var(--font-mono);
  font-size: clamp(0.8rem, 1.8vw, 0.95rem);
  color: var(--text-muted);
  margin-bottom: 20px;
}
.role-prefix { color: var(--accent); }
.role-value  { color: var(--terminal-green); }
.cursor {
  color: var(--terminal-green);
  font-weight: 700;
  font-size: 0.9em;
}
.cursor.blink { animation: blink 1s step-end infinite; }

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}

.hero-tagline {
  font-size: clamp(0.9rem, 1.8vw, 1.05rem);
  color: var(--text-muted);
  max-width: 460px;
  line-height: 1.75;
  margin-bottom: 40px;
}

.hero-actions {
  display: flex;
  gap: 14px;
  margin-bottom: 44px;
  flex-wrap: wrap;
}

.hero-stack {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}
.stack-label {
  font-family: var(--font-mono);
  font-size: 0.68rem;
  color: var(--text-muted);
  flex-shrink: 0;
}
.stack-tags {
  display: flex;
  gap: 7px;
  flex-wrap: wrap;
}

/* Right side — profile picture */
.hero-right {
  flex-shrink: 0;
}

.pic-frame {
  position: relative;
  display: inline-block;
  padding: 10px;
}

/* HUD corner brackets */
.cb {
  position: absolute;
  width: 22px;
  height: 22px;
  z-index: 2;
}
.cb.tl { top: 0;  left: 0;  border-top: 2px solid var(--accent); border-left: 2px solid var(--accent); }
.cb.tr { top: 0;  right: 0; border-top: 2px solid var(--accent); border-right: 2px solid var(--accent); }
.cb.bl { bottom: 28px; left: 0;  border-bottom: 2px solid var(--accent); border-left: 2px solid var(--accent); }
.cb.br { bottom: 28px; right: 0; border-bottom: 2px solid var(--accent); border-right: 2px solid var(--accent); }

.pic-inner {
  position: relative;
  width: 300px;
  height: 300px;
  overflow: hidden;
}

.pic-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top center;
  display: block;
  filter: brightness(0.88) contrast(1.05) saturate(0.75);
}

/* Subtle vignette overlay */
.pic-overlay {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(to bottom, transparent 60%, rgba(6, 6, 14, 0.6) 100%),
    linear-gradient(to right, rgba(6, 6, 14, 0.15) 0%, transparent 30%, transparent 70%, rgba(6, 6, 14, 0.15) 100%);
  pointer-events: none;
}

/* HUD label below photo */
.pic-hud {
  display: flex;
  align-items: center;
  gap: 7px;
  padding: 5px 8px;
  background: rgba(6, 6, 14, 0.9);
  border: 1px solid rgba(99, 102, 241, 0.25);
  border-top: none;
  font-family: var(--font-mono);
  font-size: 0.62rem;
  letter-spacing: 0.12em;
}

.hud-dot {
  width: 5px;
  height: 5px;
  background: var(--terminal-green);
  border-radius: 50%;
  box-shadow: 0 0 5px var(--terminal-green);
  flex-shrink: 0;
}
.hud-text { color: var(--text-muted); }
.hud-sep  { color: var(--accent); }

/* Scroll cue */
.scroll-cue {
  position: absolute;
  bottom: 28px;
  left: 50%;
  transform: translateX(-50%);
  background: none;
  border: 1px solid var(--border-bright);
  color: var(--text-muted);
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: var(--transition);
  animation: float 2.5s ease-in-out infinite;
  z-index: 1;
  border-radius: 2px;
}
.scroll-cue:hover { border-color: var(--accent); color: var(--accent); }

@keyframes float {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50%       { transform: translateX(-50%) translateY(7px); }
}

/* Responsive */
@media (max-width: 900px) {
  .hero-body {
    grid-template-columns: 1fr;
    text-align: left;
    gap: 40px;
  }
  .hero-right {
    display: flex;
    justify-content: flex-start;
    order: -1;
  }
  .pic-inner { width: 200px; height: 200px; }
  .cb.bl { bottom: 28px; }
  .cb.br { bottom: 28px; }
}

@media (max-width: 640px) {
  .scroll-cue { display: none; }
  .hero { padding-bottom: 48px; }
  .hero-right { display: none; }
}
</style>
