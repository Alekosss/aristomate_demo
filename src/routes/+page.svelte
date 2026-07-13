<script>
	import { universisFetch } from '$lib/universis_fetch';

	let courses = [];

	async function getGrades() {
		const result = await universisFetch("/students/me/courses"); 

		courses = result.value; 
		console.log(courses);

	}

	let section = "notes";

	let tasks = [];
	let newtask;


</script>



<ion-header>
	<ion-toolbar>
		<ion-title>Universis</ion-title>
	</ion-toolbar>
	<ion-segment on:ionChange={(event) => section = event.detail.value}>
		<ion-segment-button value="grades">
			<ion-label>Grades</ion-label>
		</ion-segment-button>
		<ion-segment-button value="notes">
			<ion-label>Notes</ion-label>
		</ion-segment-button>
</ion-header>

<ion-content>
	{#if section === 'grades'}
		<ion-card>
			<p>Here are my grades...</p>
			<ion-button on:click = {() => getGrades()}>Load my courses...</ion-button>
		</ion-card>

		{#if courses.length === 0}
		<ion-card>
			No courses found!
		</ion-card>
		{:else}
			<ion-list>
				{#each courses as course}
				<ion-item>
					<ion-label><h2>{course.courseTitle}</h2></ion-label>
					{#if course.formattedGrade}
						<ion-text>Grade: {course.formattedGrade}</ion-text>
					{:else}
						<ion-text>Grade: Not available</ion-text>
					{/if}
				</ion-item>
				{/each}
			</ion-list>
		{/if}






	{:else if section === 'notes'}
	<ion-card>
		<ion-card-header>Todo List</ion-card-header>
		<ion-input 
			placeholder="Add a task"
			label="Title"
			value={newtask}
			on:ionInput={ (event) => {newtask = event.detail.value}	}
		>
		</ion-input>

		<ion-button on:click={() => { tasks = [...tasks,newtask]; } }>add</ion-button>
		
		<ion-list>

			{#each tasks as task} 
				<ion-item>
					<ion-label>{task}</ion-label>
				</ion-item>
			{/each}
		</ion-list>
	</ion-card>
	{/if}

</ion-content>

<ion-footer>
	<ion-toolbar>
		<ion-title>This is a footer!</ion-title>
	</ion-toolbar>
</ion-footer>