<template>
  <div class="min-w-full min-h-screen h-screen overflow-hidden bg-blue-100">
  <Header/>
  <div class="flex mt-3 mx-4 justify-center mx-4 space-x-4 min-w-full">
    <div v-for="column in columns" :key="column.title" class="flex flex-col min-w-[300px] min-h-[300px] relative bg-gray-100  p-4 shadow overflow-hidden rounded-lg">
      <h4 class=" p-3 flex justify-between items-center mb-2">
              <span class="text-2xl text-gray-600">
                {{ column.title }}
              </span>
        <font-awesome-icon icon="fa-solid fa-ellipsis-v" class="w-5 h-5 text-gray-500" />
      </h4>
      <div class="flex flex-col space-y-4">
        <draggable :list="column.tasks" :animation="200" ghost-class="ghost-card" group="tasks">
        <task-card
            v-for="(task) in column.tasks"
            :key="task.id"
            :task="task"
            class="mt-3 cursor-move"
            group="task"
            @start="drag=true"
            @end="drag=false"
            @toggle-delete="deleteTask(column, task)"
        ></task-card>
        </draggable>
      </div>
      <button
          class="flex justify-center items-center my-3 space-x-2 text-lg text-gray-300"
          @click="addTask(column)"
      >
      <span>Add task</span>
      <font-awesome-icon icon="fa-solid fa-plus" className="w-5 h-5 text-gray-500" />
      </button>
    </div>
  </div>
  </div>
</template>

<script>
import { VueDraggableNext } from 'vue-draggable-next';
import Header from "./components/header.vue";
import TaskCard from "./components/TaskCard.vue";
export default {
  name: "App",
  created () {
      document.title = 'Trello Board'
  },
  components: {
    TaskCard,
    Header,
    draggable : VueDraggableNext
  },
  data() {
    return {
      columns: [
        {
          title: "Backlog",
          tasks: [
            {
              id: 654654,
              title: "Add discount code to checkout page",
              date: "2023-06-22",
              type: "Feature Request",
              url:"https://randomuser.me/api/portraits/men/75.jpg"
            },
            {
              id: 145687,
              title: "Provide documentation on integrations",
              date: "2023-07-53",
              url:"https://randomuser.me/api/portraits/men/76.jpg"
            }
          ]
        },
        {
          title: "In Progress",
          tasks: [
            {
              id: 646545,
              title: "Design shopping cart dropdown",
              date: "2023-01-25",
              type: "Design",
              url:"https://randomuser.me/api/portraits/men/76.jpg"
            }
          ]
        },
        {
          title: "Review",
          tasks: [
            {
              id: 956746,
              title: "Provide documentation on integrations",
              date: "2023-06-18",
              url:"https://randomuser.me/api/portraits/men/75.jpg"
            },
          ]
        },
        {
          title: "Done",
          tasks: [
            {
              id: 456546,
              title: "Add discount code to checkout page",
              date: "2023-06-27",
              type: "Feature Request",
              url:"https://randomuser.me/api/portraits/men/78.jpg"
            },
          ]
        }
      ]
    };
  },
  methods: {
    addTask(column) {
      const newTask = {
        id: Date.now(), 
        title: "Nova Tarefa",
        date: "",
        type: "",
        url: "https://randomuser.me/api/portraits/men/75.jpg"
      };
      this.$swal.fire({
        title: 'Criar Tarefa',
        showCancelButton: true,
        confirmButtonText: 'Salva Tarefa',
        showLoaderOnConfirm: true,
        width: 600,
        html:
            '<input id="title-input" type="text" class="swal2-input" placeholder="Nome da Tarefa">' +
            '<input id="date-input" type="date" class="swal2-input ">'+
            '<select id="type-input" class="swal2-input shadow"> ' +
            ' <option value="design">Escolha o tipo da Tarefa</option>' +
            ' <option value="design">Design</option>' +
            ' <option value="QA">Q&A</option>' +
            ' <option value="Feature Request">Feature Request</option>' +
            '</select>',
        preConfirm: function () {
          return new Promise(function (resolve) {
            resolve([
              document.getElementById("title-input").value,
              document.getElementById("date-input").value,
              document.getElementById("type-input").value,
            ])
          })
        },
      }).then((result) => {
        if (result) {
          newTask.title = result.value[0];
          newTask.date = result.value[1];
          newTask.type = result.value[2];
          console.log(newTask)
          column.tasks.push(newTask);
          this.$swal.fire(
              'Criado!',
              'A tarefa foi criado.',
              'success'
          )
        }
      })
      if (newTitle) {
        newTask.title = newTitle;
      column.tasks.push(newTask);
      }
    },
    deleteTask(column, task) {
      this.$swal.fire({
        title: 'Tem certeza?',
        text: "Você não poderá reverter isso!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Foi Excluido'
      }).then((result) => {
        if (result.isConfirmed) {
          const index = column.tasks.indexOf(task);
          if (index !== -1) {
            column.tasks.splice(index, 1);
          }
          this.$swal.fire(
              'Deletado!',
              'Seu arquivo foi excluído.',
              'success'
          )
        }
      })

    }
  }
};
</script>

