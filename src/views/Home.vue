<template>
	<Table @deleteUsers="deleteUsers" @editUsers="editUsers" ref="table" :usersData="Users.data" />
</template>

<script setup>
import axios from 'axios'
import { reactive, ref, onMounted } from 'vue'
import Table from '@/components/Table.vue'
const closeDeleteModal = ref(false)
const Users = reactive({
	data: []
})
const table = ref('table')
const getUserData = async () => {
	let res = await axios.get('https://jsonplaceholder.typicode.com/users')
	console.log(res.data);
	Users.data = res.data
}
const deleteUsers = (i) => {
	Users.data.splice(i, 1)
	table.value.closeDeleteModal()
}
const editUsers = (editUserData) => {
	Users.data.map((user) => {
		if (user.id === editUserData.id) {
			return user = editUserData
		}
		return user
	})
	table.value.closeEditModal()

}
onMounted(async () => {
	await getUserData()
})

</script>
