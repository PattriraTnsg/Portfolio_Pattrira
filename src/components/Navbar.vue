<template>
  <section id="navbar" style="font-family: 'Oswald', monospace">
    <nav 
      :class="['navbar', 'fixed-top', 'navbar-expand-lg', {
        shrink: isScrolled,
        'light-text': isLightMode
      }]"
    >
      <div class="container-fluid px-8">
        <h1
          class="navbar-brand mb-0 h1 logo"
          @click="scrollToSection('Home')"
        >
          <span class="logo-first">Pattrira</span>
          <span class="logo-dot">.</span>
          <span class="logo-last">T</span>
        </h1>

        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
        >
          <svg
            width="28"
            height="28"
            viewBox="0 0 24 24"
            fill="none"
            :stroke="isLightMode ? '#ffffff' : '#2b1a00'"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <line x1="3" y1="6" x2="21" y2="6" />
            <line x1="3" y1="12" x2="21" y2="12" />
            <line x1="3" y1="18" x2="21" y2="18" />
          </svg>
        </button>

        <!--link แต่ละ Section -->
        <div
          class="collapse navbar-collapse justify-content-end"
          id="navbarNav"
        >
          <ul class="navbar-nav">
            <li class="nav-item">
              <a
                class="nav-link"
                :class="{ active: activeSection === 'Home' }"
                @click="scrollToSection('Home')"
              >
                <span class="nav-text">Home</span>
                <span class="nav-underline"></span>
              </a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link"
                :class="{ active: activeSection === 'About' }"
                @click="scrollToSection('About')"
              >
                <span class="nav-text">About</span>
                <span class="nav-underline"></span>
              </a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link"
                :class="{ active: activeSection === 'Achievement' }"
                @click="scrollToSection('Achievement')"
              >
                <span class="nav-text">Acheivements</span>
                <span class="nav-underline"></span>
              </a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link"
                :class="{ active: activeSection === 'Skills' }"
                @click="scrollToSection('Skills')"
              >
                <span class="nav-text">Skills</span>
                <span class="nav-underline"></span>
              </a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link contact-btn"
                @click="scrollToSection('Contact')"
              >
                <span class="nav-text">Contact</span>
              </a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </section>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {
      menuOpen: false,
      isScrolled: false,
      activeSection: "Home",
      isLightMode: false,
      links: [
        { id: "Home", label: "Home" },
        { id: "About", label: "About" },
        { id: "Achievement", label: "Achievement" },
        { id: "Skills", label: "Skills" },
        { id: "Contact", label: "Contact" },
      ],
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
    this.handleScroll();
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen;
    },
    scrollToSection(id) {
      const section = document.getElementById(id);
      if (section) {
        section.scrollIntoView({ behavior: "smooth" });
      }
      this.menuOpen = false;
    },
    handleScroll() {
      const scrollY = window.scrollY;
      this.isScrolled = scrollY > 60;

      for (const link of this.links) {
        const section = document.getElementById(link.id);
        if (!section) continue;

        const rect = section.getBoundingClientRect();
        if (rect.top <= 120 && rect.bottom >= 120) {
          this.activeSection = link.id;
          this.isLightMode = ["About","Skills"].includes(link.id);
          break;
        }
      }
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

body {
  overflow-x: hidden;
}

/* Glassmorphism Navbar Style */
.navbar {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 40px);
  max-width: 1200px;
  z-index: 1000;

  background: rgba(255, 255, 255, 0.28);
  backdrop-filter: blur(16px);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.18);
  padding: 14px 30px;

  transition: all 0.35s ease;
}

.navbar.shrink {
  top: 10px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(20px);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15);
  padding: 10px 22px;
}

.navbar-toggler {
  border: none;
  background: transparent;
}

.navbar-toggler:focus {
  box-shadow: none;
}


/* Logo Styles */
.logo {
  font-size: clamp(1.3rem, 4vw, 1.6rem);
  font-weight: 800;
  letter-spacing: 0.4px;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.logo:hover {
  transform: scale(1.05);
}

.logo-first {
  color: #2b1a00;
  transition: color 0.35s ease;
}

.logo-dot {
  color: #5c4403;
  font-size: 2rem;
  animation: pulse 2s ease-in-out infinite;
}

.logo-last {
  color: #2b1a00;
  transition: color 0.35s ease;
}

.logo:hover .logo-first,
.logo:hover .logo-last {
  color: #ffb703;
  text-shadow: 0 0 10px rgba(255, 183, 3, 0.3);
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.7;
  }
}

/* Navigation Links */
.nav-item {
  margin: 0 8px;
}

.nav-link {
  color: #2b1a00;
  font-size: 1.05rem;
  font-weight: 650;
  cursor: pointer;
  padding: 8px 16px;
  position: relative;
  transition: all 0.3s ease;
  display: inline-block;
}

.nav-text {
  position: relative;
  z-index: 1;
}

.nav-underline {
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background:  #840404;
  transition: all 0.3s ease;
  transform: translateX(-50%);
}

.nav-link:hover {
  color:  #840404;
  transform: translateY(-2px);
}

.nav-link:hover .nav-underline {
  width: 100%;
}

.nav-link.active {
  color:   #840404;
}

.nav-link.active .nav-underline {
  width: 100%;
}

/* Contact Button */
.contact-btn {
  background: linear-gradient(135deg, #dcbc6d 0%, #ff8b06 100%);
  color: #ffffff !important;
  border-radius: 25px;
  padding: 8px 24px !important;
  margin-left: 15px;
  font-weight: 600;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 183, 3, 0.3);
}

.contact-btn::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  transform: translate(-50%, -50%);
  transition: width 0.5s ease, height 0.5s ease;
}

.contact-btn:hover::before {
  width: 300px;
  height: 300px;
}

.contact-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 183, 3, 0.4);
}

.contact-btn .nav-underline {
  display: none;
}

/* Light Mode (for dark sections) */
.navbar.light-text {
  background: rgba(0, 0, 0, 0.28);
  backdrop-filter: blur(16px);
}

.navbar.light-text.shrink {
  background: rgba(0, 0, 0, 0.2);
}

.navbar.light-text .logo-first,
.navbar.light-text .logo-last {
  color: #ffffff;
}

.navbar.light-text .logo-dot {
  color: #ffd84d;
}

.navbar.light-text .nav-link {
  color: #ffffff;
}

.navbar.light-text .nav-link:hover {
  color: #ffd84d;
}

.navbar.light-text .nav-link.active {
  color: #ffd84d;
}

.navbar.light-text .nav-underline {
  background: #ffd84d;
}

.navbar.light-text .logo:hover .logo-first,
.navbar.light-text .logo:hover .logo-last {
  color: #ffd84d;
  text-shadow: 0 0 10px rgba(255, 216, 77, 0.4);
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .navbar {
    padding: 8px 16px;
    top: 10px;
    width: calc(100% - 20px);
  }

  .navbar.shrink {
    padding: 8px 16px;
  }

  .nav-item {
    margin: 4px 0;
  }

  .nav-link {
    font-size: 0.95rem;
    padding: 10px 12px;
    min-height: 44px;
  }

  .nav-underline {
    display: none;
  }

  .contact-btn {
    margin-left: 0;
    margin-top: 8px;
    width: 100%;
    text-align: center;
  }

  .navbar-collapse {
    margin-top: 12px;
    padding: 12px;
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(12px);
    border-radius: 12px;
  }

  .navbar.light-text .navbar-collapse {
    background: rgba(0, 0, 0, 0.15);
  }
}

@media (max-width: 576px) {
  .navbar {
    border-radius: 15px;
  }

  .logo {
    font-size: 1.2rem;
  }

  .logo-dot {
    font-size: 1.5rem;
  }
}
</style>