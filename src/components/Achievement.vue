<template>
  <section class="Achievements-section" style="font-family: 'Oswald', monospace" id="achievements">
    <h1 class="cert-title" v-reveal>Achievements</h1>

    <!-- Loading State -->
    <div v-if="loading" class="loading">Loading achievements...</div>

    <!-- Filters -->
    <div class="filters" v-reveal v-if="!loading">
      <button
        v-for="t in ['All', ...allTags]"
        :key="t"
        class="chip"
        :class="{ active: activeTag === t }"
        @click="activeTag = t"
      >
        {{ t }}
      </button>
    </div>

    <!-- Grid -->
    <div class="grid" v-if="!loading">
      <article
        v-for="(c, i) in filtered"
        :key="i"
        class="card"
        @click="open(c)"
        v-reveal
        :style="{ '--delay': (i * 80) + 'ms' }"
      >
        <div class="thumb">
          <img :src="c.images?.[0] ?? c.image" :alt="c.title" loading="lazy" />
          <span v-if="c.images?.length" class="multi-badge">{{ c.images.length }} images</span>
        </div>

        <div class="meta">
          <h3 class="ctitle">{{ c.title }}</h3>
          <p class="issuer">{{ c.issuer }}</p>

          <div class="cert-tags">
            <span class="cert-tag" v-for="t in c.tags" :key="t">{{ t }}</span>
          </div>

          <p class="date">{{ formatDate(c.date) }}</p>
        </div>
      </article>
    </div>

    <!-- Lightbox -->
    <div v-if="active" class="lightbox" @click.self="close" v-reveal>
      <div class="lightbox-inner" v-reveal style="--delay:120ms">
        
        <div class="viewer" v-reveal style="--delay:160ms">
          <button
            v-if="active.images?.length"
            class="nav prev"
            @click.stop="prev"
            aria-label="Previous image"
          >&#10094;</button>

          <img
            :src="active.images?.length ? active.images[activeIndex] : active.image"
            :alt="active.title"
          />

          <button
            v-if="active.images?.length"
            class="nav next"
            @click.stop="next"
            aria-label="Next image"
          >&#10095;</button>

          <div v-if="active.images?.length" class="thumbs">
            <button
              v-for="(img, idx) in active.images"
              :key="idx"
              class="thumb-btn"
              :class="{ active: idx === activeIndex }"
              @click.stop="activeIndex = idx"
            >
              <img :src="img" :alt="`thumb-${idx}`" />
            </button>
          </div>
        </div>

        <div class="lb-info" v-reveal style="--delay:200ms">
          <h3>{{ active.title }}</h3>
          <p class="issuer">{{ active.issuer }} • {{ formatDate(active.date) }}</p>
          <p class="desc" v-if="active.description">{{ active.description }}</p>
        </div>

        <button class="btn ghost close-btn" @click="close">Close</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Achievements',

  directives: {
    reveal: {
      mounted(el) {
        el.classList.add('reveal');
        const io = new IntersectionObserver(
          (entries) => {
            entries.forEach((e) => {
              if (e.isIntersecting) {
                e.target.classList.add('in-view');
              } else {
                e.target.classList.remove('in-view');
              }
            });
          },
          { threshold: 0.15, rootMargin: '0px 0px -10% 0px' }
        );

        el.__io = io;
        io.observe(el);
      },
      unmounted(el) {
        if (el.__io) {
          el.__io.disconnect();
          delete el.__io;
        }
      }
    }
  },

  data() {
    return {
      activeTag: 'All',
      active: null,
      activeIndex: 0,
      items: [],
      loading: true
    };
  },

  computed: {
    allTags() {
      const set = new Set();
      this.items.forEach(c => c.tags?.forEach(t => set.add(t)));
      return [...set];
    },
    filtered() {
      return this.activeTag === 'All'
        ? this.items
        : this.items.filter(c => c.tags?.includes(this.activeTag));
    }
  },

  mounted() {
    this.fetchAchievements();
  },

  methods: {
    async fetchAchievements() {
      try {
        // เปลี่ยน path ให้ตรงกับที่เก็บไฟล์ db.json ของคุณ
        const response = await fetch('./db.json');
        const data = await response.json();
        
        // ดึงข้อมูล achievements จาก JSON
        this.items = data.achievements || [];
        this.loading = false;
      } catch (error) {
        console.error('Error loading achievements:', error);
        this.loading = false;
      }
    },

    formatDate(d) {
      return new Date(d).toLocaleDateString('en-GB');
    },

    open(c) {
      this.active = c;
      this.activeIndex = 0;
    },

    close() {
      this.active = null;
      this.activeIndex = 0;
    },

    next() {
      if (!this.active.images) return;
      this.activeIndex = (this.activeIndex + 1) % this.active.images.length;
    },

    prev() {
      if (!this.active.images) return;
      this.activeIndex =
        (this.activeIndex - 1 + this.active.images.length) %
        this.active.images.length;
    }
  }
};
</script>

<style scoped>
/* Achievements Section */
.Achievements-section {
  background-color: rgb(242, 204, 123);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 5vw;
}

.cert-title {
  font-size: 3rem;
  margin-bottom: 2rem;
  color: #333;
}

/* Loading */
.loading {
  font-size: 1.5rem;
  color: #666;
  margin: 2rem 0;
}

/* Filters */
.filters {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 2rem;
}

.chip {
  padding: 0.5rem 1.5rem;
  border: 2px solid #333;
  background: transparent;
  border-radius: 25px;
  cursor: pointer;
  font-family: 'Oswald', monospace;
  font-size: 1rem;
  transition: all 0.3s;
}

.chip:hover {
  background: rgba(0, 0, 0, 0.1);
}

.chip.active {
  background: #333;
  color: rgb(242, 204, 123);
}

/* Grid */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
  width: 100%;
  max-width: 1200px;
}

/* Card */
.card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.thumb {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.ribbon {
  position: absolute;
  top: 10px;
  right: -30px;
  background: #4CAF50;
  color: white;
  padding: 5px 40px;
  transform: rotate(45deg);
  font-size: 0.8rem;
}

.multi-badge {
  position: absolute;
  bottom: 10px;
  right: 10px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 5px 10px;
  border-radius: 15px;
  font-size: 0.8rem;
}

/* Meta */
.meta {
  padding: 1.5rem;
}

.ctitle {
  font-size: 1.3rem;
  margin-bottom: 0.5rem;
  color: #333;
}

.issuer {
  color: #666;
  margin-bottom: 1rem;
}

.cert-tags {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-bottom: 1rem;
}

.cert-tag {
  background: rgb(242, 204, 123);
  padding: 0.3rem 0.8rem;
  border-radius: 15px;
  font-size: 0.85rem;
  color: #333;
}

.date {
  color: #999;
  font-size: 0.9rem;
}

/* Lightbox */
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 2rem;
}

.lightbox-inner {
  background: white;
  border-radius: 12px;
  max-width: 1200px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  padding: 2rem;
  display: grid;
  grid-template-columns: minmax(320px, 60vw) minmax(280px, 420px);
  gap: 1.3rem;
}

.viewer {
  position: relative;
  margin-bottom: 2rem;
}

.viewer img {
  width: 100%;
  height: auto;
  border-radius: 8px;
}

.nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 1rem;
  cursor: pointer;
  font-size: 1.5rem;
  border-radius: 4px;
}

.nav:hover {
  background: rgba(0, 0, 0, 0.8);
}

.prev {
  left: 10px;
}

.next {
  right: 10px;
}

.thumbs {
  display: flex;
  gap: 0.5rem;
  margin-top: 1rem;
  overflow-x: auto;
}

.thumb-btn {
  border: 2px solid transparent;
  padding: 0;
  cursor: pointer;
  border-radius: 4px;
  overflow: hidden;
}

.thumb-btn.active {
  border-color: #333;
}

.thumb-btn img {
  width: 120px;
  height: 90px;
  object-fit: cover;
  display: block;
} 

.lb-info {
  position: relative;
  padding: 2rem 1.5rem 3rem;
  margin-top: 50px;
  margin-right: 50px;
  display: flex;
  flex-direction: column;
  gap: .75rem;
}

.lb-info h3 {
  font-size: 1.8rem;
  margin-bottom: 0.5rem;
  font-weight: 600;
  line-height: 1.4;

}

.lb-info .issuer {
  color: #666;
  margin-bottom: 1rem;
}

.lb-info .desc {
  color: #333;
  line-height: 1.6;
  margin-bottom: 1.5rem;
  
}

.close-btn {
  position: absolute;
  right: 14px;
  bottom: 14px;
  z-index: 10;
  background: #333;
  color: white;
  border: none;
  padding: 0.6rem 2rem;
  border-radius: 25px;
  cursor: pointer;
  font-family: 'Oswald', monospace;
  font-size: 1rem;
  transition: filter .2s ease, transform .15s ease;
}

.close-btn:hover {
  background: #555;
}

/* Reveal Animation */
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
  transition-delay: var(--delay, 0ms);
}

.reveal.in-view {
  opacity: 1;
  transform: translateY(0);
}
</style>