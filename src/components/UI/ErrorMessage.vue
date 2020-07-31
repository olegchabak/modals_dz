<template>
	<div class="error-text">
		<p v-if="error.required">Field is required</p>
		<p v-if="error.minLength">The field must contain at least {{error.minLength.min}} characters</p>
		<p v-if="error.sameAsPassword">Passwords don't match</p>
		<p v-if="error.alpha">The field must contain only letters</p>
		<p v-if="error.email">Invalid email</p>
	</div>

</template>

<script>
	export default {
		name: "error-field",
		props: {
			error: {
				type: Object,
				default: null
			}
		}
	}
</script>

<style lang="scss">
	.error-text {
		font-size: 13px;
		display: none;
		color: red;
		.form-item--error & {
			display: block;
		}
	}

</style>