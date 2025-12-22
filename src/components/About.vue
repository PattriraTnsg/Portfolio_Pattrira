<template>
<!-- About Section -->
<section class="About-section pt-5" style="font-family: 'Oswald', monospace">
  <div class="student-card">
    <div class="student-info">
      <h2 class="title">About me</h2>
      <div class="profile-img-wrapper">
        <img :src="about_pic" alt="me" class="profile_img" />      
        <div class="profile-overlay"></div>
      </div>   
      <div class="student-detail-container">
        <div class="student-detail">
          <span class="detail-icon">👤</span>
          <span class="detail-label">Name : </span>
          <span class="detail-value">{{ student.name }}</span>
        </div>
        <div class="student-detail">
          <span class="detail-icon">✨</span>
          <span class="detail-label">Nickname : </span>
          <span class="detail-value">{{ student.nickName }}</span>
        </div>
        <div class="student-detail">
          <span class="detail-icon">🎓</span>
          <span class="detail-label">Major : </span>
          <span class="detail-value">{{ student.major }}</span>
        </div>
        <div class="student-detail">
          <span class="detail-icon">🏛️</span>
          <span class="detail-label">University : </span>
          <span class="detail-value">{{ student.University }}</span>
        </div>
        <div class="student-detail">
          <span class="detail-icon">📚</span>
          <span class="detail-label">GPX: </span>
          <span class="detail-value">{{ student.gpx }}</span>
        </div>
      </div>
    </div>
  </div>
  
  <div class="intro-content">
    <div class="quote">
      <h2>CTRL + ALT + DEL</h2>
      <h4>
        Control yourself<br/>
        Alter your thinking<br/>
        Delete negativity
      </h4>
    </div>
  </div>
</section>
</template>

<script>
import about_pic from "@/assets/image/me2.jpg";

export default {
  name: "AboutSection",
  data() {
    return {
      about_pic,
      student: {
        name: "Pattrira Tanongsaksrikun",
        nickName: "Anfield",
        major: "Computer Science",
        University: "Kasetsart University Sriracha Campus",
        gpx: 3.92,
      },
    };
  },
  mounted() {
    fetch(`http://localhost:3000/student`)
      .then(response => {
        if (!response.ok) {
          throw new Error("Failed to fetch student data");
        }
        return response.json();
      })
      .then(data => {
        this.student = data;
      })
      .catch(error => {
        console.error(error);
      });
  }
};
</script>

<style>

.About-section {
  background: #333;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 5rem 5vw;
  gap: 4rem;
  position: relative;
  overflow: hidden;
}

.About-section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    radial-gradient(circle at 20% 50%, rgba(252, 202, 75, 0.1) 0%, transparent 50%),
    radial-gradient(circle at 80% 50%, rgba(252, 202, 75, 0.08) 0%, transparent 50%);
  pointer-events: none;
}

.student-card {
  flex: 1;
  max-width: 520px;
  background: #333;
  border-radius: 1.5rem;
  padding: 2rem;
  box-shadow: 
    0 20px 60px rgba(0, 0, 0, 0.3),
    0 0 0 1px rgba(255, 255, 255, 0.1);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  z-index: 1;
}

.student-card::before {
  content: '';
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: rgb(242, 204, 123);
  border-radius: 1.5rem;
  opacity: 0;
  transition: opacity 0.4s ease;
  z-index: -1;
}

.student-card:hover {
  transform: translateY(-5px);
}

.student-card:hover::before {
  opacity: 1;
}

.student-card:hover .title {
  color: #333;
}

.student-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.student-info .title {
  font-size: clamp(1.8rem, 4vw, 2.5rem);
  font-weight: 700;
  margin-bottom: 1.5rem;
  color: rgb(242, 204, 123);
  letter-spacing: 2px;
  transition: color 0.4s ease;
}

.title {
  font-size: clamp(1.8rem, 4vw, 2.5rem);
  font-weight: 700;
  margin-bottom: 2rem;
  color: rgb(242, 204, 123);
  letter-spacing: 2px;
  transition: color 0.4s ease;
}

.profile_img {
  width: clamp(8rem, 20vw, 12rem);
  height: clamp(8rem, 20vw, 12rem);
  border-radius: 1rem;
  object-fit: cover;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.25);
  margin-bottom: 2rem;
  transition: transform 0.3s ease;
}

.profile_img:hover {
  transform: scale(1.05);
}

/* ===============================================
   STUDENT DETAILS
   =============================================== */

.student-detail-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-top: 1rem;
}

.student-detail {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  text-align: left;
  padding: 0.75rem 1.5rem;
  background-color: #f8f8f8;
  border-radius: 0.5rem;
  border-left: 4px solid rgb(252, 202, 75);
  font-size: clamp(0.85rem, 2vw, 1rem);
  color: #333;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  word-break: break-word;
}

.student-detail:hover {
  border-left: 4px solid rgb(35, 34, 32);
  transform: translateX(5px);
}

.detail-icon {
  font-size: clamp(1.1rem, 2.5vw, 1.3rem);
  flex-shrink: 0;
}

.detail-label {
  font-weight: 600;
  margin-right: 0.5rem;
  white-space: nowrap;
}

.intro-content {
  flex: 1;
  max-width: 500px;
  color: #ebebeb;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 2rem;
}

.quote {
  position: relative;
  padding: 2.5rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 1.5rem;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(252, 202, 75, 0.2);
  box-shadow: 0 8px 32px rgba(252, 202, 75, 0.15);
  transition: all 0.3s ease;
}

.quote:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 48px rgba(252, 202, 75, 0.25);
}

.quote h2 {
  font-size: clamp(1.5rem, 3.5vw, 2rem);
  line-height: 1.6;
  font-weight: bold;
  margin-bottom: 2rem;
  color: rgb(242, 204, 123);
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: 2px;
}

.quote h4 {
  font-size: clamp(1.1rem, 2.5vw, 1.5rem);
  line-height: 1.6;
  color: #ebebeb;
  font-weight: 400;
  padding-left: 1.5rem;
  border-left: 4px solid rgb(242, 204, 123);
}

/* Tablet */
@media (max-width: 1024px) {
  .About-section {
    flex-direction: column;
    padding: 4rem 5vw;
    gap: 3rem;
  }

  .student-card,
  .intro-content {
    max-width: 100%;
  }

  .quote {
    padding: 2rem;
  }

  .quote h2 {
    letter-spacing: 1.5px;
  }
}

/* Tablet Portrait */
@media (max-width: 768px) {
  .About-section {
    padding: 4rem 5vw;
    gap: 2.5rem;
    min-height: 100svh;
  }

  .student-card {
    padding: 1.75rem;
    border-radius: 1.25rem;
  }

  .title {
    font-size: 1.8rem;
    letter-spacing: 1.5px;
    margin-bottom: 1.5rem;
  }

  .student-info .title {
    margin-bottom: 1.25rem;
  }

  .profile_img {
    width: 10rem;
    height: 10rem;
    margin-bottom: 1.5rem;
  }

  .student-detail {
    padding: 0.625rem 1.25rem;
    gap: 0.625rem;
  }

  .intro-content {
    padding: 1.5rem;
  }

  .quote {
    padding: 1.75rem;
  }

  .quote h2 {
    margin-bottom: 1.5rem;
    letter-spacing: 1px;
  }

  .quote h4 {
    padding-left: 1.25rem;
    line-height: 1.5;
  }
}

/* Mobile Large */
@media (max-width: 600px) {
  .About-section {
    padding: 3rem 4vw;
    gap: 2rem;
  }

  .student-card {
    padding: 1.5rem;
    border-radius: 1rem;
  }

  .student-card:hover {
    transform: translateY(-3px);
  }

  .title {
    font-size: 1.6rem;
    letter-spacing: 1px;
  }

  .profile_img {
    width: 9rem;
    height: 9rem;
  }

  .profile_img:hover {
    transform: scale(1.02);
  }

  .student-detail {
    padding: 0.625rem 1rem;
    font-size: 0.9rem;
    border-left-width: 3px;
  }

  .detail-icon {
    font-size: 1.1rem;
  }

  .intro-content {
    padding: 1rem;
  }

  .quote {
    padding: 1.5rem;
  }

  .quote:hover {
    transform: translateY(-3px);
  }

  .quote h2 {
    font-size: 1.4rem;
    margin-bottom: 1.25rem;
    text-align: center;
    letter-spacing: 0.5px;
  }

  .quote h4 {
    font-size: 1.1rem;
    padding-left: 1rem;
    border-left-width: 3px;
  }
}

/* Mobile Small */
@media (max-width: 480px) {
  .About-section {
    padding: 2.5rem 5vw;
    gap: 1.75rem;
  }

  .student-card {
    padding: 1.25rem;
  }

  .title {
    font-size: 1.5rem;
  }

  .profile_img {
    width: 8rem;
    height: 8rem;
    margin-bottom: 1.25rem;
  }

  .student-detail {
    padding: 0.5rem 0.875rem;
    font-size: 0.85rem;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

  .detail-icon {
    font-size: 1rem;
  }

  .detail-label {
    font-size: 0.85rem;
  }

  .intro-content {
    padding: 0.75rem;
  }

  .quote {
    padding: 1.25rem;
    border-radius: 1rem;
  }

  .quote h2 {
    font-size: 1.3rem;
    margin-bottom: 1rem;
  }

  .quote h4 {
    font-size: 1rem;
    line-height: 1.5;
    padding-left: 0.875rem;
  }
}
  
/* Landscape Mode สำหรับ Mobile */
@media (max-height: 600px) and (orientation: landscape) {
  .About-section {
    padding: 2rem 5vw;
    min-height: 100vh;
  }

  .profile_img {
    width: 7rem;
    height: 7rem;
    margin-bottom: 1rem;
  }

  .title {
    margin-bottom: 1rem;
  }

  .student-detail {
    padding: 0.5rem 1rem;
  }

  .quote {
    padding: 1.25rem;
  }

  .quote h2 {
    margin-bottom: 1rem;
  }
}

/* Reduce Motion */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
  
  .student-card:hover,
  .quote:hover,
  .student-detail:hover {
    transform: none;
  }
}


</style>