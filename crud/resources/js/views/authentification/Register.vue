<template>
    <div class="container">
		<div class="card card-register mx-auto mt-5">
			<div class="card-header">Регистрация</div>
			<div class="card-body">
				<form v-on:submit.prevent="register">
					<div class="form-group mb-0">
						<div class="form-row">
							<div class="col-md-6">
								<div class="form-label-group">
									<input
										type="text"
                                        v-model="user.name"
										id="name"
										class="form-control"
										placeholder="Введите имя"
										autofocus="autofocus"
									/>
                                    <div class="invalid-feedback d-block" v-if="errors.name">{{errors.name[0]}}</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
                                    <div class="form-label-group">
                                        <input
                                            type="email"
                                            v-model="user.email"
                                            id="email"
                                            class="form-control"
                                            placeholder="Введите существующий email адрес"
                                        />
                                        <div class="invalid-feedback d-block" v-if="errors.email">{{errors.email[0]}}</div>
                                    </div>
                                </div>
							</div>
						</div>
					</div>
					<div class="form-group">
						<div class="form-row">
							<div class="col-md-6">
								<div class="form-label-group">
									<input
										type="password"
                                        v-model="user.password"
										id="password"
										class="form-control"
										placeholder="Придумайте пароль"
									/>
                                    <div class="invalid-feedback d-block" v-if="errors.password">{{errors.password[0]}}</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-label-group">
									<input
										type="password"
                                        v-model="user.password_confirmation"
										id="password_confirmation"
										class="form-control"
										placeholder="Повторите пароль"
									/>
                                    <div class="invalid-feedback d-block" v-if="errors.password_confirmation">{{errors.password_confirmation[0]}}</div>
								</div>
							</div>
						</div>
					</div>
                    <button type="submit" class="btn btn-primary btn-block">Регистрация</button>
				</form>
				<div class="text-center">
                    <router-link class="d-block small mt-3" to="/login">Страница входа</router-link>
					<router-link to="/reset-password-request" class="d-block small">Забыли пароль?</router-link>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
    import * as auth from '../../services/auth_service';
    export default {
        name: 'Register',
        data() {
            return {
                user: {
                    name: '',
                    email: '',
                    password: '',
                    password_confirmation: ''
                },

                errors: {}
            }
        },
        methods: {
            register: async function() {
                try {
                    await auth.register(this.user);
                    this.error = {};
                    this.$router.push('login');
                } catch (error) {
                    switch (error.response.status) {
                        case 422:
                            this.errors = error.response.data.errors;
                            break;
                        case 500:
                            this.flashMessage.error({
                                message: error.response.data.message,
                                time: 5000
                            });
                            break;
                        default:
                            this.flashMessage.error({
                                message: 'Some error occurred, Please try again!',
                                time: 5000
                            });
                            break;
                    } 
                }
            }
        }
    }
</script>