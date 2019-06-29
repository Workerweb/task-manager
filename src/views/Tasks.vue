<template>
    
    <v-layout row>

    	<v-flex xs12 sm10 offset-sm1>

			<h1 style="margin-bottom: 25px">Task list</h1>
			<v-flex xs3 offset-xs9>
				<v-select
		        	v-model="select"
		        	:items="statusItems"
		        	label="Filter by status"
		        	single-line
		        ></v-select>
			</v-flex>
			<table v-if="tasks.length">
				<thead>
					<tr>
						<th style="padding-left: 25px">Title</th>
						<th class="hidden-sm-and-down">Date</th>
						<th class="hidden-sm-and-down">Text</th>
						<th style="padding-left: 10px">Status</th>
					</tr>
				</thead>
				<tbody>
					<tr 
						v-for="task of showTasks"
						:key="task.id">
						<td><v-btn 
								depressed 
								flat 
								color="error"
								:to="'/task/' + task.id">{{ task.title }}
							</v-btn></td>
						<td class="hidden-sm-and-down">{{ task.date }}</td>
						<td class="task-text hidden-sm-and-down">{{ task.text }}</td>
						<td style="padding-left: 10px">{{ task.status }}</td>	
					</tr>
				</tbody>
			</table>
			<p v-else>No tasks</p>

    	</v-flex>
      
    </v-layout>

</template>

<script>
	export default {
		data () {
	    	return {
		      	select: null,
		      	statusItems: [ 'all', 'completed', 'active', 'outdated' ]
	    	}
	    },
		computed: {
			tasks() {
				return this.$store.getters.tasks
			},
			showTasks() {
				return this.tasks.filter(t => {
					if (!this.select || this.select === 'all') {
						return true
					}
					return t.status === this.select
				})
			}
		}
	}
</script>

<style>
	table {
		width: 100%;
		text-align: left;
	}
	.task-text {
		max-width: 250px;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}
</style>
