<template>
  <!-- Home Section -->
  <section
    id="Home"
    class="home-section"
    style="font-family: 'Oswald', monospace"
  >
    <!-- Animated Background Elements -->
    <div class="bg-animation">
      <div class="circle circle-1"></div>
      <div class="circle circle-2"></div>
      <div class="circle circle-3"></div>
    </div>

    <div class="home-content">
      <div class="greeting">
        <span class="wave">👋</span>
        <span class="greeting-text">Hello, I'm</span>
      </div>
      <h1 class="title">
        <span
          class="title-line"
          v-for="(char, index) in titleText"
          :key="index"
          :style="{ animationDelay: index * 0.05 + 's' }"
        >
          {{ char === " " ? "\u00A0" : char }}
        </span>
      </h1>
      <p class="subtitle">Computer Science 3rd</p>

      <div class="btn-group">
        <button class="start-btn primary" @click="scrollToSection('About')">
          <span>Explore My Portfolio</span>
          <svg
            class="arrow"
            width="20"
            height="20"
            viewBox="0 0 20 20"
            fill="none"
          >
            <path
              d="M7 10L12 15L17 10"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
            />
          </svg>
        </button>
        <button class="start-btn secondary" @click="scrollToSection('Contact')">
          <span>Contact Me</span>
        </button>
      </div>

      <!-- Social Links -->
      <div class="social-links">
        <!-- LinkedIn -->
        <a
          href="https://www.linkedin.com/in/pattrira-tanongsaksrikun-01b223398/"
          class="social-icon"
        >
          <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
            <path
              d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"
            />
          </svg>
        </a>
        <!-- Github -->
        <a href="https://github.com/PattriraTnsg" class="social-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
            <path
              d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
            />
          </svg>
        </a>
        <!-- SocialMedia -->
        <a href="/pattrira_resume.pdf" class="social-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
            <path
              d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8l-6-6zm1 7h5l-5-5v5zM8 13h8v2H8v-2zm0 5h5v2H8v-2z"
            />
          </svg>
        </a>
      </div>
    </div>

    <div class="profile-section">
      <div class="profile-img-wrapper">
        <div class="profile-ring ring-1"></div>
        <div class="profile-ring ring-2"></div>
        <div class="profile-ring ring-3"></div>
        <div class="profile-img">
          <img :src="profile" alt="Profile" />
        </div>
      </div>

      <!-- Floating Info Cards -->
      <div class="info-card card-1">
        <div class="card-icon">💻</div>
      </div>

      <div class="info-card-nobg card-2">
        <div class="card-icon">
            <img :src="BrudaLogo" alt="Bruda Logo" />
        </div>
      </div>

      <div class="info-card card-3">
        <div class="card-icon">
            <img :src="SciLogo" alt="Sci Logo" />
        </div>
      </div>
    </div>

    <!-- Scroll Indicator -->
    <div class="scroll-indicator">
      <div class="mouse" @click="scrollToSection('About')">
        <div class="wheel"></div>
      </div>
      <p>Scroll Down</p>
    </div>
  </section>
</template>

<script>
import profile from "@/assets/image/me.jpg";
import SciLogo from "@/assets/image/Sci_Logo_Eng.png";
import BrudaLogo from "@/assets/image/Bruda.png";
import About from "@/components/About.vue";

export default {
  name: "HomeSection",
  components: { About },
  data() {
    return {
      profile,
      SciLogo,
      BrudaLogo,
      titleText: "Pattrira Tanongsaksrikung",
    };
  },
  methods: {
    scrollToSection(sectionId) {
      document
        .getElementById(sectionId)
        ?.scrollIntoView({ behavior: "smooth" });
    },
  },
};
</script>

<style scoped>
/* ===============================================
   BASE STYLES - ใช้ได้กับทุกหน้าจอ
   =============================================== */

/* Home Section */
.home-section {
  background: rgb(242, 204, 123);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 5rem 10vw;
  position: relative;
  overflow: hidden;
}

/* Animated Background */
.bg-animation {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: 0;
  pointer-events: none;
}

.circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  animation: float 20s infinite ease-in-out;
}

.circle-1 {
  width: 300px;
  height: 300px;
  top: 10%;
  left: 10%;
  animation-delay: 0s;
}

.circle-2 {
  width: 200px;
  height: 200px;
  bottom: 20%;
  right: 15%;
  animation-delay: 5s;
}

.circle-3 {
  width: 150px;
  height: 150px;
  top: 60%;
  left: 70%;
  animation-delay: 10s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  33% {
    transform: translateY(-30px) rotate(120deg);
  }
  66% {
    transform: translateY(30px) rotate(240deg);
  }
}

/* Content */
.home-content {
  max-width: 50%;
  z-index: 1;
  animation: fadeInLeft 1s ease-out;
}

@keyframes fadeInLeft {
  from {
    opacity: 0;
    transform: translateX(-50px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.greeting {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
  font-size: clamp(1.2rem, 3vw, 1.5rem);
  color: #444a56;
}

.wave {
  display: inline-block;
  animation: wave 2s infinite;
  transform-origin: 70% 70%;
  font-size: clamp(1.5rem, 4vw, 2rem);
}

@keyframes wave {
  0%, 100% { transform: rotate(0deg); }
  10%, 30% { transform: rotate(14deg); }
  20% { transform: rotate(-8deg); }
  40%, 100% { transform: rotate(0deg); }
}

.greeting-text {
  font-weight: 300;
  letter-spacing: 1px;
}

.title {
  font-size: clamp(2rem, 5vw, 3rem);
  margin: 0 0 1rem 0;
  color: #2c3e50;
  line-height: 1.2;
  word-wrap: break-word;
  overflow-wrap: break-word;
}

.title-line {
  display: inline-block;
  opacity: 0;
  animation: fadeInUp 0.5s forwards;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.subtitle {
  font-size: clamp(1.2rem, 3vw, 1.5rem);
  color: #555;
  margin-bottom: 2.5rem;
  font-weight: 300;
  animation: fadeIn 1s ease-out 0.5s backwards;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* Buttons */
.btn-group {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 3rem;
  animation: fadeIn 1s ease-out 0.8s backwards;
  flex-wrap: wrap;
}

.start-btn {
  font-size: clamp(0.95rem, 2vw, 1.1rem);
  padding: 0.75rem 2rem;
  border-radius: 50px;
  cursor: pointer;
  border: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
  font-weight: 600;
  letter-spacing: 0.5px;
  white-space: nowrap;
}

.start-btn.primary {
  background: linear-gradient(135deg, #444a56 0%, #2c3e50 100%);
  color: white;
  box-shadow: 0 10px 25px rgba(68, 74, 86, 0.3);
}

.start-btn.primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 15px 35px rgba(68, 74, 86, 0.4);
}

.start-btn.primary .arrow {
  transition: transform 0.3s ease;
}

.start-btn.primary:hover .arrow {
  transform: translateY(3px);
}

.start-btn.secondary {
  background: transparent;
  color: #444a56;
  border: 2px solid #444a56;
}

.start-btn.secondary:hover {
  background: #444a56;
  color: white;
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(68, 74, 86, 0.2);
}

/* Social Links */
.social-links {
  display: flex;
  gap: 1.5rem;
  animation: fadeIn 1s ease-out 1s backwards;
}

.social-icon {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background: rgba(68, 74, 86, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #444a56;
  transition: all 0.3s ease;
  cursor: pointer;
  flex-shrink: 0;
}

.social-icon:hover {
  background: #444a56;
  color: white;
  transform: translateY(-5px);
}

/* Profile Section */
.profile-section {
  position: relative;
  z-index: 1;
  animation: fadeInRight 1s ease-out;
  flex-shrink: 0;
}

@keyframes fadeInRight {
  from {
    opacity: 0;
    transform: translateX(50px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.profile-img-wrapper {
  position: relative;
  width: clamp(250px, 30vw, 400px);
  height: clamp(250px, 30vw, 400px);
  margin: 0 auto;
}

.profile-ring {
  position: absolute;
  border-radius: 50%;
  border: 2px solid rgba(68, 74, 86, 0.2);
  animation: rotate 20s linear infinite;
}

.ring-1 {
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.ring-2 {
  width: 110%;
  height: 110%;
  top: -5%;
  left: -5%;
  animation-direction: reverse;
  animation-duration: 15s;
}

.ring-3 {
  width: 120%;
  height: 120%;
  top: -10%;
  left: -10%;
  animation-duration: 25s;
}

@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.profile-img {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
  transition: all 0.5s ease;
}

.profile-img:hover {
  transform: scale(1.05);
  box-shadow: 0 25px 80px rgba(0, 0, 0, 0.3);
}

.profile-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Info Cards */
.info-card {
  position: absolute;
  background: white;
  padding: 0.875rem 1.25rem;
  border-radius: 50px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  animation: floatCard 3s ease-in-out infinite;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
  backdrop-filter: blur(10px);
  background: rgba(255, 255, 255, 0.95);
}

.info-card:hover {
  transform: translateY(-15px) scale(1.05) !important;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2);
}

.info-card-nobg {
  position: absolute;
  padding: 0;
  border-radius: 50px;
  display: flex;
  align-items: center;
  animation: floatCard 4s ease-in-out infinite;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.info-card-nobg:hover {
  transform: translateY(-15px) scale(1.1) !important;
}

.card-1 {
  top: 10%;
  right: -10%;
  animation-delay: 0s;
}

.card-2 {
  bottom: 15%;
  right: -15%;
  animation-delay: 1.5s;
}

.card-3 {
  bottom: 50%;
  left: -20%;
  animation-delay: 3s;
}

@keyframes floatCard {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

.card-icon {
  font-size: 2rem;
  transition: transform 0.4s ease;
}

.info-card:hover .card-icon,
.info-card-nobg:hover .card-icon {
  transform: scale(1.2) rotate(5deg);
}

.card-icon img {
  width: clamp(80px, 12vw, 150px);
  height: auto;
  object-fit: contain;
  transition: all 0.4s ease;
}

/* Scroll Indicator */
.scroll-indicator {
  position: absolute;
  bottom: 3rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  color: #444a56;
  animation: fadeIn 1s ease-out 1.5s backwards;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
}

.scroll-indicator:hover {
  transform: translateX(-50%) translateY(-5px);
}

.mouse {
  width: 30px;
  height: 50px;
  border: 2px solid #444a56;
  border-radius: 15px;
  display: flex;
  justify-content: center;
  padding-top: 8px;
  transition: all 0.3s ease;
}

.scroll-indicator:hover .mouse {
  border-color: #2c3e50;
  box-shadow: 0 5px 15px rgba(68, 74, 86, 0.2);
}

.wheel {
  width: 4px;
  height: 10px;
  background: #444a56;
  border-radius: 2px;
  animation: scroll 1.5s infinite;
}

@keyframes scroll {
  0% {
    transform: translateY(0);
    opacity: 1;
  }
  100% {
    transform: translateY(15px);
    opacity: 0;
  }
}

.scroll-indicator p {
  font-size: 0.85rem;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.scroll-indicator:hover p {
  color: #2c3e50;
  letter-spacing: 1.5px;
}

/* ===============================================
   RESPONSIVE BREAKPOINTS
   =============================================== */

/* Tablet Landscape - 1024px */
@media (max-width: 1024px) {
  .home-section {
    padding: 4rem 6vw;
    gap: 3rem;
  }

  .home-content {
    max-width: 55%;
  }

  .profile-img-wrapper {
    width: 300px;
    height: 300px;
  }

  .card-icon img {
    width: 100px;
  }
}

/* Tablet Portrait - 768px */
@media (max-width: 768px) {
  .home-section {
    flex-direction: column;
    justify-content: center;
    min-height: 100svh;
    padding: 100px 5vw 60px;
    text-align: center;
    gap: 2rem;
  }

  /* Background circles - ลดขนาดให้เหมาะกับหน้าจอเล็ก */
  .circle-1 {
    width: 200px;
    height: 200px;
  }

  .circle-2 {
    width: 150px;
    height: 150px;
  }

  .circle-3 {
    width: 100px;
    height: 100px;
  }

  .home-content {
    max-width: 100%;
    animation: fadeInUp 0.8s ease-out;
  }

  .greeting {
    justify-content: center;
  }

  .title {
    font-size: 2rem;
    line-height: 1.3;
  }

  .subtitle {
    font-size: 1.2rem;
    margin-bottom: 2rem;
  }

  .btn-group {
    flex-direction: row;
    gap: 1rem;
    justify-content: center;
    margin-bottom: 2rem;
  }

  .start-btn {
    padding: 0.75rem 1.5rem;
    font-size: 0.95rem;
  }

  .social-links {
    justify-content: center;
    gap: 1rem;
  }

  .social-icon {
    width: 40px;
    height: 40px;
  }

  .profile-section {
    margin-top: 0;
    order: -1;
  }

  .profile-img-wrapper {
    width: 250px;
    height: 250px;
  }

  .profile-img:hover {
    transform: scale(1.02);
  }

  /* Info Cards - ซ่อนในหน้าจอ tablet */
  .info-card,
  .info-card-nobg {
    display: none;
  }

  .scroll-indicator {
    bottom: 2rem;
  }
}

/* Mobile Large - 480px */
@media (max-width: 480px) {
  .home-section {
    padding: 80px 5vw 40px;
  }

  .greeting {
    font-size: 1.1rem;
    gap: 0.3rem;
  }

  .wave {
    font-size: 1.5rem;
  }

  .title {
    font-size: 1.6rem;
    margin-bottom: 0.75rem;
  }

  .subtitle {
    font-size: 1rem;
    margin-bottom: 1.5rem;
  }

  .btn-group {
    flex-direction: column;
    gap: 0.75rem;
    width: 100%;
  }

  .start-btn {
    width: 100%;
    max-width: 100%;
    padding: 0.875rem 1.5rem;
    font-size: 0.9rem;
  }

  .social-links {
    gap: 0.75rem;
  }

  .social-icon {
    width: 38px;
    height: 38px;
  }

  .social-icon svg {
    width: 20px;
    height: 20px;
  }

  .profile-img-wrapper {
    width: 200px;
    height: 200px;
  }

  .profile-ring {
    border-width: 1.5px;
  }

  .scroll-indicator {
    bottom: 1.5rem;
  }

  .scroll-indicator p {
    font-size: 0.75rem;
  }

  .mouse {
    width: 25px;
    height: 45px;
  }
}

/* Mobile Small - 375px */
@media (max-width: 375px) {
  .home-section {
    padding: 70px 4vw 30px;
  }

  .title {
    font-size: 1.4rem;
  }

  .subtitle {
    font-size: 0.95rem;
  }

  .start-btn {
    padding: 0.75rem 1.25rem;
    font-size: 0.85rem;
  }

  .profile-img-wrapper {
    width: 180px;
    height: 180px;
  }
}

/* Landscape Mode สำหรับ Mobile */
@media (max-height: 600px) and (orientation: landscape) {
  .home-section {
    flex-direction: row;
    min-height: 100vh;
    padding: 2rem 5vw;
  }

  .home-content {
    max-width: 50%;
    text-align: left;
  }

  .greeting {
    justify-content: flex-start;
  }

  .profile-section {
    order: 0;
  }

  .profile-img-wrapper {
    width: 200px;
    height: 200px;
  }

  .scroll-indicator {
    display: none;
  }

  .btn-group {
    flex-direction: row;
    justify-content: flex-start;
  }

  .social-links {
    justify-content: flex-start;
  }
}

/* ===============================================
   ACCESSIBILITY & PERFORMANCE
   =============================================== */

/* Reduce Motion สำหรับผู้ที่ไม่ชอบ animation */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
  .start-btn.primary {
    border: 2px solid #fff;
  }

  .social-icon {
    border: 2px solid #444a56;
  }
}
</style>