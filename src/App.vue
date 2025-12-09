<template>
  <div>
    <!-- Navbar -->
    <section id="navbar" style="font-family: 'Oswald', monospace">
      <nav class="navbar fixed-top navbar-expand-lg navbar-dark">
        <div class="container-fluid px-8">
          <!-- โลโก้ของเว็บ -->
          <h1
            class="navbar-brand mb-0 h1 logo"
            @click="scrollToSection('Home')"
          >
            <span class="logo-first">Pattrira</span>
            <span class="logo-dot">.</span>
            <span class="logo-last">T</span>
          </h1>

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
              <!-- <li class="nav-item">
                <a
                  class="nav-link"
                  :class="{ active: activeSection === 'Skills' }"
                  @click="scrollToSection('Skills')"
                >
                  <span class="nav-text">Skills</span>
                  <span class="nav-underline"></span>
                </a>
              </li> -->
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

    <!-- Section ต่าง ๆ-->
    <section id="Home" ref="Home">
      <Home />
    </section>

    <section id="About" ref="About">
      <About />
    </section>

    <section id="Achievement" ref="Achievement">
      <Achievement />
    </section>

    <!-- <section id="Education" ref="Education">
      <Education />
    </section> -->

    <section id="Contact" ref="Contact">
      <Contact />
    </section>
  </div>
</template>

<script>
import About from "./components/About.vue";
import Education from "./components/Education.vue";
import Contact from "./components/Contact.vue";
import Home from "./components/Home.vue";
import Achievement from "./components/Achievement.vue";

export default {
  name: "App",
  components: {
    Home,
    About,
    Education,
    Achievement,
    Contact,
  },
  data() {
    return {
      activeSection: "Home",
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    scrollToSection(sectionId) {
      this.activeSection = sectionId;
      document
        .getElementById(sectionId)
        ?.scrollIntoView({ behavior: "smooth" });
    },
    handleScroll() {
      const sections = ["Home", "About", "Education", "Achievement", "Skills", "Contact"];
      const scrollPosition = window.scrollY + 100;
      for (let section of sections) {
        const element = document.getElementById(section);
        if (element) {
          const offsetTop = element.offsetTop;
          const offsetHeight = element.offsetHeight;

          if (
            scrollPosition >= offsetTop &&
            scrollPosition < offsetTop + offsetHeight
          ) {
            this.activeSection = section;
            break;
          }
        }
      }
    },
  },
};
</script>

<style scoped>
/* Navbar */
.navbar {
  background: linear-gradient(135deg, #000000 0%, #1a1a1a 100%);
  color: white;
  padding: 10px 30px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.navbar::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, #ffffff, transparent);
  opacity: 0.3;
}

/* Logo Animation */
.logo {
  font-size: 1.5rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.logo:hover {
  transform: scale(1.05);
}

.logo-first {
  color: white;
  transition: color 0.3s ease;
}

.logo-dot {
  color: #ffffff;
  font-size: 2rem;
  animation: pulse 2s ease-in-out infinite;
}

.logo-last {
  color: #cccccc;
  transition: color 0.3s ease;
}

.logo:hover .logo-first {
  color: #ffffff;
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}

.logo:hover .logo-last {
  color: #ffffff;
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
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

/* Nav Links */
.nav-item {
  margin: 0 8px;
}

.nav-link {
  color: rgba(255, 255, 255, 0.8);
  font-size: 1rem;
  font-weight: 500;
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
  background: linear-gradient(90deg, #ffffff, #cccccc);
  transition: all 0.3s ease;
  transform: translateX(-50%);
}

.nav-link:hover {
  color: #ffffff;
  transform: translateY(-2px);
}

.nav-link:hover .nav-underline {
  width: 100%;
}

.nav-link.active {
  color: #ffffff;
}

.nav-link.active .nav-underline {
  width: 100%;
}

/* Contact Button */
.contact-btn {
  background: linear-gradient(135deg, #ffffff 0%, #cccccc 100%);
  color: #000000 !important;
  border-radius: 25px;
  padding: 8px 24px !important;
  margin-left: 15px;
  font-weight: 500;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.contact-btn::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.1);
  transform: translate(-50%, -50%);
  transition: width 0.5s ease, height 0.5s ease;
}

.contact-btn:hover::before {
  width: 300px;
  height: 300px;
}

.contact-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 20px rgba(255, 255, 255, 0.3);
}

.contact-btn .nav-underline {
  display: none;
}

/* Responsive */
@media (max-width: 768px) {
  .navbar {
    padding: 10px 20px;
  }

  .logo {
    font-size: 1.5rem;
  }

  .nav-link {
    font-size: 1rem;
    padding: 6px 12px;
  }

  .contact-btn {
    margin-left: 0;
    margin-top: 3px;
  }
}

/* Other sections styles */
.title {
  font-size: 36px;
  font-weight: bold;
}

.quote h2 {
  font-size: 24px;
  font-weight: bold;
}

.quote h4 {
  font-size: 18px;
  font-weight: normal;
}

.profile-img img {
  width: 500px;
  height: 500px;
  border-radius: 50%;
  object-fit: cover;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.profile-img {
  padding: 8rem;
  padding-left: 8px;
}

@media (max-width: 768px) {
  .home-section {
    flex-direction: column;
    text-align: center;
    height: auto;
    padding: 10vw 5vw;
  }

  .text-content {
    max-width: 100%;
  }

  .profile-img {
    margin-top: 20px;
  }
}
</style>