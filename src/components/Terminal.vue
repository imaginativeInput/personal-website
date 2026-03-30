<template>
  <div class="terminal-widget" :class="{ open: !minimized }" aria-label="Site terminal" role="complementary">

    <!-- Toggle button -->
    <button
      class="term-toggle"
      @click="minimized = !minimized"
      :aria-expanded="!minimized"
      :title="minimized ? 'Open terminal' : 'Minimize terminal'"
    >
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="4 17 10 11 4 5"/><line x1="12" y1="19" x2="20" y2="19"/>
      </svg>
      <span>terminal</span>
    </button>

    <!-- Terminal panel -->
    <transition name="term-slide">
      <div class="term-panel" v-if="!minimized" role="log" aria-live="polite">

        <!-- macOS-style titlebar -->
        <div class="term-titlebar">
          <div class="macos-dots">
            <button class="dot dot-red"    @click="minimized = true" aria-label="Close terminal" title="Close"></button>
            <button class="dot dot-yellow" @click="minimized = true" aria-label="Minimize terminal" title="Minimize"></button>
            <button class="dot dot-green"  aria-label="" title=""></button>
          </div>
          <span class="term-title">piotr@portfolio — terminal</span>
          <div class="title-spacer"></div>
        </div>

        <!-- Output -->
        <div class="term-output" ref="outputEl">
          <div class="term-welcome">
            <span class="tc-green">Welcome!</span>
            Type <span class="tc-accent">help</span> for available commands.
            Use <span class="tc-accent">↑↓</span> for history, <span class="tc-accent">Tab</span> to autocomplete.
          </div>

          <div v-for="(entry, i) in history" :key="i" class="term-entry">
            <div v-if="entry.type === 'cmd'" class="term-cmd-line">
              <span class="tc-prompt">piotr<span class="tc-dim">@</span>portfolio<span class="tc-dim">:~$</span>&nbsp;</span>
              <span class="tc-white">{{ entry.text }}</span>
            </div>
            <div v-else-if="entry.type === 'out'" class="term-out" v-html="entry.html"></div>
            <div v-else-if="entry.type === 'err'" class="term-err">
              <span class="tc-red">bash:</span> {{ entry.text }}
            </div>
          </div>
        </div>

        <!-- Input row -->
        <div class="term-input-row" @click="focusInput">
          <span class="tc-prompt">piotr<span class="tc-dim">@</span>portfolio<span class="tc-dim">:~$</span>&nbsp;</span>
          <input
            ref="inputEl"
            v-model="cmdInput"
            @keydown="onKey"
            class="term-input"
            autocomplete="off"
            autocorrect="off"
            autocapitalize="off"
            spellcheck="false"
            placeholder="type a command…"
            aria-label="Terminal input"
          />
        </div>

      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, nextTick, watch } from 'vue'

const props = defineProps({ sections: { type: Array, default: () => [] } })
const emit = defineEmits(['navigate'])

const minimized = ref(true)
const cmdInput  = ref('')
const history   = ref([])
const outputEl  = ref(null)
const inputEl   = ref(null)
const cmdLog    = ref([])
const logCursor = ref(-1)

watch(minimized, async (val) => {
  if (!val) {
    await nextTick()
    inputEl.value?.focus()
  }
})

function focusInput() {
  inputEl.value?.focus()
}

// ── Keyboard handler ──────────────────────────────────────────────────────────
function onKey(e) {
  if (e.key === 'Enter') {
    const cmd = cmdInput.value.trim()
    cmdInput.value = ''
    logCursor.value = -1
    run(cmd)
  } else if (e.key === 'ArrowUp') {
    e.preventDefault()
    if (logCursor.value < cmdLog.value.length - 1) {
      logCursor.value++
      cmdInput.value = cmdLog.value[cmdLog.value.length - 1 - logCursor.value]
    }
  } else if (e.key === 'ArrowDown') {
    e.preventDefault()
    if (logCursor.value > 0) {
      logCursor.value--
      cmdInput.value = cmdLog.value[cmdLog.value.length - 1 - logCursor.value]
    } else {
      logCursor.value = -1
      cmdInput.value = ''
    }
  } else if (e.key === 'Tab') {
    e.preventDefault()
    autocomplete()
  }
}

// ── Autocomplete ──────────────────────────────────────────────────────────────
const ALL_CMDS = [
  'help', 'ls', 'whoami', 'skills', 'projects', 'contact', 'github', 'clear', 'goto', 'cd',
  ...props.sections,
]

function autocomplete() {
  const v = cmdInput.value.toLowerCase()
  if (!v) return
  const hits = ALL_CMDS.filter(c => c.startsWith(v))
  if (hits.length === 1) {
    cmdInput.value = hits[0]
  } else if (hits.length > 1) {
    push('out', `<span class="tc-dim">${hits.join('&nbsp;&nbsp;')}</span>`)
    scroll()
  }
}

// ── Command runner ────────────────────────────────────────────────────────────
function run(raw) {
  if (!raw) return
  cmdLog.value.push(raw)
  history.value.push({ type: 'cmd', text: raw })

  const [cmd, ...args] = raw.toLowerCase().split(/\s+/)

  if (cmd === 'clear') {
    history.value = []
    scroll()
    return
  }

  const handlers = {
    help:     cmdHelp,
    ls:       cmdLs,
    whoami:   cmdWhoami,
    skills:   cmdSkills,
    projects: cmdProjects,
    contact:  cmdContact,
    github:   cmdGithub,
    goto:     () => cmdGoto(args[0]),
    cd:       () => cmdGoto(args[0]),
  }

  if (props.sections.includes(cmd)) {
    cmdGoto(cmd)
    return
  }

  if (handlers[cmd]) {
    handlers[cmd]()
  } else {
    push('err', `command not found: ${raw}. Type help for available commands.`)
  }

  scroll()
}

// ── Commands ──────────────────────────────────────────────────────────────────
function cmdHelp() {
  push('out', `<div class="tc-block">
  <div class="tc-green" style="margin-bottom:6px">Available commands:</div>
  <div class="tc-grid">
    <span class="tc-accent">help</span>            <span class="tc-dim">show this message</span>
    <span class="tc-accent">ls</span>              <span class="tc-dim">list all sections</span>
    <span class="tc-accent">goto &lt;section&gt;</span> <span class="tc-dim">navigate to section</span>
    <span class="tc-accent">whoami</span>          <span class="tc-dim">display bio</span>
    <span class="tc-accent">skills</span>          <span class="tc-dim">list tech stack</span>
    <span class="tc-accent">projects</span>        <span class="tc-dim">show current work</span>
    <span class="tc-accent">contact</span>         <span class="tc-dim">contact info</span>
    <span class="tc-accent">github</span>          <span class="tc-dim">open GitHub profile</span>
    <span class="tc-accent">clear</span>           <span class="tc-dim">clear terminal</span>
  </div>
  <div class="tc-dim" style="margin-top:8px">Tip: type a section name directly to navigate.</div>
</div>`)
}

function cmdLs() {
  const items = props.sections.map(s => `<span class="tc-accent">📁 ${s}/</span>`).join('')
  push('out', `<div class="tc-dim" style="margin-bottom:3px">Sections:</div><div class="tc-ls">${items}</div>`)
}

function cmdWhoami() {
  push('out', `<div class="tc-block">
  <div><span class="tc-green">Piotr Łatyński</span> — Software Developer</div>
  <div class="tc-dim">📍 Piaseczno, Masovian, Poland</div>
  <div class="tc-dim">🐍 Python · JavaScript · Vue.js · FastAPI · Playwright · PostgreSQL</div>
  <div class="tc-dim">✉  imaginative.input@gmail.com</div>
  <div class="tc-dim">🐙 github.com/imaginativeInput</div>
</div>`)
}

function cmdSkills() {
  push('out', `<div class="tc-block">
  <div class="tc-green" style="margin-bottom:4px">Tech Stack:</div>
  <div><span class="tc-accent">Languages  </span> <span class="tc-dim">Python · JavaScript · SQL</span></div>
  <div><span class="tc-accent">Frontend   </span> <span class="tc-dim">Vue.js · HTML/CSS</span></div>
  <div><span class="tc-accent">Backend    </span> <span class="tc-dim">FastAPI · PostgreSQL · SQLite</span></div>
  <div><span class="tc-accent">Testing    </span> <span class="tc-dim">Playwright · Selenium · PyTest · Postman</span></div>
  <div><span class="tc-accent">Tools      </span> <span class="tc-dim">Git · Azure DevOps · Jira · Swagger</span></div>
  <div><span class="tc-accent">Specialised</span> <span class="tc-dim">Taichi Lang · OCR/CV</span></div>
</div>`)
}

function cmdProjects() {
  push('out', `<div class="tc-block">
  <div class="tc-green" style="margin-bottom:4px">Projects:</div>
  <div>🏠 <span class="tc-accent">Domek Rzepiska — Vacation Rental</span> <span class="tc-dim">[completed]</span></div>
  <div class="tc-dim" style="padding-left:20px">Vue.js · FastAPI · domekrzepiska.pl</div>
  <div style="margin-top:5px">🍝 <span class="tc-accent">Italian Restaurant Website</span> <span class="tc-dim">[in progress]</span></div>
  <div class="tc-dim" style="padding-left:20px">Vue.js · FastAPI · CSS</div>
  <div style="margin-top:5px">🎮 <span class="tc-accent">2D Physics Puzzle Game</span> <span class="tc-dim">[in progress]</span></div>
  <div class="tc-dim" style="padding-left:20px">Python · Pygame · Procedural Generation</div>
  <div style="margin-top:5px">🤖 <span class="tc-accent">Automated Chatbot System</span> <span class="tc-dim">[in progress]</span></div>
  <div class="tc-dim" style="padding-left:20px">Python · Playwright · PostgreSQL</div>
</div>`)
}

function cmdContact() {
  push('out', `<div class="tc-block">
  <div class="tc-green" style="margin-bottom:4px">Contact:</div>
  <div>✉  <span class="tc-accent">imaginative.input@gmail.com</span></div>
  <div>📱 <span class="tc-accent">+48 514 233 672</span></div>
  <div>🐙 <span class="tc-accent">github.com/imaginativeInput</span></div>
  <div>📍 <span class="tc-dim">Piaseczno, Masovian, Poland</span></div>
</div>`)
}

function cmdGithub() {
  window.open('https://github.com/imaginativeInput', '_blank', 'noopener,noreferrer')
  push('out', '<span class="tc-green">✓ Opening GitHub profile…</span>')
}

function cmdGoto(target) {
  if (!target) {
    push('err', `Usage: goto <section>  |  sections: ${props.sections.join(', ')}`)
    return
  }
  if (props.sections.includes(target)) {
    emit('navigate', target)
    push('out', `<span class="tc-green">✓ Navigating to</span> <span class="tc-accent">${target}</span>`)
  } else {
    push('err', `Section "${target}" not found. Available: ${props.sections.join(', ')}`)
  }
}

// ── Helpers ───────────────────────────────────────────────────────────────────
function push(type, content) {
  if (type === 'out') history.value.push({ type: 'out', html: content })
  else history.value.push({ type: 'err', text: content })
}

async function scroll() {
  await nextTick()
  if (outputEl.value) outputEl.value.scrollTop = outputEl.value.scrollHeight
}
</script>

<style scoped>
/* ── Widget wrapper ─────────────────────────────────────────────────────────── */
.terminal-widget {
  position: fixed;
  bottom: 24px;
  right: 24px;
  z-index: 900;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 10px;
  pointer-events: none;
}
.terminal-widget > * { pointer-events: all; }

/* ── Toggle button ──────────────────────────────────────────────────────────── */
.term-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 9px 16px;
  background: rgba(24, 24, 27, 0.9);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid var(--border);
  border-radius: 999px;
  color: var(--terminal-green);
  font-family: var(--font-mono);
  font-size: 0.78rem;
  cursor: pointer;
  transition: var(--transition);
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
}
.term-toggle:hover {
  border-color: var(--terminal-green);
  background: rgba(24, 24, 27, 0.95);
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(74, 222, 128, 0.15);
}

/* ── Panel ──────────────────────────────────────────────────────────────────── */
.term-panel {
  width: 440px;
  max-height: 370px;
  background: rgba(12, 12, 18, 0.92);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid rgba(63, 63, 70, 0.7);
  border-radius: var(--radius);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  font-family: var(--font-mono);
  font-size: 0.76rem;
  line-height: 1.65;
  box-shadow:
    0 24px 80px rgba(0, 0, 0, 0.6),
    0 0 0 1px rgba(255, 255, 255, 0.04);
}

/* ── macOS titlebar ─────────────────────────────────────────────────────────── */
.term-titlebar {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 11px 14px;
  background: rgba(255, 255, 255, 0.03);
  border-bottom: 1px solid rgba(63, 63, 70, 0.5);
  flex-shrink: 0;
}

.macos-dots {
  display: flex;
  gap: 7px;
  flex-shrink: 0;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  transition: filter var(--transition);
  padding: 0;
}
.dot:hover { filter: brightness(1.2); }
.dot-red    { background: #ff5f57; }
.dot-yellow { background: #febc2e; }
.dot-green  { background: #28c840; cursor: default; }

.term-title {
  flex: 1;
  text-align: center;
  font-size: 0.7rem;
  color: var(--text-muted);
  opacity: 0.6;
  pointer-events: none;
  margin-left: -44px;
}

.title-spacer { width: 44px; flex-shrink: 0; }

/* ── Output area ────────────────────────────────────────────────────────────── */
.term-output {
  flex: 1;
  overflow-y: auto;
  padding: 12px 16px;
  display: flex;
  flex-direction: column;
  gap: 5px;
  min-height: 0;
  scrollbar-width: thin;
  scrollbar-color: var(--surface-2) transparent;
}
.term-output::-webkit-scrollbar { width: 3px; }
.term-output::-webkit-scrollbar-thumb { background: var(--surface-2); border-radius: 2px; }

.term-welcome {
  color: var(--text-muted);
  opacity: 0.7;
  line-height: 1.7;
  padding-bottom: 5px;
  border-bottom: 1px solid rgba(63, 63, 70, 0.3);
  margin-bottom: 3px;
  font-size: 0.73rem;
}

.term-entry { display: flex; flex-direction: column; gap: 2px; }

.term-cmd-line {
  display: flex;
  flex-wrap: wrap;
  gap: 0;
  align-items: baseline;
}

.term-out  { padding-left: 2px; color: #d4d4d8; }
.term-err  { color: #f87171; padding-left: 2px; }

/* ── Input row ──────────────────────────────────────────────────────────────── */
.term-input-row {
  display: flex;
  align-items: center;
  padding: 9px 16px;
  border-top: 1px solid rgba(63, 63, 70, 0.4);
  flex-shrink: 0;
  cursor: text;
}

.term-input {
  flex: 1;
  background: none;
  border: none;
  outline: none;
  color: var(--terminal-green);
  font-family: var(--font-mono);
  font-size: 0.76rem;
  caret-color: var(--terminal-green);
  min-width: 0;
}
.term-input::placeholder { color: rgba(63, 63, 70, 0.8); }

/* ── Slide transition ────────────────────────────────────────────────────────── */
.term-slide-enter-active,
.term-slide-leave-active {
  transition: opacity 0.22s ease, transform 0.22s cubic-bezier(0.4, 0, 0.2, 1);
}
.term-slide-enter-from,
.term-slide-leave-to {
  opacity: 0;
  transform: translateY(12px) scale(0.98);
}

/* ── Color utilities (deep for v-html) ───────────────────────────────────────── */
:deep(.tc-green)  { color: var(--terminal-green); }
:deep(.tc-accent) { color: var(--accent); }
:deep(.tc-dim)    { color: var(--text-muted); }
:deep(.tc-red)    { color: #f87171; }

.tc-white  { color: #e4e4e7; }
.tc-prompt { color: var(--text-muted); white-space: nowrap; user-select: none; font-size: 0.72rem; }
.tc-dim    { color: var(--text-muted); font-size: 0.72rem; }
.tc-red    { color: #f87171; }
.tc-green  { color: var(--terminal-green); }
.tc-accent { color: var(--accent); }

:deep(.tc-block) {
  display: flex;
  flex-direction: column;
  gap: 3px;
  padding: 2px 0;
}

:deep(.tc-grid) {
  display: grid;
  grid-template-columns: max-content 1fr;
  gap: 3px 16px;
  padding-left: 6px;
  margin-top: 4px;
}

:deep(.tc-ls) {
  display: flex;
  flex-wrap: wrap;
  gap: 7px 18px;
  padding: 3px 0;
}

/* ── Mobile ─────────────────────────────────────────────────────────────────── */
@media (max-width: 768px) {
  .terminal-widget {
    bottom: 16px;
    right: 16px;
    left: 16px;
    align-items: stretch;
  }
  .term-toggle { align-self: flex-end; }
  .term-panel {
    width: 100%;
    max-height: 300px;
    font-size: 0.72rem;
    border-radius: var(--radius);
  }
}
</style>
