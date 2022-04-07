<template>
  <div class="container">
    <div class="card center">
      <h1>toDo App</h1>
      <AddTaskBlock  v-model.trim="valueInput" @add="addTask"/>
      <TheFilterPanel :filter-status="activeFilter" :names-buttons="buttonsName" @show="setFilter"/>
      <TheAlert :alert="alert" @close="alert = null"/>
      <div v-if="!isLoadSuccess">
        <TheLoader />
      </div>
      <div v-else>
        <TheList :list="tasks" @finish="markTask" :filter="activeFilter" @remove="removeTask"/>
      </div>
    </div>
  </div>
</template>

<script>
import TheLoader from './components/TheLoader.vue'
import AddTaskBlock from './components/AddTaskBlock.vue'
import TheAlert from './components/TheAlert.vue'
import TheList from './components/TheList.vue'
import TheFilterPanel from './components/TheFilterPanel.vue'
export default {
  data() {
    return {
      isLoadSuccess: false,
      tasks: [],
      valueInput: "",
      currentId: 0,
      activeFilter: '',
      buttonsName: [ { name: 'Все', pvsevdo: 'All' }, { name: 'Активные', pvsevdo: 'Active' }, { name: 'Завершенные', pvsevdo: 'Finished' } ],
      alert: null
    }
  },

  mounted() {
    this.loadTasks();
  },

  methods: {
    loadTasks() {
      setTimeout(async () => {
        try {
          const response = await fetch("https://my-json-server.typicode.com/falk20/demo/todos");
          if (!response) {
            throw new Error('Список задач пуст или его не удалось загрузить')
          }
          this.tasks = await response.json();
          this.activeFilter = 'All';
          this.currentId = this.tasks.length;
          this.isLoadSuccess = true;
          } catch (e) {
          this.alert = {
            type: 'danger',
            title: 'Ошибка загрузки!',
            text: `${e.message} - Список задач пуст или его не удалось загрузить`
          }
          this.activeFilter = 'All';
          this.currentId = this.tasks.length;
          this.isLoadSuccess = true;
        }
      }, 5000);
    },
    addTask() {
      this.currentId += 1;
      const task = { id: this.currentId, text: this.valueInput, active: true };
      this.tasks.push( task );
      this.valueInput = '';
    },
    markTask( id ) {
      const toggle = this.tasks.find( t => t.id === id ).active;
      this.tasks.find( t => t.id === id ).active = !toggle;
    },
    setFilter( filter ) {
      if( filter === 'Active' ) {
        this.activeFilter = 'Active';
      } else if( filter === 'Finished' ) {
        this.activeFilter = 'Finished';
      } else {
        this.activeFilter = 'All';
      }
    },
    removeTask( id ) {
      this.tasks = this.tasks.filter( t => t.id !== id );
    }
  },
  computed: {
  },

  components: { AddTaskBlock, TheLoader, TheList, TheFilterPanel, TheAlert }
}
</script>

<style>
</style>
