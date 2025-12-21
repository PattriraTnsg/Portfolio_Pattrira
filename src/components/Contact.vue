<template>
  <!-- Contact Section -->
  <section class="contact-section">
    <div class="contact-container">
      <h1 class="title">Contact</h1>
      <div class="contact-card">
        <img :src="contact_pic" alt="me" class="contact_img" />
        <div class="contact-info">
          <h4>
            Email : pattrira.tnsg@gmail.com
            <button class="copy-btn" @click="copyText('pattrira.tnsg@gmail.com')">⧉</button>
            <br />

            Tel. : 097-110-9214 <br>
            <!-- Social Links -->
            <div class="social-links">
              <!-- LinkedIn -->
              <a href="https://www.linkedin.com/in/pattrira-tanongsaksrikun-01b223398/" class="social-icon">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                  <path
                    d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z" />
                </svg>
              </a>
              <!-- Github -->
              <a href="https://github.com/PattriraTnsg" class="social-icon">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                  <path
                    d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z" />
                </svg>
              </a>
              <!-- IG -->
              <a href="https://www.instagram.com/pt_11.3_npc?igsh=ZHR1cm9pdjMxMTN2" class="social-icon">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                  <path
                    d="M7 2C4.243 2 2 4.243 2 7v10c0 2.757 2.243 5 5 5h10c2.757 0 5-2.243 5-5V7c0-2.757-2.243-5-5-5H7zm10 2c1.654 0 3 1.346 3 3v10c0 1.654-1.346 3-3 3H7c-1.654 0-3-1.346-3-3V7c0-1.654 1.346-3 3-3h10zm-5 3c-2.757 0-5 2.243-5 5s2.243 5 5 5 5-2.243 5-5-2.243-5-5-5zm0 2c1.654 0 3 1.346 3 3s-1.346 3-3 3-3-1.346-3-3 1.346-3 3-3zm4.5-3a1.5 1.5 0 110 3 1.5 1.5 0 010-3z" />
                </svg>
              </a>
            </div>
          </h4>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import contact_pic from "@/assets/image/me_nisit.jpg";
export default {
  name: "Contact",
  data() {
    return {
      contact_pic
    }
  },
  methods:  {
    copyText(text) {
      navigator.clipboard.writeText(text);
      alert("Copied: " + text);
    },
  },
};
</script>

<style scoped>
.contact-section {
  background: linear-gradient(
    135deg,
    rgb(242, 204, 123) 0%,      /* เหลือง */
    rgb(242, 204, 123) 50%,
    #333 50%,                  /* เทา */
    #333 100%
  );
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 5vw;
  font-family: "Oswald", sans-serif;
}

.title {
  font-size: 3rem;
  margin-bottom: 1.5rem;
  color: #333;
  text-align: center;
}

.contact-card {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f9f9f9;
  padding: 3rem;
  border-radius: 20px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
  max-width: 700px;
  width: 150%;
  text-align: center;
  transition: transform 0.3s ease-in-out;
}

.contact-card:hover {
  transform: scale(1.05);
}

.copy-btn {
  margin-left: 5px;
  border: none;
  background: #eaeaea;
  padding: 0.5px 8px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: box-shadow 0.3s ease-in-out;
}

.copy-btn:hover {
  background: #444;
  color: #eaeaea;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3)
}


.contact-info {
  text-align: left;
}

.contact-info h4 {
  font-size: 1.2rem;
  color: #444;
  margin: 0.8rem 0;
  line-height: 2rem;
}

.contact_img {
  width: 200px;
  height: 200px;
  object-fit: cover;
  /* ทำให้ภาพไม่บิด */
  display: block;
  /* ป้องกัน spacing แปลก ๆ */
  border-radius: 50%;
  /* ทำให้เป็นวงกลมเนียน */
  margin-right: 3rem;
  transition: box-shadow 0.3s ease-in-out;
}

.contact_img:hover {
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3);
}

/* Responsive Design */
@media (max-width: 768px) {
  .title {
    font-size: 2.5rem;
  }

  .contact-card {
    padding: 2rem;
    max-width: 90%;
  }

  .contact-info h4 {
    font-size: 1rem;
    padding: 2rem;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 2rem;
  }

  .contact-card {
    padding: 1.5rem;
  }

  .contact-info h4 {
    font-size: 0.9rem;
  }
}


/* Social Links */
.social-links {
  display: flex;
  gap: 1rem;
  animation: fadeIn 1s ease-out 1s backwards;
  padding: 1rem;
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
}

.social-icon:hover {
  background: #f8e0ad;
  color: #444a56;
  transform: translateY(-5px);
}
</style>
