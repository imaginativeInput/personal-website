<template>
  <section id="experience" class="section">
    <div class="container">
      <p class="section-label reveal">// 03. experience</p>
      <h2 class="section-title reveal reveal-delay-1">
        Work <span class="gradient-text">history</span>
      </h2>

      <div class="timeline">
        <div
          class="timeline-item reveal"
          :class="`reveal-delay-${i + 1}`"
          v-for="(exp, i) in experience"
          :key="exp.role"
        >
          <div class="tl-marker">
            <div class="tl-dot"></div>
            <div class="tl-line" v-if="i < experience.length - 1"></div>
          </div>

          <div class="tl-card">
            <div class="tl-header">
              <div class="tl-title-block">
                <h3 class="tl-role">{{ exp.role }}</h3>
                <p class="tl-company">{{ exp.company }}</p>
              </div>
              <span class="tl-period">{{ exp.period }}</span>
            </div>

            <ul class="tl-points">
              <li v-for="pt in exp.points" :key="pt">{{ pt }}</li>
            </ul>

            <div class="tl-tags">
              <span class="tag" v-for="t in exp.tags" :key="t">{{ t }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const experience = [
  {
    role: 'Freelance Software Developer',
    company: 'Self-employed',
    period: '2023 — present',
    points: [
      'Python & JavaScript automation, workflow tooling, and OCR pipeline development.',
      'Full-stack development and testing — Vue.js, FastAPI, PostgreSQL, HTML, CSS.',
      'Built a 2D Python game with procedural generation and physics simulation; used Taichi Lang for parallelization.',
    ],
    tags: ['Python', 'JavaScript', 'Vue.js', 'FastAPI', 'Automation', 'Taichi Lang', 'PostgreSQL', 'Selenium', 'Playwright', 'PyTest'],
  },
  {
    role: 'Software QA Tester',
    company: 'Lionbridge',
    period: '2020 — 2022',
    points: [
      'Test case design and execution across Windows, console, and VR platforms.',
      'API testing with Postman; network traffic analysis with Fiddler.',
      'Test management and reporting via Azure DevOps; participated in Agile sprints.',
    ],
    tags: ['QA', 'Postman', 'Azure DevOps', 'Jira', 'Agile', 'Fiddler'],
  },
  {
    role: 'Website Maintenance Technician',
    company: 'Creo S.C.',
    period: '2020',
    points: [
      'WordPress maintenance, plugin updates, and security monitoring for a local business website.',
      'Frontend modifications (HTML/CSS), and content updates to enhance user experience.',
      'System integration (payments, email automation) and performance optimization (caching, image compression).',
      'Manual Testing, and customer support.',
      'Implemented SEO best practices, improving search engine rankings and organic traffic.',
    ],
    tags: ['Testing', 'JIRA', 'Mantis', 'Cross-platform', 'Mobile'],
  },
]
</script>

<style scoped>
.timeline {
  margin-top: 52px;
  display: flex;
  flex-direction: column;
}

.timeline-item {
  display: grid;
  grid-template-columns: 36px 1fr;
  gap: 24px;
}

.tl-marker {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 8px;
}

.tl-dot {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: var(--gradient);
  flex-shrink: 0;
  box-shadow: 0 0 14px rgba(99, 102, 241, 0.5);
  position: relative;
  z-index: 1;
}

.tl-line {
  width: 1px;
  flex: 1;
  min-height: 32px;
  background: linear-gradient(to bottom, rgba(99, 102, 241, 0.5), rgba(99, 102, 241, 0.05));
  margin: 8px 0;
}

.tl-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 28px 32px;
  margin-bottom: 24px;
  transition: var(--transition);
}
.tl-card:hover {
  border-color: rgba(99, 102, 241, 0.3);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}

.tl-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  margin-bottom: 18px;
  flex-wrap: wrap;
}

.tl-role {
  font-family: var(--font-heading);
  font-size: 1.05rem;
  font-weight: 600;
  margin-bottom: 4px;
}

.tl-company {
  font-family: var(--font-mono);
  font-size: 0.78rem;
  color: var(--accent);
}

.tl-period {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--text-muted);
  background: var(--surface-2);
  border: 1px solid var(--border);
  padding: 4px 11px;
  border-radius: 999px;
  white-space: nowrap;
  flex-shrink: 0;
}

.tl-points {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 9px;
  margin-bottom: 18px;
}

.tl-points li {
  font-size: 0.88rem;
  color: var(--text-muted);
  padding-left: 18px;
  position: relative;
  line-height: 1.65;
}
.tl-points li::before {
  content: '›';
  position: absolute;
  left: 2px;
  color: var(--accent);
  font-size: 1rem;
  line-height: 1.4;
}

.tl-tags {
  display: flex;
  gap: 7px;
  flex-wrap: wrap;
}

@media (max-width: 640px) {
  .timeline-item {
    grid-template-columns: 24px 1fr;
    gap: 14px;
  }
  .tl-card { padding: 20px; }
  .tl-header { flex-direction: column; }
}
</style>
