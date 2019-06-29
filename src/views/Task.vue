<template>
    	<v-layout row>
      
			<v-flex 
				xs12
				v-if="task"
			>

				<h1>{{ task.title }}</h1>

				<v-form
			    	ref="form"
			    	v-model="valid"
			    	lazy-validation
			  	>

					<v-chip label color="error" text-color="white" v-for="tag of task.tags" :key="tag">
				    	<v-icon left>label</v-icon>{{tag}}
				    </v-chip>
					
					<v-textarea
						label="Text"
						rows="1"
						required
						:rules="[v => !!v || 'Text is required']"
						auto-grow
						:value="task.text"
						v-model="text"
				    ></v-textarea>
					
					<v-menu
				        v-model="menu"
				        :close-on-content-click="false"
				        :nudge-right="40"
				        lazy
				        transition="scale-transition"
				        offset-y
				        full-width
				        min-width="290px"
				    >
				        <template v-slot:activator="{ on }">
					        <v-text-field
					            readonly
					            v-on="on"
					            v-model="task.date"
					        ></v-text-field>
					    </template>
					    <v-date-picker v-model="task.date" @input="menu = false"></v-date-picker>
					</v-menu>

				    <v-spacer></v-spacer>
				    <div v-if="task.status !== 'completed'">
				    	
				    	<v-btn
				      	  	:loading="loading"
				      	  	:disabled="!valid || loading"
				      	  	color="success"
				      	  	@click="updateTask">
				      	  	Update
				      	</v-btn>
				      	<v-btn
				      	  	:loading="loading"
				      	  	:disabled="!valid || loading"
				      	  	color="success"
				      	  	@click="completeTask">
				      	  	Complete
				      	</v-btn>

				    </div>
			      	

				</v-form>
			</v-flex>
			<v-flex 
				xs12
				v-else
			>
			  <p>Task not found</p>
			</v-flex>
    	</v-layout>
</template>

<script>
	export default {
		data () {
			return {
				valid: false,
			    menu: false,
			    text: ''
			}
		},
		computed: {
			task() {
				return this.$store.getters.taskById(+this.$route.params.id)
			},
			loading () {
				return this.$store.getters.loading
			}
		},
		methods: {
			updateTask() {
				this.$store.dispatch('updateTask', {
					id: this.task.id,
					date: this.task.date,
					text: this.text
				}).then(() => {
							this.$router.push('/tasks')
						})
						.catch(() => {})
			},
			completeTask() {
				this.$store.dispatch('completeTask', this.task.id)
					.then(() => {
							this.$router.push('/tasks')
						})
					.catch(() => {})
			}
		}
	}
</script>