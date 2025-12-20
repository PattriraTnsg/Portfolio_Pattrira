<template>
  <section class="skills-section">
    <div class="skills-content">
      <!-- header + switch -->
      <div class="skills-header">
        <div class="header-text">
          <h1 class="title">
            My <span class="highlight">Skills</span>
          </h1>
          <p class="description">
            {{ description }}
          </p>
        </div>

        <!-- switch buttons -->
        <div class="skills-switch">
          <button :class="{ active: activeSection === 'tech' }" @click="activeSection = 'tech'">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="3" width="20" height="14" rx="2" ry="2"></rect>
              <line x1="8" y1="21" x2="16" y2="21"></line>
              <line x1="12" y1="17" x2="12" y2="21"></line>
            </svg>
            Tech Skills
          </button>
          <button :class="{ active: activeSection === 'soft' }" @click="activeSection = 'soft'">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
              <circle cx="9" cy="7" r="4"></circle>
              <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
              <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
            </svg>
            Soft Skills
          </button>
        </div>
      </div>

      <!-- skills wrapper with arrows -->
      <div class="skills-wrapper">

        <!-- previous -->
        <button class="nav-btn left" @click="prevPage" :disabled="currentPage === 1" aria-label="Previous page">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M15 18l-6-6 6-6" stroke="currentColor" stroke-width="2" stroke-linecap="round"
              stroke-linejoin="round" />
          </svg>
        </button>

        <!-- skills grid with transition -->
        <transition-group name="fade-slide" tag="div" class="category-container">
          <div v-for="category in paginatedCategories" :key="category.key" class="category-block">
            <h3 class="category-title">
              {{ category.title }}
            </h3>
            <p v-if="category.description" class="category-desc">
              {{ category.description }}
            </p>

            <!-- tech skills -->
            <div v-if="activeSection === 'tech'" class="skills-grid">
              <div v-for="skill in category.skills" :key="skill.id" class="skill-card">
                <!-- Fixed: Added skill-icon-wrapper -->
                <div class="skill-icon-wrapper">
                  <div class="skill-icon">
                    <img :src="skill.icon || '/icons/default.svg'" :alt="skill.name" />
                  </div>
                  <div class="glow"></div>
                </div>

                <h4 class="skill-name">{{ skill.name }}</h4>

                <!-- Fixed: Added progress-container wrapper -->
                <div class="progress-container">
                  <div class="progress-bar">
                    <div class="progress-fill" :style="{ width: skill.percentage + '%' }">
                      <span class="progress-text">{{ skill.percentage }}%</span>
                    </div>
                  </div>

                  <!-- Fixed: Changed class from 'level' to 'level-badge' -->
                  <span class="level-badge" :class="getLevelClass(skill.percentage)">
                    {{ getLevel(skill.percentage) }}
                  </span>
                </div>
              </div>
            </div>

            <!-- soft skills -->
            <div v-else class="soft-grid">
              <div v-for="skill in category.skills" :key="skill.id" class="soft-card">
                <img :src="skill.icon || '/icons/default.svg'" :alt="skill.name" />
                <span>{{ skill.name }}</span>
              </div>
            </div>
          </div>
        </transition-group>

        <!-- next arrow -->
        <button class="nav-btn right" @click="nextPage" :disabled="currentPage === totalPages" aria-label="Next page">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M9 6l6 6-6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round"
              stroke-linejoin="round" />
          </svg>
        </button>
      </div>

      <!-- pagination dots -->
      <div class="pagination-dots">
        <span v-for="page in totalPages" :key="page" :class="{ active: page === currentPage }"
          @click="currentPage = page" class="dot" :aria-label="`Go to page ${page}`"></span>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const description = ref(
  'My skills reflect my learning journey in technology and personal growth.'
)

const activeSection = ref('tech')

/* pagination */
const currentPage = ref(1)
const itemsPerPage = 1

const techCategories = ref([])
const softSkills = ref([])

const activeCategories = computed(() => {
  if (activeSection.value === 'tech') {
    return techCategories.value
  }
  return [
    {
      key: 'soft',
      title: 'Soft Skills',
      skills: softSkills.value
    }
  ]
})

const totalPages = computed(() =>
  Math.ceil(activeCategories.value.length / itemsPerPage)
)

const paginatedCategories = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  return activeCategories.value.slice(start, start + itemsPerPage)
})

/* helper functions */
const getLevel = (percentage) => {
  if (percentage >= 90) return 'Expert'
  if (percentage >= 75) return 'Advanced'
  if (percentage >= 60) return 'Intermediate'
  return 'Beginner'
}

const getLevelClass = (percentage) => {
  if (percentage >= 90) return 'expert'
  if (percentage >= 75) return 'advanced'
  if (percentage >= 60) return 'intermediate'
  return 'beginner'
}

/* pagination actions */
const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}

/* reset page when switch section */
watch(activeSection, () => {
  currentPage.value = 1
})

onMounted(async () => {
  try {
    const res = await fetch('/db.json')
    const data = await res.json()

    techCategories.value = data.skills.tech
    softSkills.value = data.skills.soft
  } catch (error) {
    console.error('Error loading skills:', error)
  }
})
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.skills-section {
  position: relative;
  min-height: 100vh;
  background: #333;
  padding: 80px 20px;
  overflow: hidden;
}

@keyframes float {

  0%,
  100% {
    transform: translateY(0) rotate(0deg);
  }

  50% {
    transform: translateY(-30px) rotate(180deg);
  }
}

.skills-content {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  margin: 0 auto;
}

.skills-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 2rem;
  margin-bottom: 60px;
}

.header-text {
  flex: 1;
}

.title {
  font-size: 56px;
  font-weight: 700;
  text-align: left;
  color: #fff;
  margin-bottom: 16px;
  letter-spacing: -1px;
  line-height: 1.2;
}

.highlight {
  background: linear-gradient(135deg, rgb(242, 204, 123), rgb(255, 199, 56));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  position: relative;
}

.highlight::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 100%;
  height: 4px;
  background: linear-gradient(90deg, rgb(242, 204, 123), rgb(255, 199, 56));
  border-radius: 2px;
  opacity: 0.6;
}

.description {
  text-align: left;
  color: #b8b8d0;
  font-size: 18px;
  line-height: 1.6;
  max-width: 500px;
}

.skills-switch {
  display: flex;
  gap: 12px;
  background: rgba(255, 255, 255, 0.05);
  padding: 6px;
  border-radius: 50px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: 90px;
}

.skills-switch button {
  padding: 12px 24px;
  border-radius: 50px;
  border: none;
  background: transparent;
  color: #b8b8d0;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  font-weight: 600;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
  white-space: nowrap;
}

.skills-switch button:hover {
  color: #fff;
}

.skills-switch button.active {
  background: linear-gradient(135deg, rgb(242, 204, 123), rgb(255, 199, 56));
  color: #1a1a2e;
  box-shadow: 0 4px 15px rgba(242, 204, 123, 0.4);
}

/* Skills Wrapper with Arrows */
.skills-wrapper {
  display: flex;
  align-items: center;
  gap: 30px;
  margin-bottom: 40px;
}

/* Navigation Arrows */
.nav-btn {
  width: 56px;
  height: 56px;
  min-width: 56px;
  border-radius: 50%;
  background: linear-gradient(135deg, rgb(242, 204, 123), rgb(255, 199, 56));
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  color: #1a1a2e;
  box-shadow: 0 4px 15px rgba(242, 204, 123, 0.3);
}

.nav-btn:hover:not(:disabled) {
  transform: scale(1.1);
  box-shadow: 0 6px 20px rgba(242, 204, 123, 0.5);
}

.nav-btn:active:not(:disabled) {
  transform: scale(0.95);
}

.nav-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
  box-shadow: none;
}

.nav-btn svg {
  width: 24px;
  height: 24px;
}

/* Category Container */
.category-container {
  flex: 1;
  position: relative;
  min-height: 400px;
}

.category-block {
  width: 100%;
}

.category-title {
  color: rgb(242, 204, 123);
  margin-bottom: 8px;
  font-size: 24px;
  font-weight: 600;
}

.category-desc {
  color: #b8b8d0;
  margin-bottom: 24px;
  font-size: 16px;
}

/* Skills Grid */
.skills-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.skill-card {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
  border-radius: 24px;
  padding: 36px 28px;
  text-align: center;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  position: relative;
  overflow: hidden;
}

.skill-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, rgb(242, 204, 123), rgb(255, 199, 56));
  transform: scaleX(0);
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.skill-card:hover {
  transform: translateY(-12px);
  box-shadow: 0 20px 40px rgba(242, 204, 123, 0.2);
  border-color: rgba(242, 204, 123, 0.3);
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.15), rgba(255, 255, 255, 0.08));
}

.skill-card:hover::before {
  transform: scaleX(1);
}

.skill-icon-wrapper {
  position: relative;
  display: inline-block;
  margin-bottom: 24px;
}

.skill-icon {
  width: 72px;
  height: 72px;
  background: linear-gradient(135deg, rgba(242, 204, 123, 0.2), rgba(255, 199, 56, 0.1));
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  transition: all 0.3s ease;
  border: 2px solid rgba(242, 204, 123, 0.2);
}

.skill-card:hover .skill-icon {
  transform: scale(1.1) rotate(5deg);
  box-shadow: 0 10px 30px rgba(242, 204, 123, 0.3);
}

.skill-icon img {
  width: 42px;
  height: 42px;
  filter: drop-shadow(0 2px 8px rgba(242, 204, 123, 0.3));
}

.glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100px;
  height: 100px;
  background: radial-gradient(circle, rgba(242, 204, 123, 0.3), transparent 70%);
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.skill-card:hover .glow {
  opacity: 1;
}

.skill-name {
  color: #fff;
  margin-bottom: 20px;
  font-size: 20px;
  font-weight: 600;
  letter-spacing: -0.5px;
}

.progress-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.progress-bar {
  flex: 1;
  height: 12px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  overflow: hidden;
  position: relative;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, rgb(242, 204, 123), rgb(255, 199, 56));
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-right: 10px;
  transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.progress-fill::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  animation: shimmer 2s infinite;
}

@keyframes shimmer {
  0% {
    transform: translateX(-100%);
  }

  100% {
    transform: translateX(100%);
  }
}

.progress-text {
  font-size: 11px;
  color: #1a1a2e;
  font-weight: 700;
  position: relative;
  z-index: 1;
}

.level-badge {
  font-size: 11px;
  font-weight: 700;
  padding: 4px 10px;
  border-radius: 12px;
  white-space: nowrap;
}

.level-badge.expert {
  background: rgba(76, 175, 80, 0.2);
  color: #4caf50;
  border: 1px solid rgba(76, 175, 80, 0.3);
}

.level-badge.advanced {
  background: rgba(33, 150, 243, 0.2);
  color: #2196f3;
  border: 1px solid rgba(33, 150, 243, 0.3);
}

.level-badge.intermediate {
  background: rgba(255, 152, 0, 0.2);
  color: #ff9800;
  border: 1px solid rgba(255, 152, 0, 0.3);
}

.level-badge.beginner {
  background: rgba(158, 158, 158, 0.2);
  color: #9e9e9e;
  border: 1px solid rgba(158, 158, 158, 0.3);
}

/* Soft Skills Grid */
.soft-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 20px;
}

.soft-card {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
  border-radius: 16px;
  padding: 24px;
  text-align: center;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.soft-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 15px 30px rgba(242, 204, 123, 0.15);
  border-color: rgba(242, 204, 123, 0.3);
}

.soft-card img {
  width: 48px;
  height: 48px;
  filter: drop-shadow(0 2px 8px rgba(242, 204, 123, 0.3));
}

.soft-card span {
  color: #fff;
  font-size: 14px;
  font-weight: 600;
}

/* Transitions */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

/* Pagination Dots */
.pagination-dots {
  display: flex;
  justify-content: center;
  gap: 10px;
  align-items: center;
  margin-top: 20px;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  cursor: pointer;
  transition: all 0.3s ease;
}

.dot.active {
  width: 32px;
  border-radius: 5px;
  background: linear-gradient(90deg, rgb(242, 204, 123), rgb(255, 199, 56));
}

.dot:not(.active):hover {
  background: rgba(255, 255, 255, 0.4);
  transform: scale(1.2);
}

/* Responsive */
@media (max-width: 1024px) {
  .skills-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .skills-header {
    flex-direction: column;
    align-items: stretch;
  }

  .title {
    font-size: 40px;
    text-align: center;
  }

  .description {
    text-align: center;
    margin: 0 auto;
  }

  .skills-switch {
    justify-content: center;
    margin-top: 30px;
  }

  .skills-wrapper {
    gap: 15px;
  }

  .nav-btn {
    width: 48px;
    height: 48px;
    min-width: 48px;
  }

  .nav-btn svg {
    width: 20px;
    height: 20px;
  }

  .skills-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }

  .soft-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .skills-section {
    padding: 60px 15px;
  }

  .title {
    font-size: 32px;
  }

  .description {
    font-size: 16px;
  }

  .skills-wrapper {
    flex-direction: column;
    gap: 20px;
  }

  .nav-btn {
    display: none;
  }

  .category-container {
    min-height: auto;
  }

  .pagination-dots {
    margin-top: 30px;
  }

  .soft-grid {
    grid-template-columns: 1fr;
  }
}
</style>