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
      </div>
    </div>
  </div>
  
  <div class="intro-content">
    <div class="quote">
      <!-- คำคมให้กำลังใจ -->
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
  background: linear-gradient(135deg, #3a3938 0%, #464544 50%, #525150 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: left;
  padding: 5vw;
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
  padding: 0.5rem 1.5rem;
  background-color: #f8f8f8;
  border-radius: 0.5rem;
  border-left: 4px solid rgb(252, 202, 75);
  font-size: 1rem;
  color: #333;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.student-detail:hover {
  border-left: 4px solid rgb(35, 34, 32);
  transform: translateX(5px);
}

.detail-icon {
  font-size: 1.3rem;
  flex-shrink: 0;
}

.detail-label {
  font-weight: 600;
  margin-right: 0.5rem;
}

.student-card {
  flex: 1;
  max-width: 520px;
  background: linear-gradient(135deg, #ffffff 0%, #f8f8f8 100%);
  border-radius: 1.5rem;
  padding: 2rem;
  margin-top: 4rem;
  margin-left: 20rem;
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
  background: linear-gradient(135deg, rgb(244, 202, 95), rgb(247, 202, 88));
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

.student-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.student-info .title {
  margin-top: 1rem;
}

.title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 2rem;
  color: #333;
  letter-spacing: 2px;
}

.profile_img {
  width: 12rem;
  height: 12rem;
  border-radius: 1rem;
  object-fit: cover;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.25);
  margin-bottom: 2rem;
  transition: transform 0.3s ease;
}

.profile_img:hover {
  transform: scale(1.05);
}

.intro-content {
  flex: 1;
  max-width: 500px;
  color: #ebebeb;
  font-size: 60px;
  font-weight: bold;
  position: absolute;
  right: 5rem;
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
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  margin-right: 5rem;
  box-shadow: 0 5px 10px rgba(252, 202, 75);
}

.quote:hover {
  transform: translateY(5px);
}

.quote h2 {
  font-size: 2rem;
  line-height: 1.6;
  font-weight: bold;
  margin-bottom: 2rem;
  color: rgb(242, 204, 123);
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: 3px;
}

.quote h4 {
  font-size: 1.5rem;
  line-height: 1.4;
  color: #ebebeb;
  font-weight: 400;
  padding-left: 1.5rem;
  border-left: 4px solid rgb(242, 204, 123);
}

/* Responsive Design */
@media (max-width: 1024px) {
  .About-section {
    flex-direction: column;
    padding: 3rem 2rem;
  }

  .student-card,
  .intro-content {
    max-width: 100%;
    width: 100%;
  }

  .quote h2 {
    font-size: 2.5rem;
  }

  .quote h4 {
    font-size: 1.5rem;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 1.5rem;
  }

  .profile_img {
    width: 8rem;
    height: 8rem;
  }

  .quote h2 {
    font-size: 1.5rem;
  }

  .quote h4 {
    font-size: 1.1rem;
  }
}
</style>