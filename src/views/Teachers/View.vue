<template>
    <div class="container-fluid mt-1">
        <div class="card">
            <div class="card-header">
                <h4>Teachers
                    <RouterLink class="btn btn-primary float-end" to="/teachers/create">Add Teacher</RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Email</th>
                            <th>Scholl Matery</th>
                            <th>Phone</th>
                            <th>Created at</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="teachers.length > 0">
                        <tr v-for="(teacher, index) in this.teachers" :key="index">
                            <td>{{ teacher.id }}</td>
                            <td>{{ teacher.name }}</td>
                            <td>{{ teacher.email }}</td>
                            <td>{{ teacher.matery }}</td>
                            <td>{{ teacher.phone }}</td>
                            <td>{{ teacher.created_at }}</td>
                            <td>
                                <RouterLink class="btn btn-secondary"
                                    :to="{ path: '/teachers/' + teacher.id + '/edit' }" style="margin-right: 10px;">Edit
                                </RouterLink>
                                <button type="button" @click="deleteTeacher(teacher.id)"
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
    name: "teachers",
    data() {
        return {
            teachers: []
        }
    },
    mounted() {
        this.getTeachers();
    },
    methods: {
        getTeachers() {
            axios.get('https://api-students.test/api/v1/teachers')
                .then(response => {
                    this.teachers = response.data;
                })
                .catch(error => {
                    console.log(error);
                });
        },
        deleteTeacher(id) {
            this.$swal({
                title: 'Are you sure?',
                text: 'Once deleted, you will not be able to recover this teacher!',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, delete it!',
                cancelButtonText: 'No, cancel!',
                buttons: true,
                dangerMode: true,
            }).then((willDelete) => {
                if (willDelete) {
                    axios.delete('https://api-students.test/api/v1/teachers/' + id + '/delete')
                        .then(response => {
                            this.getTeachers();
                            this.$swal('Success', response.data.message, 'success');
                        })
                        .catch(error => {
                            console.log(error);
                        });
                } else {
                    this.$swal('Your teacher is safe!');
                }
            })
        }
    }
}
</script>