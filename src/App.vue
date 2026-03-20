<template>
  <div id="app">

    <!-- ── STICKY NAV ── -->
    <header :class="['nav', { 'nav--scrolled': scrolled }]">
      <div class="nav__inner">
        <a href="#home" class="nav__logo" @click.prevent="scrollTo('home')">
          <span class="nav__logo-dot"></span>Aura
        </a>
        <!-- <nav class="nav__links">
          <a @click.prevent="scrollTo('services')" href="#">Services</a>
          <a @click.prevent="scrollTo('team')" href="#">Team</a>
          <a @click.prevent="scrollTo('book')" href="#">Book</a>
        </nav> -->
        <button class="nav__cta" @click="scrollTo('book')"><span>✦</span> Book Now</button>
      </div>
    </header>

    <!-- ══════════════════════════════════════════
         HERO CAROUSEL — full bleed, right below nav
    ══════════════════════════════════════════ -->
    <section id="home" class="hero-carousel">
      <div class="hc__track" :style="{ transform: `translateX(-${hcActive * 100}%)` }">
        <div class="hc__slide" v-for="(slide, i) in heroSlides" :key="i">
          <img :src="slide.img" :alt="slide.title" class="hc__img" />
          <div class="hc__gradient"></div>
          <div class="hc__content">
            <span class="hc__eyebrow">{{ slide.eyebrow }}</span>
            <h1 class="hc__title" v-html="slide.title"></h1>
            <p class="hc__sub">{{ slide.sub }}</p>
            <!-- <div class="hc__btns">
              <a :href="whatsappBook('I want to book an appointment')" target="_blank" class="btn btn--white">Book Now →</a>
              <button class="btn btn--ghost-white" @click="scrollTo('services')">View Services</button>
            </div> -->
          </div>
        </div>
      </div>

      <button class="hc__arrow hc__arrow--l" @click="hcPrev">&#8592;</button>
      <button class="hc__arrow hc__arrow--r" @click="hcNext">&#8594;</button>

      <div class="hc__dots">
        <button v-for="(s, i) in heroSlides" :key="i" :class="['hc__dot', { 'hc__dot--on': hcActive === i }]" @click="hcGo(i)"></button>
      </div>

      <div class="hc__stats">
        <div class="hc__stat" v-for="s in stats" :key="s.label">
          <strong>{{ s.value }}</strong>
          <span>{{ s.label }}</span>
        </div>
      </div>
    </section>

    <!-- ══════════════════════════════════════════
         VISUAL MOSAIC GRID
    ══════════════════════════════════════════ -->
    <section class="visual-grid">
      <div class="vg__head">
        <p class="eyebrow">Inside Aura</p>
        <h2 class="vg__title">Our <em>Space & Craft</em></h2>
      </div>
      <div class="vg__mosaic">
        <div class="vg__cell vg__cell--tall" @click="openLightbox(0)">
          <img src="https://images.unsplash.com/photo-1560066984-138dadb4c035?w=600&h=900&q=85&fit=crop" alt="Salon interior" />
          <div class="vg__cell-overlay"><span>🏠 Our Space</span></div>
        </div>
        <div class="vg__col">
          <div class="vg__cell" @click="openLightbox(1)">
            <img src="https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?w=600&h=400&q=85&fit=crop" alt="Hair styling" />
            <div class="vg__cell-overlay"><span>✂️ Hair Styling</span></div>
          </div>
          <div class="vg__cell" @click="openLightbox(2)">
            <img src="https://images.unsplash.com/photo-1487412947147-5cebf100ffc2?w=600&h=400&q=85&fit=crop&crop=faces" alt="Colour" />
            <div class="vg__cell-overlay"><span>🎨 Colour</span></div>
          </div>
        </div>
        <div class="vg__col">
          <div class="vg__cell" @click="openLightbox(3)">
            <img src="https://images.unsplash.com/photo-1570172619644-dfd03ed5d881?w=600&h=400&q=85&fit=crop" alt="Facial" />
            <div class="vg__cell-overlay"><span>✨ Skin Care</span></div>
          </div>
          <div class="vg__cell" @click="openLightbox(4)">
            <img src="https://images.unsplash.com/photo-1604654894610-df63bc536371?w=600&h=400&q=85&fit=crop" alt="Nails" />
            <div class="vg__cell-overlay"><span>💅 Nails</span></div>
          </div>
        </div>
      </div>
    </section>

    <!-- ══════════════════════════════════════════
         SPECIAL GUESTS & OFFERS
    ══════════════════════════════════════════ -->
    <section class="offers">
      <div class="offers__inner">
        <p class="eyebrow" style="color:#b89a5a">Exclusive at Aura</p>
        <h2 class="offers__title">Special <em>Guests & Offers</em></h2>
        <p class="offers__sub">Pamper yourself with limited-time packages and celebrity-inspired looks.</p>

        <div class="offers__rail">
          <div class="offer-card offer-card--feature">
            <div class="offer-card__img-wrap">
              <img src="https://images.unsplash.com/photo-1580618672591-eb180b1a973f?w=500&h=400&q=85&fit=crop&crop=faces" alt="Guest Expert" />
              <div class="offer-card__badge">⭐ Guest Expert</div>
            </div>
            <div class="offer-card__body">
              <p class="offer-card__name">Zara Khan</p>
              <p class="offer-card__role">Celebrity Hair Artist · Mumbai</p>
              <p class="offer-card__desc">Signature balayage & bridal styling. Limited slots every month.</p>
              <a :href="whatsappBook('Hi! I want to book a session with Guest Expert Zara Khan at Aura Salon.')" target="_blank" class="btn btn--gold btn--sm">Book Session →</a>
            </div>
          </div>

          <div class="offer-card">
            <div class="offer-card__img-wrap">
              <img src="https://images.unsplash.com/photo-1519415943484-9fa1873496d4?w=500&h=320&q=85&fit=crop" alt="Bridal Package" />
              <div class="offer-card__badge offer-card__badge--red">🔥 Hot Deal</div>
            </div>
            <div class="offer-card__body">
              <p class="offer-card__name">Bridal Glow Package</p>
              <p class="offer-card__role">Makeup · Hair · Skin · Nails</p>
              <div class="offer-card__price"><span class="offer-card__old">₹9,999</span><span class="offer-card__new">₹6,499</span></div>
              <a :href="whatsappBook('Hi! I want to book the Bridal Glow Package at Aura Salon.')" target="_blank" class="btn btn--gold btn--sm">Grab Offer →</a>
            </div>
          </div>

          <div class="offer-card">
            <div class="offer-card__img-wrap">
              <img src="https://images.unsplash.com/photo-1512290923902-8a9f81dc236c?w=500&h=320&q=85&fit=crop" alt="Keratin" />
              <div class="offer-card__badge offer-card__badge--green">✅ New</div>
            </div>
            <div class="offer-card__body">
              <p class="offer-card__name">Keratin Smoothing</p>
              <p class="offer-card__role">Brazilian · Nano · Protein</p>
              <div class="offer-card__price"><span class="offer-card__old">₹3,999</span><span class="offer-card__new">₹2,499</span></div>
              <a :href="whatsappBook('Hi! I want to book the Keratin Smoothing offer at Aura Salon.')" target="_blank" class="btn btn--gold btn--sm">Book Now →</a>
            </div>
          </div>

          <div class="offer-card">
            <div class="offer-card__img-wrap">
              <img src="https://images.unsplash.com/photo-1600334089648-b0d9d3028eb2?w=500&h=320&q=85&fit=crop" alt="Spa Day" />
              <div class="offer-card__badge">💆 Trending</div>
            </div>
            <div class="offer-card__body">
              <p class="offer-card__name">Full Spa Day</p>
              <p class="offer-card__role">Facial · Massage · Hair Spa</p>
              <div class="offer-card__price"><span class="offer-card__old">₹4,499</span><span class="offer-card__new">₹2,999</span></div>
              <a :href="whatsappBook('Hi! I want to book the Full Spa Day package at Aura Salon.')" target="_blank" class="btn btn--gold btn--sm">Book Now →</a>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ── SERVICES ── -->
    <section id="services" class="section">
      <div class="section__head">
        <p class="eyebrow">What We Offer</p>
        <h2 class="section__title">Our <em>Services</em></h2>
      </div>
      <div class="services__grid">
        <div v-for="svc in services" :key="svc.name" class="svc-card">
          <div class="svc-card__icon">{{ svc.icon }}</div>
          <div class="svc-card__body">
            <p class="svc-card__name">{{ svc.name }}</p>
            <p class="svc-card__desc">{{ svc.desc }}</p>
          </div>
          <div class="svc-card__right">
            <span class="svc-card__price">{{ svc.price }}</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ── WHY US ── -->
    <section class="why">
      <div class="why__inner">
        <p class="eyebrow" style="color:#b89a5a">Why Choose Us</p>
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
        <p class="eyebrow">The Artists</p>
        <h2 class="section__title">Meet the <em>Team</em></h2>
      </div>
      <div class="team__rail">
        <div class="team__card" v-for="m in team" :key="m.name" @click="bookWithStylist(m.name)">
          <div class="team__avatar">{{ m.avatar }}</div>
          <p class="team__name">{{ m.name }}</p>
          <p class="team__role">{{ m.role }}</p>
          <div :class="['team__avail', m.available ? 'team__avail--on' : 'team__avail--off']">{{ m.available ? '● Available' : '● Busy today' }}</div>
          <button class="team__book">Book →</button>
        </div>
      </div>
    </section>

    <!-- ── REVIEWS ── -->
    <section id="reviews" class="section section--grey">
      <div class="section__head">
        <p class="eyebrow">Real Reviews</p>
        <h2 class="section__title">Client <em>Love</em></h2>
      </div>
      <div class="reviews__rail">
        <div class="rev-card" v-for="r in reviews" :key="r.name">
          <div class="rev-card__stars">★★★★★</div>
          <p class="rev-card__text">"{{ r.text }}"</p>
          <div class="rev-card__footer">
            <span class="rev-card__avatar">{{ r.avatar }}</span>
            <div><p class="rev-card__name">{{ r.name }}</p><p class="rev-card__date">{{ r.date }}</p></div>
          </div>
        </div>
      </div>
    </section>

    <!-- ── BOOKING FORM ── -->
    <section id="book" class="section">
      <div class="section__head">
        <p class="eyebrow">Reserve a Slot</p>
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
            <button type="button" v-for="t in timeSlots" :key="t" :class="['time-btn', { 'time-btn--sel': form.time === t }]" @click="form.time = t">{{ t }}</button>
          </div>
        </div>
        <button type="submit" class="btn btn--primary btn--full">Send via WhatsApp →</button>
      </form>
    </section>

    <!-- ── UPI PAYMENT ── -->
    <section id="pay" class="section section--grey">
      <div class="section__head">
        <p class="eyebrow">Easy Payments</p>
        <h2 class="section__title">Pay <em>Instantly</em></h2>
        <p class="section__sub">Scan & pay with any UPI app — GPay, PhonePe, Paytm, or your bank app.</p>
      </div>
      <div class="pay-card">
        <div class="pay-card__qr-wrap">
          <div class="pay-card__qr-frame">
            <img :src="qrImageUrl" alt="UPI QR Code" class="pay-card__qr-img" />
            <div class="pay-card__qr-corner pay-card__qr-corner--tl"></div>
            <div class="pay-card__qr-corner pay-card__qr-corner--tr"></div>
            <div class="pay-card__qr-corner pay-card__qr-corner--bl"></div>
            <div class="pay-card__qr-corner pay-card__qr-corner--br"></div>
          </div>
          <div class="pay-card__upi-id">
            <span class="pay-card__upi-label">UPI ID</span>
            <span class="pay-card__upi-val">{{ UPI_ID }}</span>
            <button class="pay-card__copy" @click="copyUpi">{{ copied ? '✅' : '📋' }}</button>
          </div>
        </div>
        <div class="pay-card__apps">
          <p class="pay-card__apps-label">Accepted on</p>
          <div class="pay-card__apps-row">
            <div class="upi-app" v-for="app in upiApps" :key="app.name">
              <img :src="app.url" alt="PhonePe logo" class="upi-app__icon">
              <span>{{ app.name }}</span>
            </div>
          </div>
        </div>
        <div class="pay-card__note"><span>💡</span><p>Share your payment screenshot on WhatsApp for quick confirmation.</p></div>
        <a :href="whatsappBook('Hi! I just made a payment at Aura Salon. Please find my screenshot attached.')" target="_blank" class="btn btn--primary btn--full pay-card__btn">Share Screenshot on WhatsApp →</a>
      </div>
    </section>

    <!-- ── LOCATION ── -->
    <section id="location" class="section">
      <div class="section__head"><p class="eyebrow">Find Us</p><h2 class="section__title">Our <em>Location</em></h2></div>
      <div class="location__card">
        <div class="location__map-preview">
          <div class="location__map-pin">📍</div>
          <div class="location__map-label">Aura Salon, Pune</div>
        </div>
        <div class="location__info">
          <div class="location__row"><span class="location__icon">📍</span><div><p class="location__lbl">Address</p><p class="location__val">12, FC Road, Shivajinagar,<br/>Pune, Maharashtra 411005</p></div></div>
          <div class="location__row"><span class="location__icon">🕐</span><div><p class="location__lbl">Hours</p><p class="location__val">Mon–Sat: 10:00 AM – 8:00 PM</p><p class="location__val" style="color:#e05252">Sunday: Closed</p></div></div>
          <div class="location__row"><span class="location__icon">🚇</span><div><p class="location__lbl">Nearest Landmark</p><p class="location__val">Near Pune University Circle</p></div></div>
        </div>
        <a href="https://www.google.com/maps/place/Mandarshree,+Baner+Rd,+Laxman+Nagar,+Baner,+Pune,+Maharashtra+411045/@18.5674352,73.7675398,17z" target="_blank" class="btn btn--outline btn--full">Open in Google Maps →</a>
      </div>
    </section>

    <!-- ── CONTACT ── -->
    <section id="contact" class="section">
      <div class="section__head"><p class="eyebrow">Contact Us</p><h2 class="section__title">Get in <em>Touch</em></h2></div>
      <div class="contact__list">
        <a :href="whatsappBook('Hello! I have an enquiry about your salon services.')" target="_blank" class="contact__item contact__item--wa">
          <div class="contact__item-icon">💬</div><div class="contact__item-text"><p class="contact__item-lbl">WhatsApp Us</p><p class="contact__item-val">Tap to chat instantly</p></div><span class="contact__item-arrow">→</span>
        </a>
        <a href="tel:+919876543210" class="contact__item">
          <div class="contact__item-icon">📞</div><div class="contact__item-text"><p class="contact__item-lbl">Call Us</p><p class="contact__item-val">+91 98765 43210</p></div><span class="contact__item-arrow">→</span>
        </a>
        <a href="https://instagram.com/aura.salon.pune" target="_blank" class="contact__item">
          <div class="contact__item-icon">📸</div><div class="contact__item-text"><p class="contact__item-lbl">Instagram</p><p class="contact__item-val">@aura.salon.pune</p></div><span class="contact__item-arrow">→</span>
        </a>
        <a href="mailto:hello@aurasalon.in" class="contact__item">
          <div class="contact__item-icon">✉️</div><div class="contact__item-text"><p class="contact__item-lbl">Email</p><p class="contact__item-val">hello@aurasalon.in</p></div><span class="contact__item-arrow">→</span>
        </a>
      </div>
    </section>

    <!-- ── FOOTER ── -->
    <footer class="footer">
      <div class="footer__logo"><span class="footer__logo-dot"></span>Aura</div>
      <p class="footer__tagline">Premium Salon & Spa · Pune</p>
      <div class="footer__links">
        <a @click.prevent="scrollTo('services')" href="#">Services</a>
        <a @click.prevent="scrollTo('team')" href="#">Team</a>
        <a @click.prevent="scrollTo('pay')" href="#">Pay</a>
        <a @click.prevent="scrollTo('book')" href="#">Book</a>
        <a @click.prevent="scrollTo('contact')" href="#">Contact</a>
      </div>
      <p class="footer__copy">© 2025 Aura Salon. All rights reserved.</p>
    </footer>

    <!-- ── TOAST ── -->
    <transition name="toast-fade">
      <div v-if="toast.show" class="toast">{{ toast.msg }}</div>
    </transition>

    <!-- ── LIGHTBOX ── -->
    <transition name="lb-fade">
      <div v-if="lightbox >= 0" class="lightbox" @click.self="lightbox = -1">
        <button class="lb__close" @click="lightbox = -1">✕</button>
        <img :src="galleryFull[lightbox]" class="lb__img" />
      </div>
    </transition>

  </div>
</template>

<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue'

const WHATSAPP_NUMBER = '+919996060691'
const SALON_NAME = 'Aura Salon, Pune'
const UPI_ID = '8484808337@ybl'

const scrolled = ref(false)
const toast = reactive({ show: false, msg: '' })
const copied = ref(false)
const lightbox = ref(-1)
const hcActive = ref(0)
let hcTimer = null

const form = reactive({ name: '', phone: '', service: '', date: '', time: '' })
const today = computed(() => new Date().toISOString().split('T')[0])

const qrImageUrl = computed(() => {
  const d = `upi://pay?pa=${UPI_ID}&pn=Aura+Salon+Pune&cu=INR`
  return `https://api.qrserver.com/v1/create-qr-code/?size=220x220&data=${encodeURIComponent(d)}&bgcolor=ffffff&color=111111&margin=10&qzone=1`
})

const heroSlides = [
  { img: 'https://images.unsplash.com/photo-1560066984-138dadb4c035?w=1000&h=620&q=90&fit=crop&crop=center', eyebrow: '📍 Pune\'s #1 Salon', title: 'Your Best Look,<br/><em>Every Visit.</em>', sub: 'Premium salon & spa crafted for you.' },
  { img: 'https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?w=1000&h=620&q=90&fit=crop&crop=center', eyebrow: '✂️ Expert Stylists', title: 'Hair That Turns<br/><em>Every Head.</em>', sub: 'Cuts, colour & keratin by award-winning artists.' },
  { img: 'https://images.unsplash.com/photo-1487412947147-5cebf100ffc2?w=1000&h=620&q=90&fit=crop&crop=faces', eyebrow: '🎨 Colour Experts', title: 'Bold Colour.<br/><em>Flawless Finish.</em>', sub: 'Balayage, ombré & global colour done right.' },
  { img: 'https://images.unsplash.com/photo-1570172619644-dfd03ed5d881?w=1000&h=620&q=90&fit=crop&crop=center', eyebrow: '✨ Skin & Wellness', title: 'Glow From<br/><em>Within.</em>', sub: 'Facials, spa & skin treatments for radiant skin.' },
  { img: 'https://images.unsplash.com/photo-1519415943484-9fa1873496d4?w=1000&h=620&q=90&fit=crop&crop=center', eyebrow: '💄 Bridal Specialists', title: 'Your Perfect<br/><em>Bridal Look.</em>', sub: 'HD, airbrush & traditional bridal packages.' },
]

const galleryFull = [
  'https://images.unsplash.com/photo-1560066984-138dadb4c035?w=1200&q=90&fit=crop',
  'https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?w=1200&q=90&fit=crop',
  'https://images.unsplash.com/photo-1487412947147-5cebf100ffc2?w=1200&q=90&fit=crop',
  'https://images.unsplash.com/photo-1570172619644-dfd03ed5d881?w=1200&q=90&fit=crop',
  'https://images.unsplash.com/photo-1604654894610-df63bc536371?w=1200&q=90&fit=crop',
]

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
  { icon: '🏆', title: 'Award-Winning Team', desc: 'Trained at top institutes with 10+ years each.' },
  { icon: '🌿', title: 'Premium Products', desc: 'Internationally certified, skin-safe products only.' },
  { icon: '⏱', title: 'On-Time Service', desc: 'We respect your time. Always on schedule.' },
  { icon: '✅', title: '100% Hygienic', desc: 'Fresh tools, sanitized stations every single time.' },
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
  { avatar: '🙋', name: 'Meera D.', date: '1 month ago', text: 'Got my balayage done and it\'s stunning. The colour lasted 4 months. Highly recommend Aisha!' },
  { avatar: '👰', name: 'Pooja M.', date: '3 weeks ago', text: 'My bridal makeup was absolutely perfect. I received so many compliments. Worth every rupee!' },
  { avatar: '😍', name: 'Ananya K.', date: '1 week ago', text: 'Fast, hygienic, and they truly listen to what you want. This is my forever salon in Pune.' },
]

const timeSlots = ['10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00', '17:00', '18:00']

const upiApps = [
  { name: 'GPay', icon: 'G', color: '#4285F4', url: 'https://uxwing.com/wp-content/themes/uxwing/download/brands-and-social-media/google-pay-icon.png'},
  { name: 'PhonePe', icon: '₱', color: '#5F259F', url: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTo4x8kSTmPUq4PFzl4HNT0gObFuEhivHOFYg&s'},
  
]

const paySteps = [
  'Open any UPI app on your phone',
  'Tap "Scan QR" and scan the code above',
  'Enter amount & complete payment',
  'Share screenshot with us on WhatsApp',
]

function whatsappBook(msg) { return `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(msg)}` }
function bookService(name) { window.open(whatsappBook(`Hi! I'd like to book *${name}* at ${SALON_NAME}. Please share available slots.`), '_blank') }
function bookWithStylist(name) { window.open(whatsappBook(`Hi! I'd like to book with *${name}* at ${SALON_NAME}. Please share available slots.`), '_blank') }
function submitBooking() {
  if (!form.time) { showToast('⚠️ Please select a time slot'); return }
  const msg = `*New Appointment Request* 🗓\n\n👤 Name: ${form.name}\n📱 Phone: ${form.phone}\n✂️ Service: ${form.service}\n📅 Date: ${form.date}\n⏰ Time: ${form.time}\n\nSent from ${SALON_NAME} website.`
  window.open(whatsappBook(msg), '_blank')
  showToast('✅ Redirecting to WhatsApp…')
  Object.assign(form, { name: '', phone: '', service: '', date: today.value, time: '' })
}
async function copyUpi() {
  try { await navigator.clipboard.writeText(UPI_ID); copied.value = true; showToast('✅ UPI ID copied!'); setTimeout(() => { copied.value = false }, 2000) }
  catch { showToast('📋 UPI ID: ' + UPI_ID) }
}
function openLightbox(i) { lightbox.value = i }
function scrollTo(id) { document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' }) }
function showToast(msg, d = 3000) { toast.msg = msg; toast.show = true; setTimeout(() => { toast.show = false }, d) }
function handleScroll() { scrolled.value = window.scrollY > 60 }
function hcNext() { hcActive.value = (hcActive.value + 1) % heroSlides.length; resetTimer() }
function hcPrev() { hcActive.value = (hcActive.value - 1 + heroSlides.length) % heroSlides.length; resetTimer() }
function hcGo(i) { hcActive.value = i; resetTimer() }
function resetTimer() { clearInterval(hcTimer); hcTimer = setInterval(() => { hcActive.value = (hcActive.value + 1) % heroSlides.length }, 4500) }

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
  form.date = today.value
  resetTimer()
})
onUnmounted(() => { window.removeEventListener('scroll', handleScroll); clearInterval(hcTimer) })
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600;1,700&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600&display=swap');
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

:root {
  --bg:#ffffff; --bg2:#f7f6f4; --ink:#111111; --muted:#777777;
  --border:#e4e4e0; --gold:#b89a5a; --gold-lt:#d4b97a; --gold-bg:rgba(184,154,90,0.09);
  --dark:#16140f; --dark2:#1e1b13; --wa:#25d366; --radius:16px;
  --ff-h:'Cormorant Garamond',Georgia,serif; --ff-b:'DM Sans',system-ui,sans-serif;
}
html{scroll-behavior:smooth}
body{background:var(--bg);color:var(--ink);font-family:var(--ff-b);-webkit-font-smoothing:antialiased;overflow-x:hidden;padding-bottom:72px}

/* ── NAV ── */
.nav{position:fixed;top:0;left:0;right:0;z-index:200;transition:background .3s,backdrop-filter .3s,box-shadow .3s}
.nav--scrolled{background:rgba(255,255,255,0.96);backdrop-filter:blur(12px);box-shadow:0 1px 0 var(--border)}
.nav__inner{display:flex;align-items:center;justify-content:space-between;padding:14px 20px;max-width:520px;margin:0 auto;gap:12px}
.nav__logo{font-family:var(--ff-h);font-size:26px;font-weight:700;letter-spacing:1px;text-decoration:none;display:flex;align-items:center;gap:6px;flex-shrink:0;color:var(--ink);transition:color .3s}
.nav:not(.nav--scrolled) .nav__logo{color:#fff}
.nav:not(.nav--scrolled) .nav__links a{color:rgba(255,255,255,0.85)}
.nav__logo-dot{width:7px;height:7px;background:var(--gold);border-radius:50%;animation:pulse 2s ease-in-out infinite}
.nav__links{display:flex;gap:20px;flex:1;justify-content:center}
.nav__links a{font-size:13px;font-weight:500;color:var(--ink);text-decoration:none;transition:color .2s}
.nav__links a:hover{color:var(--gold)}
.nav__cta{background:var(--gold);color:#fff;font-size:13px;font-weight:600;padding:10px 20px;border-radius:50px;text-decoration:none;transition:background .2s;flex-shrink:0}

/* ── HERO CAROUSEL ── */
.hero-carousel{position:relative;overflow:hidden}
.hc__track{display:flex;transition:transform 0.55s cubic-bezier(0.25,0.46,0.45,0.94);will-change:transform}
.hc__slide{flex:0 0 100%;min-width:100%;position:relative;height:560px}
.hc__img{width:100%;height:100%;object-fit:cover;object-position:center;display:block;pointer-events:none}
.hc__gradient{position:absolute;inset:0;background:linear-gradient(to bottom,rgba(0,0,0,0.3) 0%,rgba(0,0,0,0.08) 40%,rgba(0,0,0,0.65) 100%)}
.hc__content{position:absolute;bottom:96px;left:0;right:0;padding:0 24px;max-width:520px;margin:0 auto}
.hc__eyebrow{display:inline-block;background:rgba(184,154,90,0.88);color:#fff;font-size:11px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;padding:5px 14px;border-radius:50px;margin-bottom:12px}
.hc__title{font-family:var(--ff-h);font-size:clamp(36px,11vw,60px);font-weight:700;line-height:1.05;letter-spacing:-1px;color:#fff;margin-bottom:10px}
.hc__title em{font-style:italic;color:var(--gold-lt)}
.hc__sub{font-size:15px;color:rgba(255,255,255,0.82);line-height:1.6;margin-bottom:22px;max-width:300px}
.hc__btns{display:flex;gap:10px;flex-wrap:wrap}
.hc__arrow{position:absolute;top:45%;transform:translateY(-50%);width:44px;height:44px;border-radius:50%;background:rgba(255,255,255,0.15);border:1.5px solid rgba(255,255,255,0.3);color:#fff;font-size:20px;display:flex;align-items:center;justify-content:center;cursor:pointer;z-index:10;transition:background .2s,transform .2s;backdrop-filter:blur(4px)}
.hc__arrow:hover{background:var(--gold);border-color:var(--gold);transform:translateY(-50%) scale(1.1)}
.hc__arrow--l{left:14px}
.hc__arrow--r{right:14px}
.hc__dots{position:absolute;bottom:56px;left:50%;transform:translateX(-50%);display:flex;gap:6px;z-index:10}
.hc__dot{width:7px;height:7px;border-radius:50%;border:none;background:rgba(255,255,255,0.35);cursor:pointer;transition:all .3s;padding:0}
.hc__dot--on{background:var(--gold);width:24px;border-radius:4px}
.hc__stats{position:absolute;bottom:0;left:0;right:0;display:flex;background:rgba(22,20,15,0.85);backdrop-filter:blur(8px)}
.hc__stat{flex:1;text-align:center;padding:12px 8px;border-right:1px solid rgba(255,255,255,0.08)}
.hc__stat:last-child{border-right:none}
.hc__stat strong{display:block;font-family:var(--ff-h);font-size:22px;font-weight:700;color:var(--gold-lt);letter-spacing:-0.5px}
.hc__stat span{font-size:10px;color:rgba(255,255,255,0.5);letter-spacing:.5px}

/* ── BUTTONS ── */
.btn{display:inline-flex;align-items:center;justify-content:center;gap:6px;font-family:var(--ff-b);font-size:14px;font-weight:600;padding:13px 24px;border-radius:50px;border:none;cursor:pointer;text-decoration:none;transition:all .2s;white-space:nowrap}
.btn:active{transform:scale(.96)}
.btn--primary{background:var(--ink);color:#fff}
.btn--primary:hover{background:var(--gold)}
.btn--white{background:#fff;color:var(--ink)}
.btn--white:hover{background:var(--gold);color:#fff}
.btn--ghost-white{background:transparent;border:1.5px solid rgba(255,255,255,0.5);color:#fff}
.btn--ghost-white:hover{border-color:#fff;background:rgba(255,255,255,0.12)}
.btn--gold{background:var(--gold);color:#fff}
.btn--gold:hover{background:var(--dark)}
.btn--outline{background:transparent;border:1.5px solid var(--border);color:var(--ink);width:100%}
.btn--outline:hover{border-color:var(--gold);color:var(--gold)}
.btn--full{width:100%}
.btn--sm{font-size:12px;padding:9px 18px}

/* ── VISUAL GRID ── */
.visual-grid{padding:56px 20px 0;max-width:520px;margin:0 auto}
.vg__head{margin-bottom:20px}
.vg__title{font-family:var(--ff-h);font-size:clamp(26px,7vw,36px);font-weight:700;letter-spacing:-0.5px;line-height:1.1;color:var(--ink)}
.vg__title em{font-style:italic;color:var(--gold)}
.vg__mosaic{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px}
.vg__cell--tall{grid-row:span 2}
.vg__col{display:flex;flex-direction:column;gap:8px}
.vg__cell{position:relative;overflow:hidden;border-radius:14px;cursor:pointer;background:var(--bg2)}
.vg__cell img{width:100%;height:100%;object-fit:cover;display:block;transition:transform .5s ease}
.vg__cell:hover img{transform:scale(1.06)}
.vg__cell--tall{height:300px}
.vg__col .vg__cell{height:143px}
.vg__cell-overlay{position:absolute;inset:0;background:linear-gradient(to top,rgba(0,0,0,0.6) 0%,transparent 55%);display:flex;align-items:flex-end;padding:10px;opacity:0;transition:opacity .3s}
.vg__cell:hover .vg__cell-overlay{opacity:1}
.vg__cell-overlay span{background:rgba(184,154,90,0.9);color:#fff;font-size:10px;font-weight:700;letter-spacing:1px;text-transform:uppercase;padding:4px 10px;border-radius:50px}

/* ── OFFERS / SPECIAL GUESTS ── */
.offers{padding:60px 0 0;background:var(--dark);margin-top:56px}
.offers__inner{padding:0 20px 60px;max-width:520px;margin:0 auto}
.offers__title{font-family:var(--ff-h);font-size:clamp(28px,8vw,40px);font-weight:700;letter-spacing:-0.5px;line-height:1.1;color:#fff;margin:6px 0 8px}
.offers__title em{font-style:italic;color:var(--gold-lt)}
.offers__sub{font-size:14px;color:#666;line-height:1.6;margin-bottom:28px}
.offers__rail{display:flex;gap:14px;overflow-x:auto;scroll-snap-type:x mandatory;scrollbar-width:none;margin:0 -20px;padding:0 20px 4px}
.offers__rail::-webkit-scrollbar{display:none}
.offer-card{flex-shrink:0;width:220px;scroll-snap-align:start;background:var(--dark2);border:1px solid rgba(255,255,255,0.07);border-radius:20px;overflow:hidden;transition:border-color .25s,transform .2s}
.offer-card:hover{border-color:rgba(184,154,90,0.3);transform:translateY(-4px)}
.offer-card--feature{width:260px}
.offer-card__img-wrap{position:relative;overflow:hidden}
.offer-card__img-wrap img{width:100%;height:180px;object-fit:cover;display:block;transition:transform .5s}
.offer-card:hover .offer-card__img-wrap img{transform:scale(1.05)}
.offer-card--feature .offer-card__img-wrap img{height:200px}
.offer-card__badge{position:absolute;top:10px;left:10px;background:rgba(22,20,15,0.85);color:var(--gold-lt);font-size:10px;font-weight:700;letter-spacing:1px;padding:5px 12px;border-radius:50px;border:1px solid rgba(184,154,90,0.3);backdrop-filter:blur(4px)}
.offer-card__badge--red{background:rgba(224,82,82,0.85);color:#fff;border-color:rgba(224,82,82,0.4)}
.offer-card__badge--green{background:rgba(39,174,96,0.85);color:#fff;border-color:rgba(39,174,96,0.4)}
.offer-card__body{padding:16px;display:flex;flex-direction:column;gap:6px}
.offer-card__name{font-size:15px;font-weight:700;color:#fff;line-height:1.2}
.offer-card__role{font-size:11px;color:#555;letter-spacing:.5px}
.offer-card__desc{font-size:12px;color:#666;line-height:1.5;margin-bottom:4px}
.offer-card__price{display:flex;align-items:center;gap:8px;margin:2px 0 4px}
.offer-card__old{font-size:13px;color:#444;text-decoration:line-through}
.offer-card__new{font-family:var(--ff-h);font-size:20px;font-weight:700;color:var(--gold-lt)}

/* ── SECTION BASE ── */
.section{padding:60px 20px;max-width:520px;margin:0 auto}
.section--grey{background:var(--bg2);max-width:100%}
.section--grey>*{max-width:520px;margin-left:auto;margin-right:auto}
.section__head{margin-bottom:28px}
.eyebrow{font-size:11px;letter-spacing:2.5px;text-transform:uppercase;color:var(--gold);font-weight:600;margin-bottom:8px;display:block}
.section__title{font-family:var(--ff-h);font-size:clamp(28px,8vw,38px);font-weight:700;letter-spacing:-0.5px;line-height:1.1;color:var(--ink)}
.section__title em{font-style:italic;color:var(--gold)}
.section__sub{font-size:14px;color:var(--muted);line-height:1.7;margin-top:8px}

/* ── SERVICES ── */
.services__grid{display:flex;flex-direction:column;gap:2px}
.svc-card{display:flex;align-items:center;gap:14px;padding:18px 16px;border-radius:14px;cursor:pointer;transition:background .2s;border:1px solid transparent}
.svc-card:hover{background:var(--gold-bg);border-color:rgba(184,154,90,0.2)}
.svc-card__icon{width:46px;height:46px;background:var(--bg2);border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:22px;flex-shrink:0}
.svc-card:hover .svc-card__icon{background:rgba(184,154,90,0.15)}
.svc-card__body{flex:1;min-width:0}
.svc-card__name{font-size:15px;font-weight:600;color:var(--ink);margin-bottom:2px}
.svc-card__desc{font-size:12px;color:var(--muted)}
.svc-card__right{display:flex;flex-direction:column;align-items:flex-end;gap:4px;flex-shrink:0}
.svc-card__price{font-family:var(--ff-h);font-size:16px;font-weight:700;color:var(--ink)}
.svc-card__arrow{font-size:14px;color:var(--gold);transition:transform .2s}
.svc-card:hover .svc-card__arrow{transform:translateX(3px)}

/* ── WHY US ── */
.why{background:var(--dark);padding:60px 0}
.why__inner{padding:0 20px;max-width:520px;margin:0 auto}
.why__grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-top:28px}
.why__item{background:var(--dark2);border:1px solid rgba(255,255,255,0.06);border-radius:var(--radius);padding:20px 16px;transition:border-color .25s,transform .2s}
.why__item:hover{border-color:rgba(184,154,90,0.25);transform:translateY(-2px)}
.why__icon{font-size:26px;margin-bottom:10px}
.why__title{font-size:14px;font-weight:600;color:#fff;margin-bottom:6px}
.why__desc{font-size:12px;color:#555;line-height:1.6}

/* ── TEAM ── */
.team__rail{display:flex;gap:12px;overflow-x:auto;scroll-snap-type:x mandatory;scrollbar-width:none;margin:0 -20px;padding:4px 20px 12px}
.team__rail::-webkit-scrollbar{display:none}
.team__card{flex-shrink:0;width:148px;scroll-snap-align:start;border:1px solid var(--border);border-radius:var(--radius);padding:18px 14px;text-align:center;transition:border-color .25s,transform .2s;cursor:pointer}
.team__card:hover{border-color:var(--gold);transform:translateY(-3px)}
.team__avatar{font-size:44px;margin-bottom:10px;line-height:1}
.team__name{font-size:14px;font-weight:600;color:var(--ink);margin-bottom:3px}
.team__role{font-size:11px;color:var(--muted);letter-spacing:.5px;margin-bottom:8px}
.team__avail{font-size:11px;font-weight:500;margin-bottom:12px}
.team__avail--on{color:#27ae60}
.team__avail--off{color:var(--muted)}
.team__book{width:100%;background:var(--ink);color:#fff;border:none;border-radius:50px;padding:8px 0;font-size:12px;font-weight:600;cursor:pointer;transition:background .2s}
.team__book:hover{background:var(--gold)}

/* ── REVIEWS ── */
.reviews__rail{display:flex;gap:12px;overflow-x:auto;scroll-snap-type:x mandatory;scrollbar-width:none;margin:0 -20px;padding:4px 20px 12px}
.reviews__rail::-webkit-scrollbar{display:none}
.rev-card{flex-shrink:0;width:270px;scroll-snap-align:start;background:var(--bg);border:1px solid var(--border);border-radius:var(--radius);padding:22px 20px;transition:border-color .2s,transform .2s}
.rev-card:hover{border-color:var(--gold);transform:translateY(-2px)}
.rev-card__stars{color:#f5a623;font-size:13px;letter-spacing:2px;margin-bottom:12px}
.rev-card__text{font-size:14px;line-height:1.7;color:var(--ink);margin-bottom:16px}
.rev-card__footer{display:flex;align-items:center;gap:10px}
.rev-card__avatar{font-size:28px}
.rev-card__name{font-size:13px;font-weight:600;color:var(--ink)}
.rev-card__date{font-size:11px;color:var(--muted)}

/* ── BOOK FORM ── */
.book-form{display:flex;flex-direction:column;gap:16px}
.book-form__field label{display:block;font-size:11px;letter-spacing:1.5px;text-transform:uppercase;color:var(--muted);font-weight:600;margin-bottom:8px}
.book-form__field input,.book-form__field select{width:100%;border:1.5px solid var(--border);border-radius:12px;padding:13px 16px;font-family:var(--ff-b);font-size:15px;color:var(--ink);background:var(--bg);outline:none;transition:border-color .25s;-webkit-appearance:none;appearance:none}
.book-form__field input:focus,.book-form__field select:focus{border-color:var(--gold)}
.time-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
.time-btn{background:var(--bg2);border:1.5px solid var(--border);border-radius:10px;padding:12px 6px;font-family:var(--ff-b);font-size:13px;font-weight:600;color:var(--ink);cursor:pointer;transition:all .2s}
.time-btn:hover{border-color:var(--gold)}
.time-btn--sel{background:var(--ink);border-color:var(--ink);color:#fff}

/* ── PAY CARD ── */
.pay-card{background:var(--bg);border:1px solid var(--border);border-radius:24px;overflow:hidden}
.pay-card__qr-wrap{display:flex;flex-direction:column;align-items:center;padding:32px 24px 24px;background:linear-gradient(145deg,#faf9f6,#f0ede6);border-bottom:1px solid var(--border);gap:16px}
.pay-card__qr-frame{position:relative;padding:12px;background:#fff;border-radius:20px;box-shadow:0 4px 24px rgba(0,0,0,0.10)}
.pay-card__qr-corner{position:absolute;width:20px;height:20px;border-color:var(--gold);border-style:solid}
.pay-card__qr-corner--tl{top:-2px;left:-2px;border-width:3px 0 0 3px;border-radius:6px 0 0 0}
.pay-card__qr-corner--tr{top:-2px;right:-2px;border-width:3px 3px 0 0;border-radius:0 6px 0 0}
.pay-card__qr-corner--bl{bottom:-2px;left:-2px;border-width:0 0 3px 3px;border-radius:0 0 0 6px}
.pay-card__qr-corner--br{bottom:-2px;right:-2px;border-width:0 3px 3px 0;border-radius:0 0 6px 0}
.pay-card__qr-img{display:block;width:200px;height:200px;border-radius:10px}
.pay-card__upi-id{display:flex;align-items:center;gap:10px;background:#fff;border:1.5px solid var(--border);border-radius:50px;padding:10px 16px;width:100%;max-width:280px}
.pay-card__upi-label{font-size:10px;letter-spacing:1.5px;text-transform:uppercase;color:var(--muted);font-weight:700;flex-shrink:0}
.pay-card__upi-val{flex:1;font-size:14px;font-weight:600;color:var(--ink);font-family:monospace}
.pay-card__copy{background:none;border:none;cursor:pointer;font-size:16px;transition:transform .15s}
.pay-card__copy:hover{transform:scale(1.2)}
.pay-card__apps{padding:20px 24px;border-bottom:1px solid var(--border)}
.pay-card__apps-label{font-size:11px;letter-spacing:1.5px;text-transform:uppercase;color:var(--muted);font-weight:600;margin-bottom:12px}
.pay-card__apps-row{display:flex;gap:16px}
.upi-app{display:flex;flex-direction:column;align-items:center;gap:5px}
.upi-app__icon{width:44px;height:44px;border-radius:14px;display:flex;align-items:center;justify-content:center;color:#fff;font-size:18px;font-weight:800;box-shadow:0 2px 8px rgba(0,0,0,0.15)}
.upi-app span{font-size:10px;color:var(--muted);font-weight:500}
.pay-card__steps{padding:20px 24px;display:flex;flex-direction:column;gap:12px;border-bottom:1px solid var(--border)}
.pay-step{display:flex;align-items:center;gap:12px}
.pay-step__num{width:28px;height:28px;background:var(--gold-bg);border:1.5px solid rgba(184,154,90,0.3);border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:700;color:var(--gold);flex-shrink:0}
.pay-step__text{font-size:13px;color:var(--ink);line-height:1.4}
.pay-card__note{display:flex;gap:10px;align-items:flex-start;padding:16px 24px;background:rgba(245,166,35,0.06);border-bottom:1px solid var(--border)}
.pay-card__note p{font-size:13px;color:var(--muted);line-height:1.5}
.pay-card__btn{border-radius:0;padding:18px;font-size:14px}

/* ── LOCATION ── */
.location__card{border:1px solid var(--border);border-radius:20px;overflow:hidden}
.location__map-preview{height:160px;background:linear-gradient(135deg,#e8e4dc,#d8d0c4);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:8px;position:relative}
.location__map-preview::before{content:'';position:absolute;inset:0;background:repeating-linear-gradient(0deg,transparent,transparent 29px,rgba(0,0,0,.05) 29px,rgba(0,0,0,.05) 30px),repeating-linear-gradient(90deg,transparent,transparent 29px,rgba(0,0,0,.05) 29px,rgba(0,0,0,.05) 30px)}
.location__map-pin{font-size:40px;position:relative;z-index:1;animation:bounce 1.4s ease-in-out infinite}
.location__map-label{background:var(--ink);color:#fff;font-size:12px;font-weight:600;padding:5px 14px;border-radius:20px;position:relative;z-index:1}
.location__info{padding:20px;display:flex;flex-direction:column;gap:16px}
.location__row{display:flex;gap:14px;align-items:flex-start}
.location__icon{font-size:20px;flex-shrink:0;margin-top:1px}
.location__lbl{font-size:11px;letter-spacing:1px;text-transform:uppercase;color:var(--muted);margin-bottom:3px;font-weight:600}
.location__val{font-size:14px;color:var(--ink);line-height:1.6}
.location__card .btn--outline{border-radius:0;border:none;border-top:1px solid var(--border)}

/* ── CONTACT ── */
.contact__list{display:flex;flex-direction:column;gap:10px}
.contact__item{display:flex;align-items:center;gap:14px;background:var(--bg2);border:1px solid var(--border);border-radius:var(--radius);padding:16px 18px;text-decoration:none;transition:border-color .2s,transform .15s}
.contact__item:active{transform:scale(.98)}
.contact__item:hover{border-color:var(--gold)}
.contact__item--wa{background:rgba(37,211,102,0.06);border-color:rgba(37,211,102,0.2)}
.contact__item--wa:hover{border-color:var(--wa)}
.contact__item-icon{width:44px;height:44px;background:var(--bg);border-radius:12px;border:1px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0}
.contact__item-text{flex:1}
.contact__item-lbl{font-size:11px;letter-spacing:1px;text-transform:uppercase;color:var(--muted);font-weight:600;margin-bottom:2px}
.contact__item-val{font-size:14px;color:var(--ink);font-weight:500}
.contact__item-arrow{font-size:16px;color:var(--gold)}

/* ── FOOTER ── */
.footer{background:var(--dark);padding:48px 20px 32px;text-align:center}
.footer__logo{font-family:var(--ff-h);font-size:30px;font-weight:700;color:#fff;letter-spacing:1px;margin-bottom:6px;display:flex;align-items:center;justify-content:center;gap:6px}
.footer__logo-dot{width:7px;height:7px;background:var(--gold);border-radius:50%}
.footer__tagline{font-size:12px;color:#555;letter-spacing:1px;margin-bottom:24px}
.footer__links{display:flex;justify-content:center;gap:20px;margin-bottom:24px;flex-wrap:wrap}
.footer__links a{font-size:13px;color:#666;text-decoration:none;transition:color .2s}
.footer__links a:hover{color:var(--gold)}
.footer__copy{font-size:12px;color:#444}

/* ── BOTTOM BAR ── */
.bottom-bar{position:fixed;bottom:0;left:0;right:0;z-index:150;display:flex;background:#fff;border-top:1px solid var(--border);padding:10px 12px;padding-bottom:calc(10px + env(safe-area-inset-bottom))}
.bottom-bar__btn{flex:1;display:flex;align-items:center;justify-content:center;gap:6px;font-family:var(--ff-b);font-size:13px;font-weight:700;padding:12px 6px;border-radius:12px;text-decoration:none;border:none;cursor:pointer;transition:all .2s;margin:0 4px}
.bottom-bar__btn:active{transform:scale(.95)}
.bottom-bar__btn--call{background:var(--bg2);color:var(--ink);border:1px solid var(--border)}
.bottom-bar__btn--whatsapp{background:var(--wa);color:#fff}
.bottom-bar__btn--book{background:var(--ink);color:#fff}
.bottom-bar__btn--book:hover{background:var(--gold)}

/* ── TOAST ── */
.toast{position:fixed;bottom:84px;left:50%;transform:translateX(-50%);background:var(--ink);color:#fff;padding:13px 22px;border-radius:50px;font-size:14px;font-weight:500;z-index:300;white-space:nowrap;border-left:3px solid var(--gold);box-shadow:0 4px 20px rgba(0,0,0,.15)}
.toast-fade-enter-active,.toast-fade-leave-active{transition:all .3s ease}
.toast-fade-enter-from,.toast-fade-leave-to{opacity:0;transform:translateX(-50%) translateY(12px)}

/* ── LIGHTBOX ── */
.lightbox{position:fixed;inset:0;z-index:500;background:rgba(0,0,0,0.92);display:flex;align-items:center;justify-content:center;padding:20px}
.lb__img{max-width:100%;max-height:90vh;border-radius:16px;object-fit:contain}
.lb__close{position:absolute;top:20px;right:20px;background:rgba(255,255,255,0.15);border:none;color:#fff;width:40px;height:40px;border-radius:50%;font-size:16px;cursor:pointer;transition:background .2s}
.lb__close:hover{background:var(--gold)}
.lb-fade-enter-active,.lb-fade-leave-active{transition:opacity .25s}
.lb-fade-enter-from,.lb-fade-leave-to{opacity:0}

/* ── ANIMATIONS ── */
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.5;transform:scale(.85)}}
@keyframes bounce{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
</style>