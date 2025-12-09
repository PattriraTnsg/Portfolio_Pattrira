<template>
<!-- Contact Section -->
  <section class="contact-section">
    <div class="contact-container">
      <h1 class="title">Contact</h1>
      <div class="contact-card">
        <img :src="contact_pic" alt="me" class="contact_img" />  
        <div class="contact-info">
          <h4>
            Email : pattrira.tnsg@gmail.com <br />
            IG : pt_11.3_npc <br />
            Tel. : 097-110-9214 <br>
            Github : <a href="https://github.com/PattriraTnsg?tab=repositories" style="text-decoration:none; color: #444;" >PattriraTnsg </a>
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
  data(){
    return{
        contact_pic
    }
  }
};
</script>

<style scoped>
.contact-section {
  background-color: #333;
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
  color:  rgb(242, 204, 123);
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
  max-width: 600px;
  width: 100%;
  text-align: center;
  transition: transform 0.3s ease-in-out;
}

.contact-card:hover {
  transform: scale(1.05);
}

.contact-info {
  text-align: left;
}

.contact-info h4 {
  font-size: 1.2rem;
  color: #444;
  margin: 0.8rem 0;
  line-height: 3rem;
}

.contact_img {
  width: 200px;
  height: 200px;
  justify-content: center;
  border-radius: 60%;
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
</style>
