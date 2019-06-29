<template>
  <v-container>
	<v-layout row>
	  	
	  	<v-flex xs12 sm6 offset-sm3>

		  	<v-form
		    	ref="form"
		    	v-model="valid"
		    	lazy-validation
		  	>

			    <v-text-field
			    	label="Task title"
			    	required
			    	v-model="title"
			    	:rules="[v => !!v || 'Title is required']"
			    ></v-text-field>

				<v-combobox
					multiple
                    label="Tags" 
                    append-icon
                    chips
                    deletable-chips
                    class="tag-input"
                    v-model="chips">
          		</v-combobox>
				
				<v-textarea
					label="Text"
					counter="2048"
					rows="1"
					required
					v-model="text"
					:rules="[v => !!v || 'Text is required']"
					auto-grow
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
				            v-model="date"
				            readonly
				            v-on="on"
				        ></v-text-field>
				    </template>
				    <v-date-picker v-model="date" @input="menu = false"></v-date-picker>
				</v-menu>

			    <v-spacer></v-spacer>
		      	<v-btn
		      	  	:loading="loading"
		      	  	:disabled="!valid || loading"
		      	  	color="success"
		      	  	@click="createTask">
		      	  	Create task
		      	</v-btn>

			</v-form>

		</v-flex>

	</v-layout>
    

  </v-container>
</template>

<script>

	export default {
		data () {
			return {
				valid: false,
				title: '',
				text: '',
				chips: null,
				date: new Date().toISOString().substr(0, 10),
			    menu: false,
			}
		},
		computed: {
			loading () {
				return this.$store.getters.loading
			}
		},
		methods: {
			createTask () {
				if (this.$refs.form.validate()) {
					const task = {
						title: this.title,
						text: this.text,
						id: Date.now(),
						tags: this.chips,
						date: this.date,
						status: 'active'

					}
					this.$store.dispatch('createTask', task)
						.then(() => {
							this.$router.push('/tasks')
						})
						.catch(() => {})
				}
			}
		}
	}
	
</script>

<style lang="scss">
	.tag-input span.chip {
	  background-color: teal;
	  color: #fff;
	  font-size: 1em;
	}

	.tag-input span.v-chip {
	  background-color: teal;
	  color: #fff;
	  font-size:1em;
	  padding-left:7px;
	}

	.tag-input span.v-chip::before {
	    content: "label";
	    font-family: 'Material Icons';
	    font-weight: normal;
	    font-style: normal;
	    font-size: 20px;
	    line-height: 1;
	    letter-spacing: normal;
	    text-transform: none;
	    display: inline-block;
	    white-space: nowrap;
	    word-wrap: normal;
	    direction: ltr;
	    -webkit-font-feature-settings: 'liga';
	    -webkit-font-smoothing: antialiased;
	}
</style>
