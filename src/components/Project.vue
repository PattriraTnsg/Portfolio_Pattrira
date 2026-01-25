<template>
    <section class="projects-section" id="projects">
        <div class="projects-wrapper">
            <h1 class="section-title" v-reveal>Projects</h1>

            <!-- Filters -->
            <div class="filters" v-if="!loading">
                <button v-for="t in ['All', ...allTech]" :key="t" class="chip" :class="{ active: activeTech === t }"
                    @click="activeTech = t">
                    {{ t }}
                </button>
            </div>
            <!-- Grid -->
            <div class="grid" v-if="!loading">
                <article class="card" v-for="(p, i) in filtered" :key="i" v-reveal
                    :style="{ '--delay': (i * 80) + 'ms' }" @click="open(p)">
                    <div class="card-image">
                        <img :src="p.cover" :alt="p.title" />
                        <div class="overlay">
                            <span class="view-text">View Details</span>
                        </div>
                    </div>
                    <div class="card-body">
                        <h3>{{ p.title }}</h3>
                        <p>{{ p.description }}</p>

                        <div class="tags">
                            <span v-for="t in p.tech" :key="t">{{ t }}</span>
                        </div>
                    </div>
                </article>
            </div>
        </div>


        <!-- Lightbox -->
        <div v-if="active" class="lightbox" @click.self="close">
            <div class="lightbox-inner">
                <button class="close" @click="close">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                    </svg>
                </button>

                <div class="lightbox-content">
                    <div class="lightbox-image">
                        <img :src="active.cover" :alt="active.title" />
                    </div>

                    <div class="info">
                        <h2>{{ active.title }}</h2>
                        <p>{{ active.description }}</p>

                        <div class="tags">
                            <span v-for="t in active.tech" :key="t">{{ t }}</span>
                        </div>

                        <div class="actions">
                            <a v-if="active.demo" :href="active.demo" target="_blank" class="btn">
                                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                                    stroke-width="2">
                                    <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path>
                                    <polyline points="15 3 21 3 21 9"></polyline>
                                    <line x1="10" y1="14" x2="21" y2="3"></line>
                                </svg>
                                Live Demo
                            </a>
                            <a v-if="active.github" :href="active.github" target="_blank" class="btn ghost">
                                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                                    stroke-width="2">
                                    <path
                                        d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22">
                                    </path>
                                </svg>
                                GitHub
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: 'Projects',

    data() {
        return {
            items: [],
            loading: true,
            active: null,
            activeTech: 'All'
        };
    },

    computed: {
        allTech() {
            const set = new Set();
            this.items.forEach(p => p.tech?.forEach(t => set.add(t)));
            return [...set];
        },
        filtered() {
            return this.activeTech === 'All'
                ? this.items
                : this.items.filter(p => p.tech.includes(this.activeTech));
        }
    },

    mounted() {
        this.fetchProjects();
    },

    methods: {
        async fetchProjects() {
            try {
                const res = await fetch('./db.json');
                const data = await res.json();
                this.items = data.projects || [];
            } catch (error) {
                console.error('Error fetching projects:', error);
                this.items = [];
            } finally {
                this.loading = false;
            }
        },
        open(p) {
            this.active = p;
            document.body.style.overflow = 'hidden';
        },
        close() {
            this.active = null;
            document.body.style.overflow = '';
        }
    }
};
</script>

<style scoped>
.projects-section {
    min-height: 100vh;
    padding: 5rem 5vw;
    background: rgb(242, 204, 123);
    display: flex;
   justify-content: center;
   align-items: center;
}

.projects-wrapper {
  width: 100%;
  max-width: 1500px;
  background: #1a1a1a; /* สี card */
  border-radius: 32px;
  padding: 4rem 3rem;
  box-shadow:
    0 30px 80px rgba(0, 0, 0, 0.45),
    inset 0 0 0 2px rgba(242, 204, 123, 0.25);
  position: relative;
}


.section-title {
    font-size: clamp(2.5rem, 5vw, 4rem);
    text-align: center;
    margin-bottom: 3rem;
    background: linear-gradient(135deg, #f2cc7b 0%, #ffd98f 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    font-weight: 700;
    letter-spacing: -0.02em;
}

/* Filters */
.filters {
    display: flex;
    flex-wrap: wrap;
    gap: 0.8rem;
    justify-content: center;
    margin-bottom: 3rem;
    padding: 0 1rem;
}

@media (max-width: 768px) {
  .projects-wrapper {
    padding: 2.5rem 1.5rem;
    border-radius: 20px;
  }

  .projects-section {
    padding: 2rem 1rem;
  }
}


.chip {
    padding: 0.6rem 1.4rem;
    border: 2px solid #333;
    background: transparent;
    color: #fff;
    border-radius: 24px;
    cursor: pointer;
    font-size: 0.95rem;
    font-weight: 500;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
    overflow: hidden;
}

.chip::before {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 0;
    height: 0;
    border-radius: 50%;
    background: rgba(242, 204, 123, 0.2);
    transform: translate(-50%, -50%);
    transition: width 0.4s, height 0.4s;
}

.chip:hover::before {
    width: 300px;
    height: 300px;
}

.chip:hover {
    border-color: #f2cc7b;
    transform: translateY(-2px);
}

.chip.active {
    background: #f2cc7b;
    border-color: #f2cc7b;
    color: #333;
}

/* Grid */
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 2rem;
    margin: 0 auto;
    max-width: 1400px;
}

/* Card */
.card {
    background: #333;
    border-radius: 16px;
    overflow: hidden;
    cursor: pointer;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    border: 2px solid transparent;
    position: relative;
}

.card::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: 16px;
    padding: 2px;
    background: linear-gradient(135deg, transparent, rgba(242, 204, 123, 0.3));
    -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
    opacity: 0;
    transition: opacity 0.4s;
}

.card:hover {
    transform: translateY(-8px);
    border-color: rgba(242, 204, 123, 0.5);
}

.card:hover::before {
    opacity: 1;
}

.card-image {
    position: relative;
    overflow: hidden;
    height: 200px;
}

.card-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.card:hover .card-image img {
    transform: scale(1.1);
}

.overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(242, 204, 123, 0.9), rgba(242, 204, 123, 0.7));
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity 0.3s;
}

.card:hover .overlay {
    opacity: 1;
}

.view-text {
    color: #333;
    font-weight: 700;
    font-size: 1.1rem;
    letter-spacing: 0.5px;
    text-transform: uppercase;
}

.card-body {
    padding: 1.5rem;
}

.card-body h3 {
    font-size: 1.4rem;
    margin-bottom: 0.8rem;
    color: #f2cc7b;
    font-weight: 600;
}

.card-body p {
    font-size: 0.95rem;
    line-height: 1.6;
    color: #ddd;
    margin-bottom: 1rem;
}

.tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 1rem;
}

.tags span {
    background: rgba(242, 204, 123, 0.15);
    color: #f2cc7b;
    padding: 0.4rem 0.9rem;
    border-radius: 16px;
    font-size: 0.85rem;
    font-weight: 500;
    border: 1px solid rgba(242, 204, 123, 0.3);
    transition: all 0.3s;
}

.card:hover .tags span {
    background: rgba(242, 204, 123, 0.25);
    border-color: rgba(242, 204, 123, 0.5);
}

/* Lightbox */
.lightbox {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.95);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    padding: 2rem;
    animation: fadeIn 0.3s ease;
    backdrop-filter: blur(10px);
}

@keyframes fadeIn {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}

.lightbox-inner {
    background: #333;
    border-radius: 24px;
    max-width: 900px;
    width: 100%;
    max-height: 90vh;
    overflow-y: auto;
    position: relative;
    animation: slideUp 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    border: 2px solid rgba(242, 204, 123, 0.3);
}

@keyframes slideUp {
    from {
        transform: translateY(50px);
        opacity: 0;
    }

    to {
        transform: translateY(0);
        opacity: 1;
    }
}

.lightbox-content {
    display: grid;
    grid-template-columns: 1fr;
    gap: 2rem;
}

.lightbox-image {
    width: 100%;
    border-radius: 24px 24px 0 0;
    overflow: hidden;
}

.lightbox-image img {
    width: 100%;
    height: auto;
    max-height: 400px;
    object-fit: cover;
}

.info {
    padding: 2rem;
    padding-top: 0;
}

.info h2 {
    font-size: 2rem;
    margin-bottom: 1rem;
    color: #f2cc7b;
    font-weight: 700;
}

.info p {
    font-size: 1.05rem;
    line-height: 1.7;
    color: #ddd;
    margin-bottom: 1.5rem;
}

.info .tags {
    margin-bottom: 2rem;
}

.actions {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
}

.btn {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.9rem 1.8rem;
    background: #f2cc7b;
    color: #333;
    text-decoration: none;
    border-radius: 12px;
    font-weight: 600;
    font-size: 1rem;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    border: 2px solid #f2cc7b;
}

.btn:hover {
    background: #ffd98f;
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(242, 204, 123, 0.3);
}

.btn.ghost {
    background: transparent;
    color: #f2cc7b;
    border: 2px solid #f2cc7b;
}

.btn.ghost:hover {
    background: rgba(242, 204, 123, 0.1);
    transform: translateY(-2px);
}

.close {
    position: absolute;
    top: 1.5rem;
    right: 1.5rem;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(242, 204, 123, 0.2);
    border: 2px solid #f2cc7b;
    color: #f2cc7b;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s;
    z-index: 10;
}

.close:hover {
    background: #f2cc7b;
    color: #333;
    transform: rotate(90deg);
}

/* Responsive */
@media (max-width: 768px) {
    .projects-section {
        padding: 3rem 4vw;
    }

    .grid {
        grid-template-columns: 1fr;
        gap: 1.5rem;
    }

    .lightbox {
        padding: 1rem;
    }

    .info {
        padding: 1.5rem;
    }

    .info h2 {
        font-size: 1.5rem;
    }

    .actions {
        flex-direction: column;
    }

    .btn {
        width: 100%;
        justify-content: center;
    }
}

/* Scrollbar */
.lightbox-inner::-webkit-scrollbar {
    width: 8px;
}

.lightbox-inner::-webkit-scrollbar-track {
    background: #1a1a1a;
}

.lightbox-inner::-webkit-scrollbar-thumb {
    background: #f2cc7b;
    border-radius: 4px;
}

.lightbox-inner::-webkit-scrollbar-thumb:hover {
    background: #ffd98f;
}
</style>