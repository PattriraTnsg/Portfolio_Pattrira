<template>
<!-- Education Section -->
  <section class="Edu-section pt-5" style="font-family: 'Oswald', monospace">
    <h1 class="title">Education</h1>

  </section>
</template>

<script>
export default {
  name: "EduSection",
  data() {
    return {
      subjects: [],
    };
  },
  methods: {
    async fetchSubjects() {
      try {
        const response = await fetch(`http://localhost:3000/subjects`);
        if (!response.ok) {
          throw new Error("Failed to fetch subjects");
        }
        this.subjects = await response.json();
      } catch (error) {
        console.error("Error fetching subjects:", error);
      }
    },
    async addCard() {
      const newSubject = {
        subject_id: "",
        name: "",
        grade: "",
        credit: "",
      };

      try {
        const response = await fetch(`http://localhost:3000/subjects`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(newSubject),
        });

        if (!response.ok) {
          throw new Error("Failed to add subject");
        }

        const savedSubject = await response.json();
        savedSubject.editMode = true; // เปิดโหมดแก้ไขทันที
        this.subjects.push(savedSubject);
      } catch (error) {
        console.error("Error adding subject:", error);
      }
    },
    async deleteCard(index, id) {
      try {
        const response = await fetch(`http://localhost:3000/subjects/${id}`, {
          method: "DELETE",
        });

        if (!response.ok) {
          throw new Error("Failed to delete subject");
        }

        this.subjects.splice(index, 1);
      } catch (error) {
        console.error("Error deleting subject:", error);
      }
    },
    editCard(index) {
      this.subjects[index].editMode = true;
    },
    async saveChanges(index) {
      const subject = this.subjects[index];

      try {
        const response = await fetch(
          `http://localhost:3000/subjects/${subject.id}`, 
          {
            method: "PUT",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              subject_id: subject.subject_id,
              name: subject.name,
              grade: subject.grade,
              credit: subject.credit,
            }),
          }
        );

        if (!response.ok) {
          throw new Error("Failed to update subject");
        }

        subject.editMode = false; 
      } catch (error) {
        console.error("Error updating subject:", error);
      }
    },
    cancelEdit(index) {
  const subject = this.subjects[index];

  if (subject.id && (!subject.subject_id || !subject.name || !subject.grade || !subject.credit)) {
    this.subjects.splice(index, 1); // ลบออกจาก UI
  } else {
    this.fetchSubjects(); // โหลดข้อมูลใหม่
  }
}
,
  },
  mounted() {
    this.fetchSubjects();
  },
};
</script>

<style>
/* Educate Section */
.Edu-section {
  background-color: rgb(242, 204, 123);
  height: 100vh;
  display: flex;
  flex-direction: column; /* เรียงเป็นแนวตั้ง */
  align-items: center;
  justify-content: center;
  padding: 5vw;
}

.cards-container {
  display: flex; /* เรียงแนวนอน */
  gap: 20px; 
  overflow-x: auto; /* เลื่อนซ้าย-ขวาได้ */
  padding: 10px;
  padding-bottom: 30px;
  white-space: nowrap;
  max-width: 90vw;
}

.cards-container .edit-form {
  width: 15rem;
  display: flex;
  
}

.edit-form #eduform label {
  display: flex;
  width: 50rem; /* หรือกำหนดเป็น px ตามที่ต้องการ */
  max-width: 800px; /* จำกัดความกว้างสูงสุด */
  margin: auto; /* จัดกึ่งกลาง */
  gap: 8px;
  
}

.cards-container div:hover {
  transform: scale(1.05);
  transition: transform 0.3s ease;
}

.cards-container::-webkit-scrollbar {
  height: 8px; /* ปรับขนาด scrollbar */
}

.cards-container::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 4px;
}

.cards-container::-webkit-scrollbar-thumb:hover {
  background: #555;
}

.course-card {
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
}

#edit {
  width: 70%;
  padding: 3px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.button-group {
  margin-top: 10px;
  display: flex;
  gap: 10px;
  margin-top: 1.25rem;
}

button {
  padding: 7px 10px;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
}

.editcard-btn {
  background-color: #3b82f6;
  color: white;
  padding: 0.5rem 1.5rem;
  border-radius: 3rem;
  box-shadow: 0 8px 0px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  justify-content: center;
}
.editcard-btn:hover {
  box-shadow: 0 6px 0px rgba(0, 0, 0, 0.2);
  transform: translateY(1px); 
  color: white;
}


.delete-btn {
  background-color: #e53935;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 3rem;
  box-shadow: 0 8px 0px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  justify-content: center;
}
.delete-btn:hover {
  box-shadow: 0 6px 0px rgba(0, 0, 0, 0.2);
  transform: translateY(1px); 
  color: white;
}

.save-btn {
  background-color: #43a047;
  color: white;
  padding: 0.5rem 1.5rem;
  border-radius: 3rem;
  box-shadow: 0 8px 0px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  justify-content: center;
}
.save-btn:hover {
  box-shadow: 0 6px 0px rgba(0, 0, 0, 0.2);
  transform: translateY(1px); 
  color: white;
}

.cancel-btn {
  background-color: #757575;
  color: white;
  padding: 0.5rem 1.5rem;
  border-radius: 3rem;
  box-shadow: 0 8px 0px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  justify-content: center;
}
.cancel-btn:hover {
  box-shadow: 0 6px 0px rgba(0, 0, 0, 0.2);
  transform: translateY(1px); 
  color: white;
}

.add-btn {
  margin: 30px;
  font-size: 18px; 
  padding: 12px 20px; 
  box-shadow: 0 10px 0px rgba(0, 0, 0, 0.25);
}

.add-btn:hover {
  background-color: #58422c;
  box-shadow: 0 6px 0px rgba(0, 0, 0, 0.2);
  transform: translateY(1px); 
  color: white;
}

</style>
