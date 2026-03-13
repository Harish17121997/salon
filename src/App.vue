<template>
  <div id="app">

    <!-- ── STICKY NAV ── -->
    <header :class="['nav', { 'nav--scrolled': scrolled }]">
      <div class="nav__inner">
        <a href="#home" class="nav__logo" @click.prevent="scrollTo('home')">
          <span class="nav__logo-dot"></span>Aura
        </a>
        <div class="nav__actions">
          <a :href="whatsappBook('Quick enquiry')" target="_blank" class="nav__cta">Book Now</a>
        </div>
      </div>
    </header>

    <!-- ── HERO ── -->
    <section id="home" class="hero">
      <div class="hero__bg"></div>
      <div class="hero__content">
        <div class="hero__badge">📍 Pune, Maharashtra</div>
        <h1 class="hero__title">
          Your Best Look,<br /><span>Every Visit.</span>
        </h1>
        <p class="hero__sub">Premium salon & spa services crafted for you. Walk in, transform, walk out glowing.</p>
        <div class="hero__btns">
          <a :href="whatsappBook('I want to book an appointment')" target="_blank" class="btn btn--primary">
            Book Appointment
          </a>
          <button class="btn btn--ghost" @click="scrollTo('services')">Our Services</button>
        </div>
        <div class="hero__stats">
          <div class="hero__stat" v-for="s in stats" :key="s.label">
            <strong>{{ s.value }}</strong>
            <span>{{ s.label }}</span>
          </div>
        </div>
      </div>
      <div class="hero__scroll-hint">
        <span></span>
      </div>
    </section>

    <!-- ── SERVICES ── -->
    <section id="services" class="section">
      <div class="section__head">
        <p class="section__eyebrow">What We Offer</p>
        <h2 class="section__title">Our <em>Services</em></h2>
      </div>
      <div class="services__grid">
        <div
          v-for="svc in services"
          :key="svc.name"
          class="svc-card"
          @click="bookService(svc.name)"
        >
          <div class="svc-card__icon">{{ svc.icon }}</div>
          <div class="svc-card__body">
            <p class="svc-card__name">{{ svc.name }}</p>
            <p class="svc-card__desc">{{ svc.desc }}</p>
          </div>
          <div class="svc-card__right">
            <span class="svc-card__price">{{ svc.price }}</span>
            <span class="svc-card__arrow">→</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ── WHY US ── -->
    <section class="why">
      <div class="why__inner">
        <p class="section__eyebrow" style="color:#b89a5a">Why Choose Us</p>
        <h2 class="section__title" style="color:#fff">Trusted by <em>Thousands</em></h2>
        <div class="why__grid">
          <div class="why__item" v-for="w in whyUs" :key="w.title">
            <div class="why__icon">{{ w.icon }}</div>
            <p class="why__title">{{ w.title }}</p>
            <p class="why__desc">{{ w.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ── TEAM ── -->
    <section id="team" class="section">
      <div class="section__head">
        <p class="section__eyebrow">The Artists</p>
        <h2 class="section__title">Meet the <em>Team</em></h2>
      </div>
      <div class="team__rail">
        <div class="team__card" v-for="m in team" :key="m.name" @click="bookWithStylist(m.name)">
          <div class="team__avatar">{{ m.avatar }}</div>
          <p class="team__name">{{ m.name }}</p>
          <p class="team__role">{{ m.role }}</p>
          <div :class="['team__avail', m.available ? 'team__avail--on' : 'team__avail--off']">
            {{ m.available ? '● Available' : '● Busy today' }}
          </div>
          <button class="team__book">Book →</button>
        </div>
      </div>
    </section>

    <!-- ── REVIEWS ── -->
    <section id="reviews" class="section section--grey">
      <div class="section__head">
        <p class="section__eyebrow">Real Reviews</p>
        <h2 class="section__title">Client <em>Love</em></h2>
      </div>
      <div class="reviews__rail">
        <div class="rev-card" v-for="r in reviews" :key="r.name">
          <div class="rev-card__stars">★★★★★</div>
          <p class="rev-card__text">"{{ r.text }}"</p>
          <div class="rev-card__footer">
            <span class="rev-card__avatar">{{ r.avatar }}</span>
            <div>
              <p class="rev-card__name">{{ r.name }}</p>
              <p class="rev-card__date">{{ r.date }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ── BOOKING FORM ── -->
    <section id="book" class="section">
      <div class="section__head">
        <p class="section__eyebrow">Reserve a Slot</p>
        <h2 class="section__title">Book <em>Appointment</em></h2>
        <p class="section__sub">Fill in the details — we'll confirm via WhatsApp within 30 minutes.</p>
      </div>
      <form class="book-form" @submit.prevent="submitBooking">
        <div class="book-form__field">
          <label>Your Name</label>
          <input v-model="form.name" type="text" placeholder="e.g. Priya Sharma" required />
        </div>
        <div class="book-form__field">
          <label>Phone Number</label>
          <input v-model="form.phone" type="tel" placeholder="+91 98765 43210" required />
        </div>
        <div class="book-form__field">
          <label>Select Service</label>
          <select v-model="form.service" required>
            <option value="" disabled>Choose a service…</option>
            <option v-for="s in services" :key="s.name" :value="s.name">{{ s.name }}</option>
          </select>
        </div>
        <div class="book-form__field">
          <label>Preferred Date</label>
          <input v-model="form.date" type="date" :min="today" required />
        </div>
        <div class="book-form__field">
          <label>Pick a Time</label>
          <div class="time-grid">
            <button
              type="button"
              v-for="t in timeSlots"
              :key="t"
              :class="['time-btn', { 'time-btn--sel': form.time === t }]"
              @click="form.time = t"
            >{{ t }}</button>
          </div>
        </div>
        <button type="submit" class="btn btn--primary btn--full">
          <span>Send via WhatsApp →</span>
        </button>
      </form>
    </section>

    <!-- ── LOCATION ── -->
    <section id="location" class="section section--grey">
      <div class="section__head">
        <p class="section__eyebrow">Find Us</p>
        <h2 class="section__title">Our <em>Location</em></h2>
      </div>
      <div class="location__card">
        <div class="location__map-preview">
          <div class="location__map-pin">📍</div>
          <div class="location__map-label">Aura Salon, Pune</div>
        </div>
        <div class="location__info">
          <div class="location__row">
            <span class="location__icon">📍</span>
            <div>
              <p class="location__lbl">Address</p>
              <p class="location__val">12, FC Road, Shivajinagar,<br />Pune, Maharashtra 411005</p>
            </div>
          </div>
          <div class="location__row">
            <span class="location__icon">🕐</span>
            <div>
              <p class="location__lbl">Hours</p>
              <p class="location__val">Mon–Sat: 10:00 AM – 8:00 PM</p>
              <p class="location__val" style="color:#e05252">Sunday: Closed</p>
            </div>
          </div>
          <div class="location__row">
            <span class="location__icon">🚇</span>
            <div>
              <p class="location__lbl">Nearest Landmark</p>
              <p class="location__val">Near Pune University Circle</p>
            </div>
          </div>
        </div>
        <a
          href="https://maps.google.com/?q=12+FC+Road+Shivajinagar+Pune"
          target="_blank"
          class="btn btn--outline btn--full"
        >Open in Google Maps →</a>
      </div>
    </section>

    <!-- ── CONTACT / GET IN TOUCH ── -->
    <section id="contact" class="section">
      <div class="section__head">
        <p class="section__eyebrow">Contact Us</p>
        <h2 class="section__title">Get in <em>Touch</em></h2>
        <p class="section__sub">Tap any option below — we respond within minutes.</p>
      </div>
      <div class="contact__list">
        <a :href="whatsappBook('Hello! I have an enquiry about your salon services.')" target="_blank" class="contact__item contact__item--wa">
          <div class="contact__item-icon">💬</div>
          <div class="contact__item-text">
            <p class="contact__item-lbl">WhatsApp Us</p>
            <p class="contact__item-val">Tap to chat instantly</p>
          </div>
          <span class="contact__item-arrow">→</span>
        </a>
        <a href="tel:+919876543210" class="contact__item">
          <div class="contact__item-icon">📞</div>
          <div class="contact__item-text">
            <p class="contact__item-lbl">Call Us</p>
            <p class="contact__item-val">+91 98765 43210</p>
          </div>
          <span class="contact__item-arrow">→</span>
        </a>
        <a href="https://instagram.com/aura.salon.pune" target="_blank" class="contact__item">
          <div class="contact__item-icon">📸</div>
          <div class="contact__item-text">
            <p class="contact__item-lbl">Instagram</p>
            <p class="contact__item-val">@aura.salon.pune</p>
          </div>
          <span class="contact__item-arrow">→</span>
        </a>
        <a href="mailto:hello@aurasalon.in" class="contact__item">
          <div class="contact__item-icon">✉️</div>
          <div class="contact__item-text">
            <p class="contact__item-lbl">Email</p>
            <p class="contact__item-val">hello@aurasalon.in</p>
          </div>
          <span class="contact__item-arrow">→</span>
        </a>
      </div>
    </section>

    <!-- ── FOOTER ── -->
    <footer class="footer">
      <div class="footer__logo">
        <span class="footer__logo-dot"></span>Aura
      </div>
      <p class="footer__tagline">Premium Salon & Spa · Pune</p>
      <div class="footer__links">
        <a @click.prevent="scrollTo('services')" href="#">Services</a>
        <a @click.prevent="scrollTo('team')" href="#">Team</a>
        <a @click.prevent="scrollTo('book')" href="#">Book</a>
        <a @click.prevent="scrollTo('contact')" href="#">Contact</a>
      </div>
      <p class="footer__copy">© 2025 Aura Salon. All rights reserved.</p>
    </footer>

    <!-- ── STICKY BOTTOM BAR ── -->
    <div class="bottom-bar">
      <a href="tel:+919876543210" class="bottom-bar__btn bottom-bar__btn--call">
        <span>📞</span> Call
      </a>
      <a :href="whatsappBook('Hi! I want to book an appointment at Aura Salon.')" target="_blank" class="bottom-bar__btn bottom-bar__btn--whatsapp">
        <span>💬</span> WhatsApp
      </a>
      <button class="bottom-bar__btn bottom-bar__btn--book" @click="scrollTo('book')">
        <span>✦</span> Book Now
      </button>
    </div>

    <!-- ── TOAST ── -->
    <transition name="toast-fade">
      <div v-if="toast.show" class="toast">{{ toast.msg }}</div>
    </transition>

  </div>
</template>

<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue'

// ── Config ──────────────────────────────────────────────
const WHATSAPP_NUMBER = '919876543210' // Replace with real number
const SALON_NAME = 'Aura Salon, Pune'

// ── State ────────────────────────────────────────────────
const scrolled = ref(false)
const toast = reactive({ show: false, msg: '' })
const form = reactive({
  name: '', phone: '', service: '', date: '', time: ''
})

// ── Computed ─────────────────────────────────────────────
const today = computed(() => new Date().toISOString().split('T')[0])

// ── Data ─────────────────────────────────────────────────
const stats = [
  { value: '8K+', label: 'Happy Clients' },
  { value: '4.9★', label: 'Google Rating' },
  { value: '12yr', label: 'Experience' },
]

const services = [
  { icon: '✂️', name: 'Haircut & Styling', desc: 'Cut · Blowdry · Finish', price: '₹499+' },
  { icon: '🎨', name: 'Colour & Highlights', desc: 'Global · Balayage · Ombré', price: '₹999+' },
  { icon: '✨', name: 'Keratin Treatment', desc: 'Smoothing · Frizz control', price: '₹2,499+' },
  { icon: '🧖‍♀️', name: 'Facial & Skin Care', desc: 'Glow · Hydration · Cleanse', price: '₹799+' },
  { icon: '💅', name: 'Nail Art & Extensions', desc: 'Gel · French · 3D Art', price: '₹399+' },
  { icon: '💄', name: 'Bridal Makeup', desc: 'Trial · HD · Airbrush', price: '₹3,999+' },
  { icon: '🧴', name: 'Hair Spa & Deep Care', desc: 'Nourish · Repair · Shine', price: '₹699+' },
  { icon: '🪒', name: 'Threading & Waxing', desc: 'Eyebrows · Full body', price: '₹199+' },
]

const whyUs = [
  { icon: '🏆', title: 'Award-Winning Team', desc: 'Trained at top institutes with 10+ years experience each.' },
  { icon: '🌿', title: 'Premium Products', desc: 'We only use internationally certified, skin-safe products.' },
  { icon: '⏱', title: 'On-Time Service', desc: 'Your time matters. We stick to appointments, always.' },
  { icon: '✅', title: '100% Hygienic', desc: 'Fresh tools and sanitized stations for every single client.' },
]

const team = [
  { avatar: '👩‍🦱', name: 'Priya S.', role: 'Senior Stylist', available: true },
  { avatar: '👩‍🦰', name: 'Aisha K.', role: 'Colour Expert', available: true },
  { avatar: '👩', name: 'Riya P.', role: 'Nail Artist', available: false },
  { avatar: '🧑‍🎨', name: 'Dev M.', role: 'Skin Therapist', available: true },
  { avatar: '👩‍🦳', name: 'Neha G.', role: 'Makeup Artist', available: false },
]

const reviews = [
  { avatar: '😊', name: 'Sneha R.', date: '2 weeks ago', text: 'Absolutely love this place! Priya gave me the best haircut I\'ve ever had. The salon is so clean and welcoming.' },
  { avatar: '🙋', name: 'Meera D.', date: '1 month ago', text: 'Got my balayage done and it\'s stunning. The colour lasted 4 months. Highly recommend Aisha for colour work!' },
  { avatar: '👰', name: 'Pooja M.', date: '3 weeks ago', text: 'My bridal makeup was absolutely perfect. I received so many compliments. Worth every rupee!' },
  { avatar: '😍', name: 'Ananya K.', date: '1 week ago', text: 'Fast, hygienic, and they truly listen to what you want. This is my forever salon in Pune.' },
]

const timeSlots = ['10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00', '17:00', '18:00']

// ── Methods ──────────────────────────────────────────────
function whatsappBook(message) {
  const encoded = encodeURIComponent(message)
  return `https://wa.me/${WHATSAPP_NUMBER}?text=${encoded}`
}

function bookService(name) {
  const msg = `Hi! I'd like to book *${name}* at ${SALON_NAME}. Please let me know available slots.`
  window.open(whatsappBook(msg), '_blank')
}

function bookWithStylist(name) {
  const msg = `Hi! I'd like to book an appointment with *${name}* at ${SALON_NAME}. Please share available slots.`
  window.open(whatsappBook(msg), '_blank')
}

function submitBooking() {
  if (!form.time) {
    showToast('⚠️ Please select a time slot')
    return
  }
  const msg =
    `*New Appointment Request* 🗓\n\n` +
    `👤 Name: ${form.name}\n` +
    `📱 Phone: ${form.phone}\n` +
    `✂️ Service: ${form.service}\n` +
    `📅 Date: ${form.date}\n` +
    `⏰ Time: ${form.time}\n\n` +
    `Sent from ${SALON_NAME} website.`
  window.open(whatsappBook(msg), '_blank')
  showToast('✅ Redirecting to WhatsApp…')
  Object.assign(form, { name: '', phone: '', service: '', date: '', time: '' })
}

function scrollTo(id) {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}

function showToast(msg, duration = 3000) {
  toast.msg = msg
  toast.show = true
  setTimeout(() => { toast.show = false }, duration)
}

function handleScroll() {
  scrolled.value = window.scrollY > 40
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
  form.date = today.value
})
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<style>
/* ── RESET & BASE ──────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:      #ffffff;
  --bg2:     #f7f6f4;
  --ink:     #111111;
  --ink2:    #222222;
  --muted:   #777777;
  --border:  #e4e4e0;
  --gold:    #b89a5a;
  --gold-lt: #d4b97a;
  --gold-bg: rgba(184,154,90,0.08);
  --dark:    #16140f;
  --dark2:   #1e1b13;
  --wa:      #25d366;
  --red:     #e05252;
  --radius:  16px;
  --shadow:  0 2px 16px rgba(0,0,0,0.07);
  --ff-head: 'Cormorant Garamond', Georgia, serif;
  --ff-body: 'DM Sans', system-ui, sans-serif;
}

html { scroll-behavior: smooth; font-size: 16px; }

body {
  background: var(--bg);
  color: var(--ink);
  font-family: var(--ff-body);
  font-weight: 400;
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
  padding-bottom: 76px; /* bottom bar */
}

@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600&family=DM+Sans:wght@300;400;500;600&display=swap');

/* ── NAV ───────────────────────────────────────── */
.nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  background: rgba(255,255,255,0);
  transition: background .3s, box-shadow .3s, border-color .3s;
  border-bottom: 1px solid transparent;
}
.nav--scrolled {
  background: rgba(255,255,255,0.97);
  box-shadow: 0 1px 0 var(--border);
  border-color: var(--border);
  backdrop-filter: blur(10px);
}
.nav__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  max-width: 480px;
  margin: 0 auto;
}
.nav__logo {
  font-family: var(--ff-head);
  font-size: 24px;
  font-weight: 700;
  letter-spacing: 1px;
  color: var(--ink);
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 6px;
}
.nav__logo-dot {
  width: 7px; height: 7px;
  background: var(--gold);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
}
.nav__cta {
  background: var(--ink);
  color: #fff;
  font-size: 13px;
  font-weight: 600;
  padding: 10px 20px;
  border-radius: 50px;
  text-decoration: none;
  transition: background .2s;
}
.nav__cta:hover { background: var(--gold); }

/* ── HERO ───────────────────────────────────────── */
.hero {
  min-height: 100svh;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 96px 24px 80px;
  max-width: 480px;
  margin: 0 auto;
  overflow: hidden;
}
.hero__bg {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 100% 70% at 60% -10%, rgba(184,154,90,0.12) 0%, transparent 60%),
    radial-gradient(ellipse 80% 50% at -10% 90%, rgba(184,154,90,0.07) 0%, transparent 60%);
  pointer-events: none;
}
.hero__badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: var(--gold-bg);
  border: 1px solid rgba(184,154,90,0.25);
  border-radius: 50px;
  padding: 6px 14px;
  font-size: 12px;
  color: var(--gold);
  font-weight: 500;
  width: fit-content;
  margin-bottom: 20px;
  animation: fadeUp .6s .1s ease both;
}
.hero__title {
  font-family: var(--ff-head);
  font-size: clamp(44px, 13vw, 68px);
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -1px;
  color: var(--ink);
  margin-bottom: 16px;
  animation: fadeUp .7s .2s ease both;
}
.hero__title span {
  font-style: italic;
  color: var(--gold);
}
.hero__sub {
  font-size: 16px;
  line-height: 1.7;
  color: var(--muted);
  margin-bottom: 32px;
  max-width: 320px;
  animation: fadeUp .7s .35s ease both;
}
.hero__btns {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-bottom: 40px;
  animation: fadeUp .7s .48s ease both;
}
.hero__stats {
  display: flex;
  gap: 0;
  border-top: 1px solid var(--border);
  padding-top: 24px;
  animation: fadeUp .7s .6s ease both;
}
.hero__stat {
  flex: 1;
  text-align: center;
  padding: 0 8px;
  border-right: 1px solid var(--border);
}
.hero__stat:last-child { border-right: none; }
.hero__stat strong {
  display: block;
  font-family: var(--ff-head);
  font-size: 26px;
  font-weight: 700;
  color: var(--ink);
  letter-spacing: -0.5px;
}
.hero__stat span {
  font-size: 11px;
  color: var(--muted);
  letter-spacing: .5px;
}
.hero__scroll-hint {
  position: absolute;
  bottom: 28px;
  left: 50%;
  transform: translateX(-50%);
  opacity: .4;
}
.hero__scroll-hint span {
  display: block;
  width: 1px;
  height: 36px;
  background: linear-gradient(to bottom, var(--ink), transparent);
  margin: 0 auto;
  animation: scrollDown 1.6s ease-in-out infinite;
}

/* ── BUTTONS ────────────────────────────────────── */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  font-family: var(--ff-body);
  font-size: 14px;
  font-weight: 600;
  padding: 14px 26px;
  border-radius: 50px;
  border: none;
  cursor: pointer;
  text-decoration: none;
  transition: all .2s;
  white-space: nowrap;
}
.btn:active { transform: scale(.96); }
.btn--primary {
  background: var(--ink);
  color: #fff;
}
.btn--primary:hover { background: var(--gold); }
.btn--ghost {
  background: transparent;
  border: 1.5px solid var(--border);
  color: var(--ink);
}
.btn--ghost:hover { border-color: var(--ink); }
.btn--outline {
  background: transparent;
  border: 1.5px solid var(--border);
  color: var(--ink);
  width: 100%;
}
.btn--outline:hover { border-color: var(--gold); color: var(--gold); }
.btn--full { width: 100%; }

/* ── SECTION BASE ───────────────────────────────── */
.section {
  padding: 60px 20px;
  max-width: 480px;
  margin: 0 auto;
}
.section--grey { background: var(--bg2); max-width: 100%; }
.section--grey > * { max-width: 480px; margin-left: auto; margin-right: auto; }
.section__head { margin-bottom: 28px; }
.section__eyebrow {
  font-size: 11px;
  letter-spacing: 2.5px;
  text-transform: uppercase;
  color: var(--gold);
  font-weight: 600;
  margin-bottom: 8px;
}
.section__title {
  font-family: var(--ff-head);
  font-size: clamp(28px, 8vw, 38px);
  font-weight: 700;
  letter-spacing: -0.5px;
  line-height: 1.1;
  color: var(--ink);
}
.section__title em {
  font-style: italic;
  color: var(--gold);
}
.section__sub {
  font-size: 14px;
  color: var(--muted);
  line-height: 1.7;
  margin-top: 8px;
}

/* ── SERVICES ───────────────────────────────────── */
.services__grid {
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.svc-card {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 18px 16px;
  border-radius: 14px;
  cursor: pointer;
  transition: background .2s, transform .15s;
  border: 1px solid transparent;
}
.svc-card:active { transform: scale(.98); }
.svc-card:hover {
  background: var(--gold-bg);
  border-color: rgba(184,154,90,0.2);
}
.svc-card__icon {
  width: 46px; height: 46px;
  background: var(--bg2);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  flex-shrink: 0;
  transition: background .2s;
}
.svc-card:hover .svc-card__icon { background: rgba(184,154,90,0.15); }
.svc-card__body { flex: 1; min-width: 0; }
.svc-card__name {
  font-size: 15px;
  font-weight: 600;
  color: var(--ink);
  margin-bottom: 2px;
}
.svc-card__desc { font-size: 12px; color: var(--muted); }
.svc-card__right {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 4px;
  flex-shrink: 0;
}
.svc-card__price {
  font-family: var(--ff-head);
  font-size: 16px;
  font-weight: 700;
  color: var(--ink);
}
.svc-card__arrow {
  font-size: 14px;
  color: var(--gold);
  transition: transform .2s;
}
.svc-card:hover .svc-card__arrow { transform: translateX(3px); }

/* ── WHY US ─────────────────────────────────────── */
.why {
  background: var(--dark);
  padding: 60px 0;
}
.why__inner {
  padding: 0 20px;
  max-width: 480px;
  margin: 0 auto;
}
.why__grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
  margin-top: 28px;
}
.why__item {
  background: var(--dark2);
  border: 1px solid rgba(255,255,255,0.06);
  border-radius: var(--radius);
  padding: 20px 16px;
  transition: border-color .25s, transform .2s;
}
.why__item:hover {
  border-color: rgba(184,154,90,0.25);
  transform: translateY(-2px);
}
.why__icon { font-size: 26px; margin-bottom: 10px; }
.why__title {
  font-size: 14px;
  font-weight: 600;
  color: #fff;
  margin-bottom: 6px;
  line-height: 1.3;
}
.why__desc { font-size: 12px; color: #666; line-height: 1.6; }

/* ── TEAM ───────────────────────────────────────── */
.team__rail {
  display: flex;
  gap: 12px;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
  margin: 0 -20px;
  padding: 4px 20px 12px;
}
.team__rail::-webkit-scrollbar { display: none; }
.team__card {
  flex-shrink: 0;
  width: 148px;
  scroll-snap-align: start;
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 18px 14px;
  text-align: center;
  transition: border-color .25s, transform .2s;
  cursor: pointer;
}
.team__card:hover { border-color: var(--gold); transform: translateY(-3px); }
.team__avatar {
  font-size: 44px;
  margin-bottom: 10px;
  line-height: 1;
}
.team__name { font-size: 14px; font-weight: 600; color: var(--ink); margin-bottom: 3px; }
.team__role { font-size: 11px; color: var(--muted); letter-spacing: .5px; margin-bottom: 8px; }
.team__avail {
  font-size: 11px;
  font-weight: 500;
  margin-bottom: 12px;
}
.team__avail--on { color: #27ae60; }
.team__avail--off { color: var(--muted); }
.team__book {
  width: 100%;
  background: var(--ink);
  color: #fff;
  border: none;
  border-radius: 50px;
  padding: 8px 0;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: background .2s;
}
.team__book:hover { background: var(--gold); }

/* ── REVIEWS ────────────────────────────────────── */
.reviews__rail {
  display: flex;
  gap: 12px;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
  margin: 0 -20px;
  padding: 4px 20px 12px;
}
.reviews__rail::-webkit-scrollbar { display: none; }
.rev-card {
  flex-shrink: 0;
  width: 270px;
  scroll-snap-align: start;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 22px 20px;
  transition: border-color .2s, transform .2s;
}
.rev-card:hover { border-color: var(--gold); transform: translateY(-2px); }
.rev-card__stars { color: #f5a623; font-size: 13px; letter-spacing: 2px; margin-bottom: 12px; }
.rev-card__text { font-size: 14px; line-height: 1.7; color: var(--ink); margin-bottom: 16px; }
.rev-card__footer { display: flex; align-items: center; gap: 10px; }
.rev-card__avatar { font-size: 28px; line-height: 1; }
.rev-card__name { font-size: 13px; font-weight: 600; color: var(--ink); }
.rev-card__date { font-size: 11px; color: var(--muted); }

/* ── BOOK FORM ──────────────────────────────────── */
.book-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.book-form__field label {
  display: block;
  font-size: 11px;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: var(--muted);
  font-weight: 600;
  margin-bottom: 8px;
}
.book-form__field input,
.book-form__field select {
  width: 100%;
  border: 1.5px solid var(--border);
  border-radius: 12px;
  padding: 14px 16px;
  font-family: var(--ff-body);
  font-size: 15px;
  color: var(--ink);
  background: var(--bg);
  outline: none;
  transition: border-color .25s;
  -webkit-appearance: none;
  appearance: none;
}
.book-form__field input:focus,
.book-form__field select:focus { border-color: var(--gold); }
.time-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
}
.time-btn {
  background: var(--bg2);
  border: 1.5px solid var(--border);
  border-radius: 10px;
  padding: 12px 6px;
  font-family: var(--ff-body);
  font-size: 13px;
  font-weight: 600;
  color: var(--ink);
  cursor: pointer;
  text-align: center;
  transition: all .2s;
}
.time-btn:hover { border-color: var(--gold); }
.time-btn--sel {
  background: var(--ink);
  border-color: var(--ink);
  color: #fff;
}

/* ── LOCATION ───────────────────────────────────── */
.location__card {
  border: 1px solid var(--border);
  border-radius: 20px;
  overflow: hidden;
}
.location__map-preview {
  height: 160px;
  background: linear-gradient(135deg, #e8e4dc, #d8d0c4);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  position: relative;
}
.location__map-preview::before {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    0deg, transparent, transparent 29px, rgba(0,0,0,.05) 29px, rgba(0,0,0,.05) 30px
  ),
  repeating-linear-gradient(
    90deg, transparent, transparent 29px, rgba(0,0,0,.05) 29px, rgba(0,0,0,.05) 30px
  );
}
.location__map-pin {
  font-size: 40px;
  position: relative;
  z-index: 1;
  animation: bounce 1.4s ease-in-out infinite;
}
.location__map-label {
  background: var(--ink);
  color: #fff;
  font-size: 12px;
  font-weight: 600;
  padding: 5px 14px;
  border-radius: 20px;
  position: relative;
  z-index: 1;
}
.location__info {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.location__row { display: flex; gap: 14px; align-items: flex-start; }
.location__icon { font-size: 20px; flex-shrink: 0; margin-top: 1px; }
.location__lbl {
  font-size: 11px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--muted);
  margin-bottom: 3px;
  font-weight: 600;
}
.location__val { font-size: 14px; color: var(--ink); line-height: 1.6; }
.location__card .btn--outline { border-radius: 0; border: none; border-top: 1px solid var(--border); }

/* ── CONTACT ────────────────────────────────────── */
.contact__list { display: flex; flex-direction: column; gap: 10px; }
.contact__item {
  display: flex;
  align-items: center;
  gap: 14px;
  background: var(--bg2);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 16px 18px;
  text-decoration: none;
  transition: border-color .2s, transform .15s;
}
.contact__item:active { transform: scale(.98); }
.contact__item:hover { border-color: var(--gold); }
.contact__item--wa {
  background: rgba(37,211,102,0.06);
  border-color: rgba(37,211,102,0.2);
}
.contact__item--wa:hover { border-color: var(--wa); }
.contact__item-icon {
  width: 44px; height: 44px;
  background: var(--bg);
  border-radius: 12px;
  border: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  flex-shrink: 0;
}
.contact__item-text { flex: 1; }
.contact__item-lbl {
  font-size: 11px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--muted);
  font-weight: 600;
  margin-bottom: 2px;
}
.contact__item-val { font-size: 14px; color: var(--ink); font-weight: 500; }
.contact__item-arrow { font-size: 16px; color: var(--gold); }

/* ── FOOTER ─────────────────────────────────────── */
.footer {
  background: var(--dark);
  padding: 48px 20px 32px;
  text-align: center;
}
.footer__logo {
  font-family: var(--ff-head);
  font-size: 30px;
  font-weight: 700;
  color: #fff;
  letter-spacing: 1px;
  margin-bottom: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
}
.footer__logo-dot {
  width: 7px; height: 7px;
  background: var(--gold);
  border-radius: 50%;
}
.footer__tagline { font-size: 12px; color: #555; letter-spacing: 1px; margin-bottom: 24px; }
.footer__links {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 24px;
}
.footer__links a {
  font-size: 13px;
  color: #666;
  text-decoration: none;
  transition: color .2s;
}
.footer__links a:hover { color: var(--gold); }
.footer__copy { font-size: 12px; color: #444; }

/* ── BOTTOM BAR ─────────────────────────────────── */
.bottom-bar {
  position: fixed;
  bottom: 0; left: 0; right: 0;
  z-index: 99;
  display: flex;
  align-items: center;
  gap: 0;
  background: #fff;
  border-top: 1px solid var(--border);
  padding: 10px 12px;
  padding-bottom: calc(10px + env(safe-area-inset-bottom));
}
.bottom-bar__btn {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  font-family: var(--ff-body);
  font-size: 13px;
  font-weight: 700;
  padding: 12px 6px;
  border-radius: 12px;
  text-decoration: none;
  border: none;
  cursor: pointer;
  transition: all .2s;
  margin: 0 4px;
}
.bottom-bar__btn:active { transform: scale(.95); }
.bottom-bar__btn--call {
  background: var(--bg2);
  color: var(--ink);
  border: 1px solid var(--border);
}
.bottom-bar__btn--whatsapp {
  background: var(--wa);
  color: #fff;
}
.bottom-bar__btn--book {
  background: var(--ink);
  color: #fff;
}
.bottom-bar__btn--book:hover { background: var(--gold); }

/* ── TOAST ──────────────────────────────────────── */
.toast {
  position: fixed;
  bottom: 88px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--ink);
  color: #fff;
  padding: 13px 22px;
  border-radius: 50px;
  font-size: 14px;
  font-weight: 500;
  z-index: 200;
  white-space: nowrap;
  border-left: 3px solid var(--gold);
  box-shadow: 0 4px 20px rgba(0,0,0,.15);
}
.toast-fade-enter-active, .toast-fade-leave-active { transition: all .3s ease; }
.toast-fade-enter-from, .toast-fade-leave-to { opacity: 0; transform: translateX(-50%) translateY(12px); }

/* ── ANIMATIONS ─────────────────────────────────── */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50%       { opacity: .5; transform: scale(.85); }
}
@keyframes scrollDown {
  0%, 100% { opacity: .3; transform: scaleY(1); }
  50%       { opacity: 1; transform: scaleY(1.2); }
}
@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(-8px); }
}
</style>