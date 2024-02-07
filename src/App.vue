<script>
import assignments from './data/assignments.json';


export default{
  data() {
    return {
      assignments: [],
      showCompleted: false
    };
  },
  
  methods : {
    calculateDaysRemaining(dueDate) {
      const today = new Date();
      const dueDateTime = new Date(dueDate);
      let daysDifference = 0;

      // Calculate the difference in days
      const timeDifference = dueDateTime.getTime() - today.getTime();
      if(timeDifference < 0){
        daysDifference == 0;
      }
      else{
         daysDifference = Math.ceil(timeDifference / (1000 * 3600 * 24));
      }

      return daysDifference;
    },

  },

  computed: {
    filteredAssignments() {
      return !this.showCompleted
        ? this.assignments
        : this.assignments.filter((assignment) => assignment.PercentComplete == 100 );
    }
  },
  mounted() {

    this.assignments = assignments.assignments;
  }
}
</script>

<template> 
<div class ="wrapper">
  <h1>Assignments</h1>

  <label>
      <input type="checkbox" v-model="showCompleted" />
      Show Completed Assignments
  </label>

  <div class="assignment-list">
      <div v-for="assignment in filteredAssignments" :key="assignment.id" class="assignment-item">
        <h2>{{ assignment.title }}</h2>
        <p><strong>Due Date:</strong> {{ assignment.dueDate }}</p>
        <p><strong>Days Remaining:</strong> {{ calculateDaysRemaining(assignment.dueDate) }}</p>
        <p><strong>Percent Complete:</strong> {{ assignment.PercentComplete }}%</p>
      </div>
  </div>

  </div>
</template>

<style scoped>
.wrapper{
  display: grid;
  grid-template-rows: 50px 50px 1fr;
  gap:10px
}

h2{
  grid-row: 1;
}
label{
grid-row: 2;
}
.assignment-list {
  grid-row: 3;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 50px;
  
}

.assignment-item {
  border: 1px solid black;
  padding: 10px;
  border-radius: 10px;
  width: 400px;
}

</style>
