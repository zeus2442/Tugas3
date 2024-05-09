<template>
  <div class="background">
    <img src="https://wallpaperwaifu.com/wp-content/uploads/2023/12/ocean-wave-thumb.jpg"/>
  </div>
  <div class="container">
    <div class="card bg-blur border-1 mt-0 ">
      <div class="card-header d-flex justify-content-between align-items-center">
        <h5 class="mb-0">List Kegiatan</h5>
      </div>
      <div class="card-body">
        <div class="input-group mb-3">
          <input type="text" class="form-control" placeholder="Add new task" style="background-color: rgba(255, 255, 255, 0.5);" v-model="newTask" @keyup.enter="addTask">
          <button class="btn btn-primary" @click="addTask">+</button>
        </div>
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">Kegiatan</th>
              <th scope="col">Status</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in filteredTasks" :key="index">
              <td :class="{ 'text-decoration-line-through': task.done }">{{ task.text }}</td>
              <td>
                <input type="checkbox" class="form-check-input" v-model="task.done" @change="toggleTaskDone(task)">
              </td>
              <td>
                <button class="btn btn-danger" @click="removeTask(index)">Hapus</button>
                <button class="btn btn-info" @click="editTask(task)">Edit</button>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="d-flex justify-content-between align-items-center">
          <button class="btn btn-primary" @click="showUnfinishedOnly = !showUnfinishedOnly">
            Show Unfinished
          </button>
          <button class="btn btn-secondary" v-if="showUnfinishedOnly" @click="showUnfinishedOnly = false">
            Hapus Filter
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2';

export default {
  data() {
    return {
      tasks: [
        { text: 'Nongkrong', done: false },
        { text: 'Menghabiskan Waktu', done: true },
        { text: 'Hiling', done: false },
      ],
      newTask: '',
      showUnfinishedOnly: false,
    }
  },
  computed: {
    filteredTasks() {
      if (this.showUnfinishedOnly) {
        return this.tasks.filter(task => !task.done)
      } else {
        return this.tasks
      }
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask.trim(), done: false })
        this.newTask = ''
      }
    },
    toggleTaskDone(task) {
      task.done = !task.done
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(task) {
      Swal.fire({
        title: 'Edit Kegiatan',
        input: 'text',
        inputValue: task.text,
        showCancelButton: true,
        confirmButtonText: 'Simpan',
        cancelButtonText: 'Batal',
        inputValidator: (value) => {
          if (!value) {
            return 'Kolom tidak boleh kosong!';
          }
        }
      }).then((result) => {
        if (result.isConfirmed) {
          task.text = result.value;
          Swal.fire({
            icon: 'success',
            title: 'Kegiatan Diperbarui!',
            showConfirmButton: false,
            timer: 1500
          });
        }
      });
    }
  },
}
</script>

<style scoped>
.list-unstyled{
  color: white;
}
.card-header{
  border-color: white;
}
h5{
  color: white;
  font-weight: bold;
}
.border-1{
  border-color: white;
}

.bg-blur {
  background-color: transparent;
  backdrop-filter: blur(5px);
  border-radius: 1rem;
  top: 5%;
  left: -50%;
}
.background img{
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 1500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
  filter: brightness(0.8);
}
</style>
