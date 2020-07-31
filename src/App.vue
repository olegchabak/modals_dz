<template>
	<div class="wrapper">
		<!--<header></header>-->
		<div class="wrapper-content">
			<section>
				<div class="container">
					<h1>{{title}}</h1>
					<br>

					<!--modal with Validate-->
					<button class="btn btnPrimary" @click="modalValidate = !modalValidate">Modal with validate</button>
					<modal-validate v-show="modalValidate" @modalClose="modalValidate=false"></modal-validate>

					<!--authorization-->
					<button class="btn btnPrimary" @click="authorization = !authorization">Authorization</button>
					<authorization v-show="authorization" @modalClose="authorization=false" @openModal="openModal"/>

					<!--registration-->
					<button class="btn btnPrimary" @click="registration = !registration">Registration</button>
					<registration v-show="registration" @modalClose="registration=false" @openModal="openModal"/>
				</div>
			</section>
		</div>
		<!--<footer></footer>-->
	</div>
</template>

<script>
	import modal from '@/components/UI/Modal'
	import modalValidate from '@/components/ModalValidate'
	import Authorization from '@/components/Authorization'
	import Registration from '@/components/Registration'

	export default {
		components: {modal, modalValidate, Authorization, Registration},
		data(){
			return {
				title: 'My modals',
				modalValidate: false,
				authorization: false,
				registration: false
			}
		},
		methods:{
			openModal(modalName) {
				console.log(modalName)
				this[modalName] = true;
			}
		}
	}
</script>

<style lang="scss">
	.wrapper-content {
		max-width: 1200px;
		margin: 60px auto;
	}
	.btn {
		margin-right: 15px;
	}
</style>
