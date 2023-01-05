<template>
    <div>
<NewComponent />
        <form @submit.prevent="save">
   <h1>Employee Registration</h1>           
  <div class="form-group">
    <label >Employee Name</label>
    <input type="text" class="form-control"  v-model="employee.name" aria-describedby="emailHelp" placeholder="Enter Name">
  </div>
  <div class="form-group">
    <label >Employee Address</label>
    <input type="text" class="form-control"  v-model="employee.address" aria-describedby="emailHelp" placeholder="Enter address">
  </div>
  <div class="form-group">
    <label >Employee Mobile</label>
    <input type="text" class="form-control"   v-model="employee.mobile" aria-describedby="emailHelp" placeholder="Enter mobile">
  </div>

  <button type="submit" class="btn btn-primary">Save</button>
</form>




        <table class="table mt-5 ml-5 mr-5 table-dark">
  <thead>
    <tr>
      <th scope="col">Id</th>
      <th scope="col">Employee Name</th>
      <th scope="col">Employee Address</th>
      <th scope="col">Employee Mobile</th>
      <th>Options</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="employee in results" :key="employee.id">
      
      <td>{{employee.id}}</td>
      <td>{{employee.name}}</td>
      <td>{{employee.address}}</td>
      <td>{{employee.mobile}}</td>
      <td>
        <button type="button" class="btn btn-warning" @click="edit(employee)">Edit</button>
        <button type="button" class="btn btn-danger" @click="remove(employee)">Delete</button>

      </td>
    </tr>

  </tbody>
</table>
    </div>
</template>

<script>
  import axios from 'axios'
import NewComponent from './NewComponent.vue';
    export default {
    data() {
        return {
            results: {},
            employee: {
                id: "",
                name: "",
                address: "",
                mobile: "",
            }
        };
    },
    created() {
        this.EmployeeLoad();
    },
    methods: {
        EmployeeLoad() {
            var page = "http://127.0.0.1:8000/api/employees";
            axios.get(page).then(({ data }) => {
                console.log(data);
                this.results = data;
            });
        },
        save() {
            if (this.employee.id == "") {
                this.saveData();
            }
            else {
                this.updateData();
            }
        },
        saveData() {
            axios.post("http://127.0.0.1:8000/api/save", this.employee)
                .then(({ data }) => {
                alert("Saved");
                this.EmployeeLoad();
            });
        },
        edit(employee) {
            this.employee = employee;
        },
        updateData() {
            var editrecords = "http://127.0.0.1:8000/api/update/" + this.employee.id;
            axios.put(editrecords, this.employee)
                .then(({ data }) => {
                this.employee.name = "",
                    this.employee.address = "",
                    this.employee.mobile = "",
                    this.id = "",
                    alert("updated");
                this.EmployeeLoad();
            });
        },
        remove(employee) {
            var url = `http://127.0.0.1:8000/api/delete/${employee.id}`;
            axios.delete(url);
            alert("Deleted");
            this.EmployeeLoad();
        }
    },
    components: { NewComponent }
}
</script>

<style scoped>

</style>