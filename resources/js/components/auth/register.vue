<template>
	<div>
		<div class="container">
		    <div class="card card-login mx-auto mt-5">
		      <div class="card-header">Register a new Account</div>
		      <div class="card-body">
		        <form @submit.prevent="signup">
		          <div class="form-group">
		            <div class="form-label-group">
		              <input type="text" id="inputName" class="form-control" placeholder="Full Name" autofocus="autofocus" v-model="form.name">
		              <small class="text-danger" v-if="errors.name">{{ errors.name[0] }}</small>
		              <label for="inputName">Full Name</label>
		            </div>
		          </div>
		          <div class="form-group">
		            <div class="form-label-group">
		              <input type="email" id="inputEmail" class="form-control" placeholder="Email address" autofocus="autofocus" v-model="form.email">
		              <small class="text-danger" v-if="errors.email">{{ errors.email[0] }}</small>
		              <label for="inputEmail">Email address</label>
		            </div>
		          </div>
		          <div class="form-group">
		            <div class="form-label-group">
		              <input type="password" id="inputPassword" class="form-control" placeholder="Password" v-model="form.password" >
		              <small class="text-danger" v-if="errors.password">{{ errors.password[0] }}</small>
		              <label for="inputPassword">Password</label>
		            </div>
		          </div>
		          <div class="form-group">
		            <div class="form-label-group">
		              <input type="password" id="confirmInputPassword" class="form-control" placeholder="Confirm Password" v-model="form.password_confirmation">
		              <label for="confirmInputPassword">Confirm Password</label>
		            </div>
		          </div>
		          
		          <button type="submit" class="btn btn-primary btn-block">Register</button>
		        </form>
		        <div class="text-center">
		          <router-link class="d-block small mt-3" to="/">Already have an Account</router-link>
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
			if(User.loggedIn())
			{
				this.$router.push({name: 'home'});
			}
		},
		data()
		{
			return {
				form: {
					name: null,
					email: null,
					password: null,
					password_confirmation: null,
				},
				errors: {},
			}
		},
		methods: {
			signup()
			{
				axios.post('api/auth/signup',this.form)
				.then(response => {

					User.responseAfterLogin(response);

					Toast.fire({
					  type: 'success',
					  title: 'Signed in successfully'
					});

					this.$router.push({name: 'home'});

				})
				.catch(error => {
					console.log(error.response.data.errors);
					this.errors = error.response.data.errors;
				})
			}
		}
	}
</script>

<style type="text/css">
	
</style>