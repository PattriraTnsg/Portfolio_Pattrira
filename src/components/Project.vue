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

                    <div class="lightbox-media-column">
                        <div class="main-image-wrapper" v-if="active.images?.length">
                            <img :src="active.images[activeImageIndex]" :alt="active.title" class="main-img" />

                            <button class="nav prev" @click.stop="prevImage" v-if="active.images.length > 1">
                                ‹
                            </button>

                            <button class="nav next" @click.stop="nextImage" v-if="active.images.length > 1">
                                ›
                            </button>
                        </div>

                        <div class="thumbnails-container" v-if="active.images.length > 1">
                            <div class="thumbnails">
                                <img v-for="(img, i) in active.images" :key="i" :src="img"
                                    :class="{ active: i === activeImageIndex }" @click="activeImageIndex = i" />
                            </div>
                        </div>
                    </div>

                    <div class="lightbox-info-column">
                        <div class="info-header">
                            <h2>{{ active.title }}</h2>
                            <p class="subtitle" v-if="active.date">{{ active.date }}</p>
                        </div>

                        <div class="info-body">
                            <p>{{ active.description }}</p>

                            <div class="tags">
                                <span v-for="t in active.tech" :key="t">{{ t }}</span>
                            </div>
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
            activeTech: 'All',
            activeImageIndex: 0
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
            this.activeImageIndex = 0; // เริ่มที่รูปแรก
            document.body.style.overflow = 'hidden';
        },
        close() {
            this.active = null;
            document.body.style.overflow = '';
        },
        nextImage() {
            this.activeImageIndex =
                (this.activeImageIndex + 1) % this.active.images.length;
        },
        prevImage() {
            this.activeImageIndex =
                (this.activeImageIndex - 1 + this.active.images.length) %
                this.active.images.length;
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
    background: #1a1a1a;
    /* สี card */
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
    display: flex;          
    flex-direction: column;
    height: 100%;
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
    display: flex;
    flex-direction: column;
    flex-grow: 1;
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
/* --- Lightbox Container --- */
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
    from { opacity: 0; }
    to { opacity: 1; }
}

/* --- Lightbox Inner Box --- */
.lightbox-inner {
    background: #1a1a1a; 
    border-radius: 24px;
    max-width: 1200px; /* 💡 ปรับให้พอดีขึ้น ไม่กว้างเกินไป */
    width: 100%;
    max-height: 90vh; 
    position: relative;
    border: 2px solid rgba(242, 204, 123, 0.3);
    display: flex;
    flex-direction: column;
    overflow: hidden; /* 💡 ป้องกันเนื้อหาทะลุกรอบ */
}

/* --- Layout ฝั่งซ้าย (รูป) ขวา (ข้อมูล) สำหรับ Desktop --- */
.lightbox-content {
    display: grid;
    grid-template-columns: 1.5fr 1fr; /* 💡 ปรับสัดส่วนให้เนื้อหาอ่านง่ายขึ้น */
    height: 100%; /* 💡 ให้เต็มความสูงของ lightbox-inner */
}

/* --- ฝั่งซ้าย: รูปภาพ --- */
.lightbox-media-column {
    background: #141414;
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
    height: 100%;
}

.main-image-wrapper {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
    padding: 2rem;
}

.main-img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
    border-radius: 8px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}

/* --- ปุ่มเลื่อนรูปภาพ --- */
.nav {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(38, 36, 32, 0.5);
    color: #f2cc7b;
    border: 1px solid rgba(242, 204, 123, 0.3);
    border-radius: 50%;
    width: 48px;
    height: 48px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s;
    z-index: 10;
}
.nav:hover {
    background: #f2cc7b;
    color: #1a1a1a;
}
.nav.prev { left: 20px; }
.nav.next { right: 20px; }

/* --- รูปเล็ก (Thumbnails) ด้านล่าง --- */
.thumbnails-container {
    padding: 1rem;
    background: rgba(255, 255, 255, 0.02);
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    overflow-x: auto; /* 💡 เผื่อรูปเยอะจะได้เลื่อนซ้ายขวาได้ */
}

/* 💡 ซ่อน Scrollbar ของ Thumbnail แต่ยังเลื่อนได้ */
.thumbnails-container::-webkit-scrollbar { height: 6px; }
.thumbnails-container::-webkit-scrollbar-track { background: transparent; }
.thumbnails-container::-webkit-scrollbar-thumb { background: rgba(242, 204, 123, 0.3); border-radius: 4px; }

.thumbnails {
    display: flex;
    justify-content: center;
    gap: 10px;
    min-width: max-content; /* 💡 ป้องกัน thumbnail โดนบีบ */
}
.thumbnails img {
    width: 60px;
    height: 40px;
    object-fit: cover;
    border-radius: 4px;
    opacity: 0.5;
    cursor: pointer;
    border: 2px solid transparent;
    transition: all 0.2s;
    flex-shrink: 0;
}
.thumbnails img.active {
    opacity: 1;
    border-color: #f2cc7b;
    transform: scale(1.1);
}

/* --- ฝั่งขวา: ข้อมูล (Info Column) --- */
.lightbox-info-column {
    padding: 3rem 2.5rem;
    display: flex;
    flex-direction: column;
    overflow-y: auto; /* 💡 ให้ scroll ได้เฉพาะฝั่งข้อมูล */
    background: #1a1a1a;
    position: relative;
    max-height: 90vh; /* 💡 ป้องกันยาวเกินจอ */
}

/* Scrollbar สำหรับฝั่งข้อมูล */
.lightbox-info-column::-webkit-scrollbar { width: 8px; }
.lightbox-info-column::-webkit-scrollbar-track { background: #1a1a1a; }
.lightbox-info-column::-webkit-scrollbar-thumb { background: rgba(242, 204, 123, 0.5); border-radius: 4px; }
.lightbox-info-column::-webkit-scrollbar-thumb:hover { background: #f2cc7b; }

.info-header h2 {
    font-size: 2.2rem;
    color: #f2cc7b;
    margin-bottom: 0.5rem;
    line-height: 1.2;
    padding-right: 2rem; /* 💡 เผื่อที่ให้ปุ่ม Close */
}

.subtitle {
    color: #888;
    font-size: 0.9rem;
    margin-bottom: 2rem;
    font-weight: 500;
}

.info-body p {
    color: #ccc;
    font-size: 1rem;
    line-height: 1.8;
    margin-bottom: 2rem;
}

.lightbox-info-column .tags {
    margin-bottom: auto; /* ดันปุ่ม action ลงไปล่างสุด (ถ้ามีพื้นที่เหลือ) */
}

.actions {
    margin-top: 2rem;
    padding-top: 2rem;
    border-top: 1px solid rgba(255,255,255,0.1);
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
    flex: 1; /* 💡 ให้ปุ่มกว้างเท่าๆ กัน */
    justify-content: center;
}

.btn:hover {
    background: #ffd98f;
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(242, 204, 123, 0.3);
}

.btn.ghost {
    background: transparent;
    color: #f2cc7b;
}

.btn.ghost:hover {
    background: rgba(242, 204, 123, 0.1);
    transform: translateY(-2px);
}

/* --- ปุ่มปิด (Close) --- */
.close {
    position: absolute;
    top: 20px;
    right: 20px;
    z-index: 100;
    background: rgba(26, 26, 26, 0.8); /* 💡 เพิ่มพื้นหลังให้เห็นชัดขึ้น */
    border: none;
    color: #f2cc7b;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s;
}
.close:hover {
    background: #f2cc7b;
    color: #1a1a1a;
    transform: rotate(90deg);
}

/* =========================================
   📱 Responsive Design สำหรับมือถือและแท็บเล็ต
   ========================================= */

@media (max-width: 1024px) {
    .lightbox-content {
        grid-template-columns: 1fr 1fr;
    }
}

@media (max-width: 768px) {
    .lightbox {
        padding: 1rem;
        align-items: center; /* 💡 จัดให้อยู่ตรงกลางจอ */
    }
    
    .lightbox-inner {
        max-height: 90vh; /* 💡 จำกัดความสูงไม่ให้เกิน 90% ของจอ */
        border-radius: 16px;
        display: flex;
        flex-direction: column;
        overflow: hidden; /* 💡 ป้องกันไม่ให้เนื้อหาทะลุกรอบ */
    }

    /* 💡 เปลี่ยนจาก Grid เป็น Flex เรียงแนวตั้ง และให้ Scroll ที่นี่จุดเดียว */
    .lightbox-content {
        display: flex;
        flex-direction: column;
        overflow-y: auto; 
        height: 100%;
        -webkit-overflow-scrolling: touch; /* ให้มือถือเลื่อนได้สมูทขึ้น */
    }

    .lightbox-media-column {
        height: auto;
        min-height: 35vh; /* 💡 ให้รูปมีพื้นที่กำลังดี ไม่ใหญ่/เล็กไป */
        width: 100%;
        flex-shrink: 0;
        border-radius: 16px 16px 0 0;
    }

    .main-image-wrapper {
        padding: 1.5rem 1rem 0.5rem 1rem;
        height: 100%;
        display: flex;
        align-items: center;
    }

    .main-img {
        max-height: 35vh;
        width: 100%;
        object-fit: contain;
    }

    /* 💡 ย่อปุ่มเลื่อนรูปในมือถือ */
    .nav { width: 36px; height: 36px; }
    .nav.prev { left: 10px; }
    .nav.next { right: 10px; }

    /* 💡 ปรับฝั่งข้อมูลให้ความสูงยืดหยุ่น */
    .lightbox-info-column {
        padding: 1.5rem;
        overflow-y: visible; /* 💡 ปิด scroll ซ้ำซ้อน ให้ไป scroll ที่ตัวแม่แทน */
        height: auto;
    }

    .info-header h2 {
        font-size: 1.6rem; /* 💡 ปรับขนาดฟอนต์ให้พอดีจอมือถือ */
        padding-right: 2.5rem; /* เผื่อพื้นที่ให้ปุ่มกากบาทตอนเลื่อนลงมา */
    }

    .info-body p {
        font-size: 0.95rem;
        margin-bottom: 1.5rem;
    }

    /* 💡 ให้ปุ่ม Action เรียงแนวตั้งบนมือถือ */
    .actions {
        flex-direction: column;
        gap: 0.8rem;
        margin-top: 1.5rem;
        padding-top: 1.5rem;
    }
    
    .btn {
        width: 100%;
    }
    
    /* 💡 ปุ่มปิดให้อยู่ตำแหน่งขวาบนของกรอบเสมอ */
    .close {
        position: absolute;
        top: 10px;
        right: 10px;
        background: rgba(0, 0, 0, 0.7);
        width: 36px;
        height: 36px;
        z-index: 100;
    }
}
</style>