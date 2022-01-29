<template>
  <!-- <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div> -->
  <div>

    <button @click="toggleList" v-if="showList">Hide Employees</button>
    <button @click="toggleList"  v-else>Show Employees</button>
    <div v-show="showList">
    <h1>List of Employees</h1>
    <div :style="searchStyles">
      <input type="text" v-model="query">
      <button :style="{marginLeft: '1rem'}" @click="filter">Search</button>
    </div>
    <ul>
      <li 
        v-for="(employee, index) in filteredEmployees" 
        :key="index" 
        @click="showDetails(index)"
      >
        <p>{{employee.name | capitalize}}</p>
      </li>
    </ul>
    </div>
    <div v-show="showList && !filteredEmployees.length">
      <p>No  Employees Found</p>
      </div>
    <div v-if="display || display === 0" class="card">
    <div>
      <h3>Name: {{employees[display].name | capitalize}}</h3>
      <p>Age: {{employees[display].age}}</p>
      <p>Experience: {{employees[display].experience}}</p>
      <p>Position: {{employees[display].position}}</p>
    </div>     
    <div>
      <button @click="prevEmployee" :disabled="!display">Prev</button>
      <p>{{display+1}} / {{employees.length}}</p>
      <button @click="nextEmployee" :disabled="display === (employees.length-1)">Next</button>
    </div>
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  // components: {
  //   HelloWorld
  // }
  data: ()=> ({
    query: '',
    searchStyles: {
      margin: '2rem 0',
    },
    employees: [{
      name: 'Tanya gashtold',
      age: 32,
      experience: 10,
      position: 'Software Engineer'
    },
    {
      name: 'Tanyaiii gashtold',
      age: 32,
      experience: 10,
      position: 'Software Engineer'
    },
    {
      name: 'Peter parker',
      age: 40,
      experience: 14,
      position: 'Engineer'
    },
    {
      name: 'Igor ivanov',
      age: 22,
      experience: 1,
      position: 'Designer'
    },
    {
      name: 'Olga NOVIK',
      age: 26,
      experience: 3,
      position: 'Accountant'
    },],
    display: undefined,
    showList: true
    //filteredEmployees: []
  }),
  computed:{
    filteredEmployees(){
    const searchString = this.query.trim();
      if(searchString.length > 3){
      return this.employees.filter(empl => {
        return empl.name.toLowerCase().includes(searchString);
      })}
      return this.employees
    }
  },
  methods: {
    filter(){
      const searchString = this.query.trim();
      this.filteredEmployees = this.employees.filter(empl => {
        return empl.name.toLowerCase().includes(searchString );
      })
    },
    prevEmployee(){
      if(this.display > 0){
        this.display --
      }
    },
    nextEmployee(){
      if(this.display < (this.employees.length-1)){
        this.display ++
      }
    },
    showDetails(index) {
      this.display = index;
    },
    toggleList() {
      this.showList = !this.showList;
      this.display = undefined;
    }
  },
  mounted() {
    this.filteredEmployees = this.employees;
  },
  filters: {
    capitalize(name) {
      return name.split(/\s+/).map(part => part.charAt(0).toUpperCase() + part.toLowerCase().slice(1)).join(' ');
    } 
  }


}
</script>

<style scoped lang="scss">
.card {
  border: 2px green solid;
  margin-top: 10px;
  padding: 1rem;
}
</style>