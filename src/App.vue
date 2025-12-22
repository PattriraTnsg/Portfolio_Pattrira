<template>
  <div>
    <Navbar />

    <section id="Home" ref="Home">
      <Home />
    </section>

    <section id="About" ref="About">
      <About />
    </section>

    <section id="Achievement" ref="Achievement">
      <Achievement />
    </section>

    <section id="Skills" ref="Skills">
      <Skills />
    </section>

    <section id="Contact" ref="Contact">
      <Contact />
    </section>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Home from "./components/Home.vue";
import About from "./components/About.vue";
import Skills from "./components/Skills.vue";
import Achievement from "./components/Achievement.vue";
import Contact from "./components/Contact.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Home,
    About,
    Skills,
    Achievement,
    Contact,
  },
  data() {
    return {
      activeSection: "Home",
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll, { passive: true });
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
      const sections = [
        "Home",
        "About",
        "Education",
        "Achievement",
        "Skills",
        "Contact",
      ];
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

</style>
