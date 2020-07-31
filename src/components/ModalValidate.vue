<template>
	<modal title="Modal with validate" @modalClose="closeModal">
		<div slot="modalBody">
			<form @submit.prevent="onSubmit">
				<!--name-->
				<div class="form-item" :class="{'form-item--error': $v.name.$error}">
					<label for="inputName">Your name:</label>
					<input id="inputName" v-model="name" :class="{error: $v.name.$error}" @change="$v.name.$touch()">
					<p class="error-text" v-if="!$v.name.required">Field is required</p>
					<p class="error-text" v-if="!$v.name.alpha">The field must contain only letters</p>
					<p class="error-text" v-if="!$v.name.minLength">The field must contain at least {{$v.name.$params.minLength.min}} characters</p>
				</div>
				<!--password-->
				<div class="form-item" :class="{'form-item--error': $v.password.$error}">
					<label for="inputPassword">Password:</label>
					<input id="inputPassword"
					       v-model.trim="$v.password.$model"
					       :class="{error: $v.password.$error}"
					>
					<p class="error-text" v-if="!$v.password.required">Field is required</p>
					<p class="error-text" v-if="!$v.password.minLength">The field must contain at least {{$v.password.$params.minLength.min}} characters</p>
				</div>
				<!--repeat password-->
				<div class="form-item" :class="{'form-item--error': $v.passwordRepeat.$error}">
					<label for="inputPasswordRepeat">Repeat password:</label>
					<input id="inputPasswordRepeat"
					       type="password"
					       :class="{error: $v.passwordRepeat.$error}"
					       v-model.trim="$v.passwordRepeat.$model"
					       >
					<p class="error-text" v-if="!$v.passwordRepeat.required">Field is required</p>
					<p class="error-text" v-if="!$v.passwordRepeat.sameAsPassword">Passwords don't match</p>
				</div>
				<!--email-->
				<div class="form-item" :class="{'form-item--error': $v.email.$error}">
					<label for="inputEmail">Your email:</label>
					<input
						id="inputEmail"
						v-model="email"
						:class="{error: $v.email.$error}"
						@change="$v.email.$touch()"
					>
					<p class="error-text" v-if="!$v.email.required">Field is required</p>
					<p class="error-text" v-if="!$v.email.alpha">Invalid email address</p>
				</div>
				<button class="btn btnPrimary">Submit</button>
			</form>
		</div>

	</modal>
</template>

<script>
	import {required, sameAs, minLength, email, alpha,} from 'vuelidate/lib/validators';
	import modal from '@/components/UI/Modal';

	export default {
		name: "modal-validate",
		components: {modal},
		validations: {
			name: {required, alpha, minLength: minLength(2)},
			password: {required, minLength: minLength(6)},
			passwordRepeat: {required, sameAsPassword: sameAs('password')},
			email: {email, required}
		},
		data() {
			return {
				name: '',
				password: '',
				passwordRepeat: '',
				email: ''
			}
		},
		methods: {
			clearForm() {
				this.$v.name.$model = ''
				this.$v.password.$model = ''
				this.$v.passwordRepeat.$model = ''
				this.$v.email.$model = ''
				this.$v.$reset()
			},
			closeModal() {
				this.clearForm();
				this.$emit('modalClose');
			},
			onSubmit() {
				this.$v.$touch();
				if (!this.$v.$invalid) {
					const user = {
						name: this.$v.name.$model,
						password: this.$v.password.$model,
						email: this.$v.email.$model
					}
					console.log(user);
					this.closeModal();
				}
			}
		}
	}
</script>

<style lang="scss">

</style>