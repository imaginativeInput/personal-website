<template>
  <section id="projects" class="section projects-section">
    <div class="container">
      <p class="section-label reveal">// 02. projects</p>
      <h2 class="section-title reveal reveal-delay-1">
        My <span class="gradient-text">projects</span>
      </h2>
      <p class="section-subtitle reveal reveal-delay-1">
        Real-world projects — from client websites and automation to game development.
      </p>

      <div class="projects-grid">
        <article
          class="project-card reveal"
          :class="`reveal-delay-${(i % 2) + 1}`"
          v-for="(p, i) in projects"
          :key="p.title"
        >
          <div class="card-top">
            <span class="p-icon" aria-hidden="true">{{ p.icon }}</span>
            <span class="p-status" :class="{ 'status-completed': p.status === 'Completed' }">
              <span class="status-dot" :class="{ 'dot-completed': p.status === 'Completed' }"></span>
              {{ p.status }}
            </span>
          </div>

          <h3 class="p-title">{{ p.title }}</h3>
          <p class="p-desc">{{ p.description }}</p>

          <ul class="p-features">
            <li v-for="f in p.features" :key="f">
              <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>
              {{ f }}
            </li>
          </ul>

          <div class="p-tags">
            <span class="tag" v-for="t in p.stack" :key="t">{{ t }}</span>
          </div>

          <a v-if="p.link" :href="p.link" target="_blank" rel="noopener noreferrer" class="p-link">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>
            Visit website
          </a>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup>
const projects = [
  {
    icon: '🏠',
    title: 'Domek Rzepiska — Vacation Rental',
    status: 'Completed',
    link: 'https://domekrzepiska.pl/',
    description:
      'A complete vacation rental website with a polished layout, interactive image viewer for property photos, and a reservation form with email integration.',
    features: [
      'Interactive image gallery & lightbox viewer',
      'Reservation form with email sender',
      'Clean, responsive layout',
      'SEO-friendly structure',
    ],
    stack: ['Vue.js', 'HTML', 'CSS', 'JavaScript', 'FastAPI'],
  },
  {
    icon: '🍝',
    title: 'Italian Restaurant Website with CMS — SaaS application',
    status: 'In progress',
    link: 'https://szamma-mia.pl/',
    description:
      'A full-featured restaurant website built for an authentic Italian dining experience — elegant design, smooth navigation, and an intuitive reservation and online ordering flow with custom CMS, thoughout database, loyalty and marketing features, and a fast asynchronous API gluing everything together.',
    features: [
      'Responsive, mobile-first design',
      'Interactive menu with category browsing',
      'Online reservation and ordering system',
      'Custom CMS for menu and content management',
    ],
    stack: ['Vue.js', 'HTML', 'CSS', 'JavaScript', 'FastAPI', 'PostgreSQL'],
  },
  {
    icon: '🎮',
    title: '2D Physics Puzzle Game',
    status: 'In progress',
    description:
      'A 2D action/puzzle game with heavy focus on physics simulation — an educational game that teaches physics through fun gameplay, featuring procedural generation and an original soundtrack composed by me.',
    features: [
      'Realistic physics simulation engine',
      'Procedurally generated levels',
      'Educational physics concepts through gameplay',
      'Original soundtrack & sound design',
    ],
    stack: ['Python', 'Pygame', 'TaichiLang', 'Procedural Generation'],
  },
  {
    icon: '🤖',
    title: 'Automated Chatbot System',
    status: 'In progress',
    description:
      'An intelligent outreach automation platform that scrapes and analyzes user profiles, initiates personalised conversations, and generates context-aware AI responses — with minimal manual input.',
    features: [
      'Profile scraping & data enrichment',
      'Automated conversation initiation',
      'Lightweight context database for memory',
      'AI-driven personalized response generation',
    ],
    stack: ['Python', 'JavaScript', 'Tampermonkey', 'FastAPI', 'PostgreSQL'],
  },
]
</script>

<style scoped>
.projects-section {
  background: linear-gradient(180deg, transparent 0%, rgba(99, 102, 241, 0.02) 50%, transparent 100%);
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  margin-top: 52px;
}

.project-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px;
  transition: var(--transition);
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  gap: 0;
}

.project-card::after {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--gradient);
  opacity: 0;
  transition: opacity var(--transition);
  pointer-events: none;
}

.project-card:hover {
  border-color: rgba(99, 102, 241, 0.35);
  transform: translateY(-5px);
  box-shadow: 0 24px 64px rgba(0, 0, 0, 0.35);
}
.project-card:hover::after { opacity: 0.03; }

.card-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 20px;
}

.p-icon {
  font-size: 2.2rem;
  line-height: 1;
}

.p-status {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-family: var(--font-mono);
  font-size: 0.68rem;
  color: var(--terminal-green);
  background: rgba(74, 222, 128, 0.07);
  border: 1px solid rgba(74, 222, 128, 0.2);
  padding: 4px 10px;
  border-radius: 999px;
}

.status-dot {
  width: 6px;
  height: 6px;
  background: var(--terminal-green);
  border-radius: 50%;
  animation: blink 2s ease-in-out infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.3; }
}

.p-title {
  font-family: var(--font-heading);
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 12px;
  line-height: 1.3;
}

.p-desc {
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.72;
  margin-bottom: 22px;
}

.p-features {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 24px;
  flex: 1;
}

.p-features li {
  display: flex;
  align-items: flex-start;
  gap: 9px;
  font-size: 0.85rem;
  color: var(--text-muted);
  line-height: 1.5;
}

.p-features li svg {
  color: var(--accent);
  flex-shrink: 0;
  margin-top: 2px;
}

.p-tags {
  display: flex;
  gap: 7px;
  flex-wrap: wrap;
}

.status-completed {
  color: var(--accent) !important;
  background: rgba(129, 140, 248, 0.07) !important;
  border-color: rgba(129, 140, 248, 0.2) !important;
}

.dot-completed {
  background: var(--accent) !important;
  animation: none !important;
}

.p-link {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  margin-top: 16px;
  font-family: var(--font-mono);
  font-size: 0.78rem;
  color: var(--accent);
  text-decoration: none;
  transition: var(--transition);
  padding: 6px 0;
}

.p-link:hover {
  color: #a5b4fc;
}

.p-link svg {
  flex-shrink: 0;
}

@media (max-width: 768px) {
  .projects-grid { grid-template-columns: 1fr; }
  .project-card { padding: 26px; }
}
</style>
