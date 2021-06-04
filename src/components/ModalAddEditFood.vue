<template>
	<q-card class="form-card">
    <q-card-section>
      <div class="text-h6 heading">{{ type }} Food</div>
    </q-card-section>

	<form @submit.prevent="submitForm">
    <q-card-section>

    	<div class="row q-mb-md">
	      <q-input
	      	filled
	      	v-model="foodToSubmit.name"
	      	label="Name (e.g. Burger)"
	      	class="col"
			:rules="[(val => !!val || 'Field is required'), 
					(val => !(val.length > 20) || 'Name length can only be 20')]"
			ref="name" />
    	</div>

    	<div class="row q-mb-md">
    		<q-input
		      filled
		      v-model="foodToSubmit.description"
		      label="Description"
		      type="textarea"
			  :rules="[ (val => !(val.length > 135) || 'Description cannot be longer than 135 chars')]"
			  ref="description"
		      class="col" />
    	</div>

    	<div class="row q-mb-md">
	      <q-input
	      	filled
	      	v-model="foodToSubmit.imageUrl"
	      	label="Image URL"
	      	class="col" />
	      <q-img
          :src="foodToSubmit.imageUrl ? foodToSubmit.imageUrl : 'statics/image-placeholder.png'"
          class="q-ml-sm"
          contain />
    	</div>

    	<div class="q-mb-md">
    		<div class="row">
					<p class="q-mb-none">Rating:</p>
    		</div>
				<div class="row">
	    		<q-rating
			      v-model="foodToSubmit.rating"
			      size="2em"
			      color="orange" />
				</div>
    	</div>
    	
    </q-card-section>

    <q-card-actions align="right">
      <q-btn
      	label="Cancel"
      	color="grey"
      	v-close-popup />
      <q-btn
      	label="Save"
      	color="primary"
		type="submit" />
    </q-card-actions>
	</form>
  </q-card>
</template>

<script>
	import { mapActions } from 'vuex'

	export default {
		props: ['type', 'food', 'id'],
		data() {
			return {
				foodToSubmit: {
					name: '',
					description: '',
					rating: 1,
					imageUrl: ''
				}
			}
		},
		methods: {
			...mapActions('storeFoods',['addFood','editFood']),
			submitForm() {
				this.$refs.name.validate();
				this.$refs.description.validate();
				if(!this.$refs.name.hasError && !this.$refs.description.hasError){
					this.submitFood();
				}
			},
			submitFood() {
				if(this.type==='add'){
					this.addFood(this.foodToSubmit);
				}else{
					this.editFood({id: this.id, updates: this.foodToSubmit});
				}
				this.$emit('close');
			}
		},
		mounted() {
			if(this.type==='edit'){
				this.foodToSubmit = Object.assign({},this.food);
			}
		}
	}
</script>

<style>
	.form-card {
		min-width: 400px;
	}
	.form-card .heading {
		text-transform: capitalize;
	}
	.form-card .q-card-section {
		width: 100%;
	}
	.thumbnail {
		max-width: 50px;
		max-height: 50px;
	}
	.form-card .q-img {
		height: 56px;
		width: 56px;
		border-radius: 10px;
	}
	.form-card .q-img__image {
		background-size: cover !important;
	}
	.form-card .q-rating__icon {
		opacity: 0.2;
	}
	.form-card .q-rating__icon--active {
		opacity: 1;
	}
</style>