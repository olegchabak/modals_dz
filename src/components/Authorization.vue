<template>
	<modal title="Authorization" @modalClose="closeModal">
		<div slot="modalBody">
			<form @submit.prevent="onSubmit">
				<div class="form-item" :class="{'form-item--error': $v.login.$error}">
					<label> Login:
						<input type="text" v-model="login" @change="$v.login.$touch()">
					</label>
					<error-message :error="$v.login.$params"></error-message>
				</div>
				<div class="form-item" :class="{'form-item--error': $v.password.$error}">
					<label>Password:
						<input type="password" v-model="password" @change="$v.password.$touch()">
					</label>
					<error-message :error="$v.password.$params"></error-message>
				</div>
				<button class="btn btnPrimary">Submit</button>
			</form>
		</div>
		<div slot="modalFooter">
			<div class="modal-footer__inner">
				You don't have an account? <a href="#" @click.prevent="openModal('registration')">Register</a> now!
			</div>
		</div>
	</modal>
</template>

<script>
	import modal from '@/components/UI/Modal';
	import { required, minLength } from 'vuelidate/lib/validators';
	import errorMessage from '@/components/UI/ErrorMessage';

	export default {
		name: "authorization",
		components: {modal, errorMessage},
		data() {
			return {
				login: '',
				password: ''
			}
		},
		validations: {
			login: {required, minLength:minLength(2)},
			password: {required, minLength:minLength(6)},
		},
		methods: {
			clearForm() {
				this.$v.login.$model = ''
				this.$v.password.$model = ''
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
						login: this.$v.login.$model,
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