<template>
  <q-page class="q-pa-md">
    <q-card class="shadow-4 rounded-borders">
      <q-card-section class="q-pa-none">
        <div class="text-h6 text-center q-py-md">Gerenciar Tarefas</div>
      </q-card-section>

      <q-card-section class="q-pa-md">
        <div class="q-gutter-md">
          <q-input
            v-model="newTask.title"
            label="Nova Tarefa"
            outlined
            dense
            autofocus
            class="rounded-borders"
            @keyup.enter="createTask"
          />
          <div class="q-mt-md q-mb-none flex justify-center">
            <q-btn
              label="Adicionar Tarefa"
              color="primary"
              class="full-width rounded-borders"
              @click="createTask"
            />
          </div>
        </div>
      </q-card-section>

      <q-separator />

      <q-card-section class="q-pa-md">
        <q-list>
          <q-item
            v-for="task in tasks"
            :key="task.id"
            clickable
            class="q-mb-sm rounded-borders bg-grey-2 hover:bg-grey-3"
          >
            <q-item-section>
              <q-checkbox
                v-model="task.completed"
                @update:model-value="updateTask(task)"
                color="primary"
              />
              <span :class="{ 'text-strike': task.completed }">{{ task.title }}</span>
            </q-item-section>

            <q-item-section side>
              <q-btn
                icon="delete"
                color="negative"
                flat
                @click="confirmDeleteTask(task.id)"
                class="rounded-borders"
              />
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>

      <q-card-section class="q-pa-md">
        <div class="flex justify-center">
          <q-btn
            label="Exportar para PDF"
            color="secondary"
            @click="exportToPDF"
          />
        </div>
      </q-card-section>
    </q-card>

    <q-dialog v-model="showDeleteDialog">
      <q-card>
        <q-card-section class="q-pa-md">
          <div class="text-h6">Tem certeza que deseja excluir esta tarefa?</div>
        </q-card-section>

        <q-card-actions>
          <q-btn label="Cancelar" color="secondary" @click="showDeleteDialog = false" />
          <q-btn label="Excluir" color="negative" @click="deleteTask" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import axios from 'axios';
import { jsPDF } from 'jspdf';

export default {
  data() {
    return {
      apiUrl: 'http://127.0.0.1:8000/api/tasks',
      tasks: [],
      newTask: {
        title: '',
        description: '',
        completed: false
      },
      showDeleteDialog: false,  
      taskToDelete: null        
    };
  },
  methods: {
    async fetchTasks() {
      try {
        const response = await axios.get(this.apiUrl);
        this.tasks = response.data;
      } catch (error) {
        console.error('Erro ao carregar tarefas:', error);
      }
    },
    async createTask() {
      if (!this.newTask.title.trim()) return;
      try {
        await axios.post(this.apiUrl, this.newTask);
        this.newTask.title = '';
        this.fetchTasks();
      } catch (error) {
        console.error('Erro ao criar tarefa:', error);
      }
    },
    async updateTask(task) {
      try {
        await axios.put(`${this.apiUrl}/${task.id}`, task);
      } catch (error) {
        console.error('Erro ao atualizar tarefa:', error);
      }
    },
    async deleteTask() {
      if (this.taskToDelete) {
        try {
          await axios.delete(`${this.apiUrl}/${this.taskToDelete}`);
          this.fetchTasks();
          this.showDeleteDialog = false;  
          this.taskToDelete = null;       
        } catch (error) {
          console.error('Erro ao deletar tarefa:', error);
        }
      }
    },
    confirmDeleteTask(taskId) {
      this.taskToDelete = taskId; 
      this.showDeleteDialog = true; 
    },
    exportToPDF() {
      const doc = new jsPDF();
      doc.setFontSize(18);
      doc.text('Tarefas', 20, 20);

      doc.setFontSize(12);
      this.tasks.forEach((task, index) => {
        const yPosition = 30 + index * 10;
        doc.text(`${task.title} - ${task.completed ? 'Concluída' : 'Pendente'}`, 20, yPosition);
      });

      doc.save('tarefas.pdf');
    }
  },
  mounted() {
    this.fetchTasks();
  }
};
</script>

<style scoped>
/* Estilos adicionais */
.text-strike {
  text-decoration: line-through;
}

.rounded-borders {
  border-radius: 8px;
}

.q-pa-none {
  padding: 0;
}

.q-gutter-md {
  gap: 16px;
}

.bg-grey-2 {
  background-color: #f5f5f5;
}

.hover\:bg-grey-3:hover {
  background-color: #e0e0e0;
}

.flex {
  display: flex;
}

.justify-center {
  justify-content: center;
}

.q-mt-md {
  margin-top: 16px;
}

.q-mb-none {
  margin-bottom: 0;
}
</style>
