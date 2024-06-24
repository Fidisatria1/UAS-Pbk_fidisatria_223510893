<template>
  <q-page class="todos-container q-pa-md">
    <q-card flat bordered class="q-pa-md card-container">
      <q-card-section class="q-gutter-md">
        <div class="post-title">Tambahkan Daftar Todo List Anda</div>
        <q-input v-model="newTodo" placeholder="Tambah item baru di sini..." outlined />
        <q-btn @click="addTodo" label="Tambah" color="primary" class="q-mt-md" />
        <q-btn-dropdown label="Filter" color="primary" class="q-mt-md">
          <q-list>
            <q-item clickable @click="filter = 'all'">
              <q-item-section>Semua</q-item-section>
            </q-item>
            <q-item clickable @click="filter = 'done'">
              <q-item-section>Selesai</q-item-section>
            </q-item>
            <q-item clickable @click="filter = 'pending'">
              <q-item-section>Belum Selesai</q-item-section>
            </q-item>
          </q-list>
        </q-btn-dropdown>
      </q-card-section>
    </q-card>
    <q-separator spaced class="q-my-md" />
    <TodoList :todos="filteredTodos" @remove-todo="removeTodo" />
  </q-page>
</template>

<script>
import TodoList from './TodoList.vue';

export default {
  components: {
    TodoList,
  },
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos;
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.done);
      } else if (this.filter === 'pending') {
        return this.todos.filter(todo => !todo.done);
      }
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({
          text: this.newTodo,
          done: false,
        });
        this.newTodo = '';
        this.animateAction('added');
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
      this.animateAction('removed');
    },
    animateAction(action) {
      const notification = document.createElement('div');
      notification.className = `notification ${action}`;
      notification.textContent = `Item ${action}`;
      document.body.appendChild(notification);
      setTimeout(() => {
        document.body.removeChild(notification);
      }, 2000);
    },
  },
};
</script>

<style scoped>
.todos-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.post-title {
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.card-container {
  background: #f0f0f0; /* Light gray background */
  color: #333333;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}

.q-input {
  border-color: #757575; /* Gray border */
  border-radius: 8px;
}

.q-btn {
  background-color: #757575; /* Gray background */
  color: white;
  border-radius: 8px;
}

.q-btn:hover {
  background-color: #5f5f5f; /* Darker gray on hover */
}

.q-btn-dropdown {
  background-color: #757575; /* Gray background */
  color: white;
  border-radius: 8px;
}

.q-btn-dropdown:hover {
  background-color: #5f5f5f; /* Darker gray on hover */
}

.q-separator {
  border-color: #757575; /* Gray border */
}

.notification {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: #757575; /* Gray background */
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  z-index: 1000;
  animation: fadeInOut 2s;
}

.notification.removed {
  background: #b0b0b0; /* Lighter gray for removed notification */
}

@keyframes fadeInOut {
  0%, 100% { opacity: 0; }
  50% { opacity: 1; }
}

@media (max-width: 600px) {
  .todos-container {
    padding: 10px;
  }

  .card-container {
    padding: 10px;
  }
}
</style>
