<template>
	<div class="row">
		<div class="col-md-6 offset-md-3 col-xl-4 offset-xl-4">
			<form>
                <div>
                    <b-alert dismissible v-model="showMessage" fade variant="success">{{message}}</b-alert>
                    <b-alert dismissible v-model="showErrorMessage" fade variant="danger">{{errorMessage}}</b-alert>
                </div>
				<div class="form-group">
					<label>Username</label>
					<input v-model="username" type="text" class="form-control" placeholder="Username">
				</div>
				<div class="form-group">
					<label>Password</label>
					<input v-model="password" type="password" class="form-control" placeholder="Password">
				</div>
				<div v-if="invalidCredentials" class="form-group">
					<small class="text-danger">Invalid credentials</small>
				</div>
				<button type="submit" @click.prevent="onSubmit(username, password)" class="btn btn-primary">Login</button>
			</form>
		</div>
	</div>
</template>
 
<script>
import axios from 'axios';
import jwt_decode from '../../node_modules/jwt-decode';
import Alert from './Alert.vue';


export default {
    data() {
        return {
            username: '',
            password: '',
            invalidCredentials: false,
            user_id: '',
        }
	},
    components: {
        alert: Alert,
    },
    methods: {
        onSubmit(username, password){
            let formData = {
					login: this.username,
					password: this.password,
            }
            const path = window.location.protocol + "//" + window.location.hostname + ':' + process.env.VUE_APP_AUTH_PORT + `/auth/login`

            axios.post(path, formData)
                .then((res) => {
                localStorage.setItem('token', res.data.access_token);
                localStorage.setItem('refresh_token', res.data.refresh_token)
                localStorage.setItem('role', res.data.role)

                const user_id = jwt_decode(localStorage.getItem('token'))['sub']
                console.log("user_id = ", jwt_decode(localStorage.getItem('token'))['sub'])
                const path = '/user/' + user_id + '/order';
                this.$router.push(path);                               
                })
                .catch((error) => {
                    // eslint-disable-next-line
                    this.errorMessage = "Bad credentials.";
                    this.showErrorMessage = true;
                    console.error(error);
                });
        }
    
    },
    
}
</script>