<template>
  <section id="about" class="section">
    <div class="container">
      <div class="about-grid">

        <!-- Text side -->
        <div class="about-text">
          <p class="section-label reveal">// 01. about</p>
          <h2 class="section-title reveal reveal-delay-1">
            A little about <span class="gradient-text">me</span>
          </h2>
          <p class="about-desc reveal reveal-delay-2">
            I'm a software developer from Piaseczno, Poland with a background in both development
            and quality assurance. I have a strong eye for detail — a habit built from years of
            professional testing — and I apply it to everything I write.
          </p>
          <p class="about-desc reveal reveal-delay-2">
            My work spans automation scripts, REST APIs, and full-featured responsive applications. I enjoy finding
            the simplest path through a complex problem, and I'm always looking for things to
            learn along the way.
          </p>

          <div class="about-meta reveal reveal-delay-3">
            <div class="meta-item" v-for="m in meta" :key="m.label">
              <span class="meta-label">{{ m.label }}</span>
              <span class="meta-value">{{ m.value }}</span>
            </div>
          </div>

          <div class="about-hobbies reveal reveal-delay-3">
            <span class="meta-label">Interests</span>
            <div class="hobby-list">
              <span class="hobby-chip" v-for="h in hobbies" :key="h">{{ h }}</span>
            </div>
          </div>
        </div>

        <!-- Code card side -->
        <div class="about-visual reveal reveal-delay-2">
          <div class="code-card">
            <div class="code-titlebar">
              <div class="mac-dots">
                <span class="mac-dot r"></span>
                <span class="mac-dot y"></span>
                <span class="mac-dot g"></span>
              </div>
              <span class="code-filename">piotr.py</span>
            </div>
            <pre class="code-content"><code
><span class="kw">class</span> <span class="cls">Developer</span>:

    <span class="kw">def</span> <span class="fn">__init__</span>(self):
        self.name     <span class="op">=</span> <span class="str">"Piotr Łatyński"</span>
        self.location <span class="op">=</span> <span class="str">"Piaseczno, PL"</span>
        self.stack    <span class="op">=</span> [
            <span class="str">"Python"</span>, <span class="str">"Vue.js"</span>,
            <span class="str">"FastAPI"</span>, <span class="str">"Playwright"</span>,
        ]
        self.status   <span class="op">=</span> <span class="str">"open to work"</span>

    <span class="kw">def</span> <span class="fn">approach</span>(self, problem):
        <span class="kw">return</span> (
            self.understand(problem)
                .automate()
                .test()
                .ship()
        )</code></pre>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
const meta = [
  { label: 'Location', value: 'Piaseczno, Masovian, Poland' },
  { label: 'Email',    value: 'imaginative.input@gmail.com' },
  { label: 'GitHub',   value: 'github.com/imaginativeInput' },
  { label: 'Languages', value: 'Polish & English' },
]

const hobbies = [
  'Songwriting', 'Visual Arts', 'Gym', 'Parkour', 'Physics', 'Robotics', 'Gaming',
]
</script>

<style scoped>
.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: center;
}

.about-desc {
  color: var(--text-muted);
  line-height: 1.82;
  font-size: 0.95rem;
  margin-bottom: 18px;
}

.about-meta {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px 24px;
  margin: 30px 0 22px;
}

.meta-item {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.meta-label {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 0.12em;
}

.meta-value {
  font-size: 0.85rem;
  color: var(--text-muted);
}

.about-hobbies {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.hobby-list {
  display: flex;
  gap: 7px;
  flex-wrap: wrap;
}

.hobby-chip {
  padding: 4px 12px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 999px;
  font-size: 0.78rem;
  color: var(--text-muted);
  transition: var(--transition);
}
.hobby-chip:hover {
  border-color: var(--accent);
  color: var(--text);
}

/* Code card */
.about-visual {
  position: relative;
}

.code-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: 0 32px 80px rgba(0, 0, 0, 0.45), 0 0 0 1px rgba(255, 255, 255, 0.03);
}

.code-titlebar {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 18px;
  background: var(--surface-2);
  border-bottom: 1px solid var(--border);
}

.mac-dots { display: flex; gap: 6px; }
.mac-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: block;
}
.mac-dot.r { background: #ff5f57; }
.mac-dot.y { background: #febc2e; }
.mac-dot.g { background: #28c840; }

.code-filename {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--text-muted);
  margin-left: 4px;
}

.code-content {
  padding: 22px 24px;
  font-family: var(--font-mono);
  font-size: 0.8rem;
  line-height: 1.85;
  overflow-x: auto;
}

.kw  { color: #c792ea; }
.cls { color: #82aaff; }
.fn  { color: #82aaff; }
.str { color: #c3e88d; }
.op  { color: #89ddff; }

@media (max-width: 1024px) {
  .about-grid { gap: 48px; }
}

@media (max-width: 768px) {
  .about-grid { grid-template-columns: 1fr; }
  .about-visual { display: none; }
  .about-meta { grid-template-columns: 1fr; }
}
</style>
