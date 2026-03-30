<template>
  <section id="contact" class="section">
    <div class="container">
      <div class="contact-layout">

        <!-- Left: text + links -->
        <div class="contact-left">
          <p class="section-label reveal">// 05. contact</p>
          <h2 class="section-title reveal reveal-delay-1">
            Let's <span class="gradient-text">connect</span>
          </h2>
          <p class="contact-desc reveal reveal-delay-1">
            Got a project in mind or just want to talk shop?
            I'm open to freelance work, collaborations, and interesting conversations.
          </p>

          <div class="contact-links reveal reveal-delay-2">
            <a
              class="contact-row"
              v-for="c in contacts"
              :key="c.label"
              :href="c.href"
              target="_blank"
              rel="noopener noreferrer"
            >
              <div class="c-icon" v-html="c.icon"></div>
              <div class="c-text">
                <span class="c-label">{{ c.label }}</span>
                <span class="c-value">{{ c.value }}</span>
              </div>
              <svg class="c-arrow" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
            </a>
          </div>
        </div>

        <!-- Right: CTA card -->
        <div class="contact-right reveal reveal-delay-2">
          <div class="cta-card">
            <div class="cta-badge">
              <span class="cta-dot"></span>
              Available for work
            </div>
            <h3 class="cta-heading">Ready to start something?</h3>
            <p class="cta-body">
              Whether it's a web app, an automation pipeline, or just a chat — send me a message.
            </p>
            <a href="mailto:imaginative.input@gmail.com" class="btn btn-primary cta-btn">
              <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
              Send a message
            </a>
          </div>
        </div>

      </div>
    </div>

    <footer class="footer">
      <div class="container footer-inner">
        <p class="footer-copy">
          Built with <span class="gradient-text">Vue.js</span> &nbsp;·&nbsp; Piotr Łatyński © {{ year }}
        </p>
        <p class="footer-tip">
          <span class="mono-hint">$</span>&nbsp;Try the terminal in the corner ↘
        </p>
      </div>
    </footer>
  </section>
</template>

<script setup>
const year = new Date().getFullYear()

const contacts = [
  {
    label: 'Email',
    value: 'imaginative.input@gmail.com',
    href: 'mailto:imaginative.input@gmail.com',
    icon: `<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>`,
  },
  {
    label: 'GitHub',
    value: 'github.com/imaginativeInput',
    href: 'https://github.com/imaginativeInput',
    icon: `<svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.44 9.8 8.21 11.39.6.11.82-.26.82-.58v-2.23c-3.34.73-4.04-1.42-4.04-1.42-.55-1.39-1.34-1.76-1.34-1.76-1.09-.74.08-.73.08-.73 1.2.08 1.84 1.24 1.84 1.24 1.07 1.83 2.8 1.3 3.49.99.11-.78.42-1.3.76-1.6-2.67-.3-5.47-1.34-5.47-5.93 0-1.31.47-2.38 1.24-3.22-.13-.3-.54-1.52.12-3.18 0 0 1.01-.32 3.3 1.23a11.5 11.5 0 016.01 0c2.29-1.55 3.3-1.23 3.3-1.23.66 1.66.24 2.88.12 3.18.77.84 1.24 1.91 1.24 3.22 0 4.6-2.81 5.63-5.48 5.92.43.37.82 1.1.82 2.22v3.29c0 .32.19.69.8.58C20.57 21.8 24 17.3 24 12c0-6.63-5.37-12-12-12z"/></svg>`,
  },
  {
    label: 'Phone',
    value: '+48 514 233 672',
    href: 'tel:+48514233672',
    icon: `<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 01-2.18 2 19.79 19.79 0 01-8.63-3.07A19.5 19.5 0 013.07 10.8 19.79 19.79 0 01.01 2.18 2 2 0 012 0h3a2 2 0 012 1.72c.13 1 .36 1.98.7 2.92a2 2 0 01-.45 2.11L6.09 7.91a16 16 0 006 6l1.16-1.16a2 2 0 012.11-.45c.94.34 1.92.57 2.92.7A2 2 0 0122 14.92v2z"/></svg>`,
  },
  {
    label: 'Location',
    value: 'Piaseczno, Masovian, Poland',
    href: '#contact',
    icon: `<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13S3 17 3 10a9 9 0 0118 0z"/><circle cx="12" cy="10" r="3"/></svg>`,
  },
]
</script>

<style scoped>
.contact-layout {
  display: grid;
  grid-template-columns: 1.2fr 0.85fr;
  gap: 72px;
  align-items: start;
}

.contact-desc {
  color: var(--text-muted);
  line-height: 1.8;
  font-size: 0.95rem;
  margin-bottom: 36px;
  max-width: 460px;
}

.contact-links {
  display: flex;
  flex-direction: column;
  gap: 11px;
}

.contact-row {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 15px 18px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  transition: var(--transition);
}
.contact-row:hover {
  border-color: rgba(99, 102, 241, 0.38);
  transform: translateX(4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.c-icon {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(99, 102, 241, 0.08);
  border-radius: 8px;
  color: var(--accent);
  flex-shrink: 0;
}

.c-text {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 2px;
  min-width: 0;
}

.c-label {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.c-value {
  font-size: 0.88rem;
  color: var(--text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.c-arrow {
  color: var(--text-muted);
  transition: var(--transition);
  flex-shrink: 0;
}
.contact-row:hover .c-arrow {
  color: var(--accent);
  transform: translateX(3px);
}

/* CTA card */
.cta-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px;
  position: sticky;
  top: 100px;
}

.cta-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--terminal-green);
  background: rgba(74, 222, 128, 0.07);
  border: 1px solid rgba(74, 222, 128, 0.2);
  padding: 5px 12px;
  border-radius: 999px;
  margin-bottom: 22px;
}

.cta-dot {
  width: 7px;
  height: 7px;
  background: var(--terminal-green);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.4; transform: scale(0.75); }
}

.cta-heading {
  font-family: var(--font-heading);
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 12px;
  line-height: 1.3;
}

.cta-body {
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.7;
  margin-bottom: 26px;
}

.cta-btn {
  width: 100%;
  justify-content: center;
}

/* Footer */
.footer {
  margin-top: 90px;
  padding: 24px 0;
  border-top: 1px solid rgba(63, 63, 70, 0.4);
}

.footer-inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
}

.footer-copy {
  font-size: 0.82rem;
  color: var(--text-muted);
}

.footer-tip {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--text-muted);
}

.mono-hint { color: var(--terminal-green); }

@media (max-width: 968px) {
  .contact-layout {
    grid-template-columns: 1fr;
    gap: 44px;
  }
  .cta-card { position: static; }
}

@media (max-width: 640px) {
  .footer-inner { flex-direction: column; text-align: center; }
  .footer-tip { display: none; }
}
</style>
