<template>
    <div class="container-fluid mt-1">
        <div class="card">
            <div class="card-header">
                <h4>Class Rooms
                    <RouterLink class="btn btn-primary float-end" to="/class-rooms/create">Add Class Room</RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Teacher</th>
                            <th>Student Capacity</th>
                            <th>Description</th>
                            <th>Status</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="class_roooms.length > 0">
                        <tr v-for="(class_room, index) in this.class_roooms" :key="index">
                            <td>{{ class_room.id }}</td>
                            <td>{{ class_room.name }}</td>
                            <td>{{ class_room.teacher.name }}</td>
                            <td>{{ class_room.student_capacity }}</td>
                            <td>{{ class_room.description }}</td>
                            <td>{{ class_room.status }}</td>
                            <td>
                                <RouterLink class="btn btn-secondary"
                                    :to="{ path: '/class_rooms/' + class_room.id + '/edit' }"
                                    style="margin-right: 10px;">Edit
                                </RouterLink>
                                <button type="button" @click="deleteClassRoom(class_room.id)"
                                    class="btn btn-danger mt-1">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                    <tbody v-else>
                        <tr>
                            <td colspan="7" class="text-center">Loading...</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "class_rooms",
    data() {
        return {
            class_roooms: []
        }
    },
    created() {
        this.getClassRooms();
    },
    methods: {
        getClassRooms() {
            axios.get('https://api-students.test/api/v1/class-rooms')
                .then(response => {
                    this.class_roooms = response.data.map(room => {
                        return {
                            ...room,
                            teacher: room.teacher
                        }
                    });
                })
                .catch(error => {
                    console.log(error);
                });
        },
        deleteClassRoom(id) {
            this.$swal({
                text: 'Are you sure you want to delete this student?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, delete it!',
                cancelButtonText: 'No, keep it'
            }).then((result) => {
                if (result.isConfirmed) {
                    axios.delete('https://api-students.test/api/v1/class-rooms/' + id + '/delete')
                        .then(response => {
                            this.getClassRooms();
                            this.$swal({
                                title: 'Success',
                                text: 'Class Room deleted successfully',
                                icon: 'success'
                            });
                        })
                        .catch(error => {
                            console.log(error);
                        });
                }
            });
        }
    }
}
</script>