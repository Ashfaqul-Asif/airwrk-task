<template>
	<v-container class="fill-height">
		<v-table>
			<thead>
				<!-- id, name, username, email, address, phone, and website. -->
				<tr>
					<th>id</th>
					<th>name</th>
					<th>username</th>
					<th>email</th>
					<th>address</th>
					<th>phone</th>
					<th>website</th>
					<th></th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(user,i) in props.usersData" :key="i">
					<td>{{user.id}}</td>
					<td>{{user.name}}</td>
					<td>{{user.username}}</td>
					<td>{{user.email}}</td>
					<td>{{user.phone}}</td>
					<td>{{user.website}}</td>
					<td>
						<v-btn icon @click="openAlertDialog(i)">
							<v-icon>mdi-delete</v-icon>
						</v-btn>
						<v-btn icon @click="openEditDialog(user)">
							<v-icon>mdi-pencil</v-icon>
						</v-btn>
					</td>
				</tr>
			</tbody>
		</v-table>
		<v-dialog max-width="420" v-model="openAlertModal">
			<v-card>
				<v-card-text>
					<p>Are you sure you want to delete the user?</p>
				</v-card-text>
				<v-card-actions>
					<v-btn @click="closeDeleteModal">close</v-btn>
					<v-btn @click="onClickConfirmDelete(i)">confirm</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
		<v-dialog max-width="420" v-model="openEditModal">
			<v-card>
				<v-card-text>
					<v-form @submit.prevent="onEdit(i)">
						<v-row>
							<v-col cols="12" md="6">
								<v-text-field v-model="editUser.name" label="Name"></v-text-field>
							</v-col>

							<v-col cols="12" md="6">
								<v-text-field v-model="editUser.name" label="Email"></v-text-field>
							</v-col>

							<v-col cols="12" md="6">
								<v-text-field v-model="editUser.phone" label="Phone"></v-text-field>
							</v-col>
							<v-col cols="12" md="6">
								<v-text-field v-model="editUser.website" label="Website"></v-text-field>
							</v-col>
						</v-row>
					</v-form>
				</v-card-text>
				<v-card-actions>
					<v-btn @click="closeEditModal">close</v-btn>
					<v-btn @click="onClickConfirmEdit()">confirm</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
	</v-container>
</template>

<script setup>
import { ref, watch, defineExpose } from 'vue'
const openAlertModal = ref(false)
const openEditModal = ref(false)
const deleteId = ref(null)
const editUser = ref(null)
const emit = defineEmits(['deleteUsers', 'editUsers'])
const openAlertDialog = (i) => {
	openAlertModal.value = true
	deleteId.value = i
}
const onClickConfirmDelete = (i) => {
	console.log(i);
	emit('deleteUsers', deleteId.value)
}
const onClickConfirmEdit = () => {
	emit('editUsers', editUser.value)
}
const openEditDialog = (user) => {
	editUser.value = user
	openEditModal.value = true
}
const closeDeleteModal = () => {
	openAlertModal.value = false
}
const closeEditModal = () => {
	openEditModal.value = false
}
defineExpose({ closeDeleteModal, closeEditModal })
const props = defineProps({
	usersData: {
		type: Array,
		default: null
	},
})

  //
</script>
