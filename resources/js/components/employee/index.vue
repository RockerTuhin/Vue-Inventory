<template>
	<div>
		<!-- Breadcrumbs -->
		<ol class="breadcrumb">
			<li class="breadcrumb-item">
				<a href="">Dashboard</a>
			</li>
			<li class="breadcrumb-item active">All Employee</li>
		</ol>
		<!-- Icon Cards -->
		<div class="row container">
		    <div class="card col-lg-12">
		      <div class="card-header">
		      	<i class="fas fa-chart-area"></i>
		      	All Employee
		      	<router-link to="/add-employee" class="btn btn-sm btn-info align-right" id="add_new">Add Employee</router-link>
		      </div>
		      <div class="card-body">
		      	<label>Search:</label>
		      	<input type="text" v-model="searchTerm">
		        <div class="table-responsive">
	              <table class="table table-bordered" id="" width="100%" cellspacing="0">
	                <thead>
	                  <tr>
	                    <th>Name</th>
	                    <th>Phone</th>
	                    <th>Photo</th>
	                    <th>Salary</th>
	                    <th>Joining date</th>
	                    <th>Action</th>
	                  </tr>
	                </thead>
	                <tfoot>
	                  <tr>
	                    <th>Name</th>
	                    <th>Phone</th>
	                    <th>Photo</th>
	                    <th>Salary</th>
	                    <th>Joining date</th>
	                    <th>Action</th>
	                  </tr>
	                </tfoot>
	                <tbody>
	                  <tr v-for="employee in filterSearch" :key="employee.id">
	                    <td>{{ employee.name }}</td>
	                    <td>{{ employee.phone }}</td>
	                    <td><img :src="employee.photo" id="employee_photo"></td>
	                    <td>{{ employee.salary }}</td>
	                    <td>{{ employee.joining_date }}</td>
	                    <td>
	                    	<router-link :to="{ name: 'edit_employee', params:{id: employee.id} }" class="btn btn-sm btn-info">Edit</router-link>
	                    	<a @click="deleteEmployee(employee.id)" class="btn btn-sm btn-danger">Delete</a>
	                    </td>
	                  </tr>
	                </tbody>
	              </table>
	            </div>
		       
		      </div>
		    </div>
		</div>
	</div>
</template>

<script type="text/javascript">
	export default {
		mounted()
		{
			if(!User.loggedIn())
			{
				this.$router.push({name: '/'});
			}
			this.allEmployee();
		},
		data()
		{
			return {
				employees:[],
				searchTerm: '',
			}
		},
		computed: {
			filterSearch()
			{
				return this.employees.filter(employee => {
					return employee.phone.match(this.searchTerm) || employee.name.match(this.searchTerm);
				});
			}
		},
		methods: {
			allEmployee()
			{
				axios.get('api/employee')
				.then(response => {
					this.employees = response.data;
					console.log(response.data);
				})
				.catch(error => {
					console.log(error)
				})
			},
			deleteEmployee(id)
			{
				Swal.fire({
				  title: 'Are you sure?',
				  text: "You won't be able to revert this!",
				  type: 'warning',
				  showCancelButton: true,
				  confirmButtonColor: '#3085d6',
				  cancelButtonColor: '#d33',
				  confirmButtonText: 'Yes, delete it!'
				}).then((result) => {
				  if (result.value) {
				  	axios.delete('api/employee/'+id)
				  	.then(() => {
				  		this.employees = this.employees.filter(employee => {
				  			return employee.id != id;
				  		})
				  	})
				  	.catch(() => {
				  		this.$router.push({name: 'all_employee'});
				  	})
				    Swal.fire(
				      'Deleted!',
				      'Your file has been deleted.',
				      'success'
				    )
				  }
				})
			}
		},
		created()
		{
			
		},
		
	}
</script>

<style type="text/css">
	#add_new {
		float: right;
	}
	#employee_photo {
		width: 60px;
		height: 60px;
	}
</style>