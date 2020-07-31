<template>
	<modal title="Registration" @modalClose="closeModal">
		<div slot="modalBody">
			<form @submit.prevent="onSubmit">
				<!--login-->
				<div class="form-item" :class="{'form-item--error': $v.login.$error}">
					<label> Login:
						<input type="text" v-model="login" @change="$v.login.$touch()">
					</label>
					<error-message :error="$v.login.$params"></error-message>
				</div>
				<!--name-->
				<div class="form-item" :class="{'form-item--error': $v.name.$error}">
					<label> Name:
						<input type="text" v-model="name" @change="$v.name.$touch()">
					</label>
					<error-message :error="$v.name.$params"></error-message>
				</div>
				<!--email-->
				<div class="form-item" :class="{'form-item--error': $v.email.$error}">
					<label> Email:
						<input type="email" v-model="email" @change="$v.email.$touch()">
					</label>
					<error-message :error="$v.email.$params"></error-message>
				</div>
				<!--Password-->
				<div class="form-item" :class="{'form-item--error': $v.password.$error}">
					<label>Password:
						<input type="password" v-model="password" @change="$v.password.$touch()">
					</label>
					<error-message :error="$v.password.$params"></error-message>
				</div>
				<!--Password Repeat-->
				<div class="form-item" :class="{'form-item--error': $v.passwordRepeat.$error}">
					<label>Repeat password:
						<input type="password" v-model="passwordRepeat" @change="$v.passwordRepeat.$touch()">
					</label>
					<error-message :error="$v.passwordRepeat.$params"></error-message>
				</div>
				<button class="btn btnPrimary">Submit</button>
			</form>
		</div>
		<div slot="modalFooter">
			<div class="modal-footer__inner">
				I already have an account. Go to <a href="#" @click.prevent="openModal('authorization')">authorization</a>.
			</div>
		</div>
	</modal>
</template>

<script>
	import modal from '@/components/UI/Modal';
	import { required, minLength, alpha, sameAs, email } from 'vuelidate/lib/validators';
	import errorMessage from '@/components/UI/ErrorMessage';

	export default {
		name: "authorization",
		components: {modal, errorMessage},
		data() {
			return {
				name: '',
				login: '',
				email: '',
				password: '',
				passwordRepeat: ''
			}
		},
		validations: {
			name: {required, minLength:minLength(2), alpha},
			login: {required, minLength:minLength(2)},
			email: { email, required, minLength:minLength(2)},
			password: {required, minLength:minLength(6)},
			passwordRepeat: { sameAsPassword:sameAs('password'), required },
		},
		methods: {
			clearForm() {
				this.$v.login.$model = ''
				this.$v.name.$model = ''
				this.$v.email.$model = ''
				this.$v.password.$model = ''
				this.$v.passwordRepeat.$model = ''
				this.$v.$reset()
			},
			closeModal() {
				this.clearForm();
				this.$emit('modalClose');
			},
			onSubmit() {
				this.$v.$touch();
				if (!this.$v.$invalid) {
					const data = {
						name: this.$v.name.$model,
						login: this.$v.login.$model,
						email: this.$v.email.$model,
						password: this.$v.password.$model,
					}
					console.log(data);
					this.closeModal();
				}
			},
			openModal(modalName) {
				this.closeModal();
				this.$emit('openModal', modalName);
			}
		}
	}
</script>

<style lang="scss">

</style>