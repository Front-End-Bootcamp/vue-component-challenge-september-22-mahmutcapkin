<script setup>
import { ref } from "vue";
import Student from "@/components/StudentComponent/Student.vue";
import fetchData from "@/assets/scripts/fetch.data";
import filterByGroups from "@/assets/scripts/option.three";

const props = defineProps(["group"]);
const studentGroups = ref([]);
const isShow = ref(false);

async function getStudentsOnClick(groupName) {
	const requestData = await fetchData();
	const groups = await filterByGroups(requestData);
	studentGroups.value = groups[groupName];
	isShow.value = !isShow.value;
}
</script>

<template>
	<button class="primary" @click="getStudentsOnClick(props.group)">
		{{ props.group }}
	</button>
	<table v-if="isShow" class="table">
		<caption>
			List of Students
		</caption>
		<thead>
			<tr>
				<th scope="col">id</th>
				<th scope="col">Name</th>
			</tr>
		</thead>
		<tbody>
			<Student
				v-for="student in studentGroups"
				:key="student.id"
				:student="student"
			></Student>
		</tbody>
	</table>
</template>

<style scoped>
.primary {
	background-color: aqua;
	text-decoration: solid;
	display: block;
	margin: 5px;
}
.table {
	border: 1px solid red;
}
</style>
