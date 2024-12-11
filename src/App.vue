<template>
  <div id="app">
    <header>
      <h1>FixyPro</h1>
      <button class="request-button">Inicia Solicitud</button>
    </header>

    <main>
      <section class="problems">
        <div class="problem-card" v-for="problem in problems" :key="problem.id">
          <img :src="problem.image" alt="Problem image" />
          <h2>{{ problem.title }}</h2>
          <p>{{ problem.category }}</p>
          <p>{{ problem.description }}</p>
          <button @click="viewDetails(problem.id)">Más información</button>
        </div>
      </section>

      <!-- Modal for "More Info" -->
      <div v-if="selectedProblem" class="modal">
        <div class="modal-content">
          <h3>{{ selectedProblem.title }}</h3>
          <p><strong>Categoría:</strong> {{ selectedProblem.category }}</p>
          <p><strong>Descripción:</strong> {{ selectedProblem.description }}</p>
          <p><strong>Dirección:</strong> {{ selectedProblem.address }}</p>
          <div>
            <button @click="editProblem">Editar</button>
            <button @click="deleteProblem">Eliminar</button>
            <button @click="cancelDetails">Cancelar</button>
          </div>
        </div>
      </div>

      <div v-if="showForm" class="modal">
        <h3>Publicar problema</h3>
        <form @submit.prevent="publishProblem">
          <input v-model="form.title" placeholder="Problema" required />
          <input v-model="form.description" placeholder="Descripción" required />
          <input v-model="form.address" placeholder="Dirección" required />
          <input type="file" @change="handleFileUpload" />
          <button type="submit">Publicar</button>
          <button type="button" @click="cancelForm">Cancelar</button>
        </form>
      </div>
    </main>

    <!-- Navbar at the bottom -->
    <footer>
      <nav>
        <button>Inicio</button>
        <button>Historial</button>
        <button>Notificaciones</button>
        <button>Perfil</button>
      </nav>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      problems: [
        {
          id: 1,
          title: "Mesa rota",
          category: "Carpintería",
          description: "Se rompió la mesa de una esquina y se despegó la pata",
          image: "https://via.placeholder.com/150",
          address: "Arrabal Samuel Galindo 57, Sandy Springs, Nav",
        },
      ],
      showForm: false,
      form: {
        title: "",
        description: "",
        address: "",
      },
      selectedProblem: null,
    };
  },
  methods: {
    viewDetails(id) {
      this.selectedProblem = this.problems.find((problem) => problem.id === id);
    },
    publishProblem() {
      const newProblem = {
        ...this.form,
        id: Date.now(),
        image: "https://via.placeholder.com/150",
      };
      this.problems.push(newProblem);
      this.showForm = false;
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      console.log(file);
    },
    cancelForm() {
      this.showForm = false;
    },
    cancelDetails() {
      this.selectedProblem = null;  // Cerrar el modal de "Más información"
    },
    editProblem() {
      alert("Editar problema - funcionalidad pendiente");
    },
    deleteProblem() {
      if (this.selectedProblem) {
        this.problems = this.problems.filter((problem) => problem.id !== this.selectedProblem.id);
        this.selectedProblem = null;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  height: 100vh; /* Para asegurarse de que todo el contenido ocupe el 100% de la altura de la pantalla */
}

header {
  background: #f4a71d;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.request-button {
  background: #007bff;
  color: #fff;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
}

.problems {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  flex: 1; /* Para que ocupe el espacio restante de la pantalla */
}

.problem-card {
  border: 1px solid #ddd;
  padding: 1rem;
  width: 200px;
}

.modal {
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 1rem;
  border-radius: 8px;
  width: 400px;
}

.problem-details {
  padding: 1rem;
  border: 1px solid #ddd;
  margin: 1rem;
}

footer {
  background: #f4a71d;
  padding: 1rem;
  display: flex;
  justify-content: space-around;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}
</style>
