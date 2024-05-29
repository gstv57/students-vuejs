<template>

    <div class="container-fluid mt-1">
        <div class="card">
            <div class="card-header">
                <h4>Students
                    <RouterLink class="btn btn-primary float-end" to="/students/create">Add Student</RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Course</th>
                            <th>Email</th>
                            <th>Phone</th>
                            <th>Created at</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="students.length > 0">
                        <tr v-for="(student, index) in this.students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ formatDate(student.created_at) }}</td>
                            <td>
                                <RouterLink class="btn btn-secondary"
                                    :to="{ path: '/students/' + student.id + '/edit' }" style="margin-right: 10px;">Edit
                                </RouterLink>
                                <button type="button" @click="deleteStudent(student.id)"
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
    name: "students",
    data() {
        return {
            students: []
        }
    },
    mounted() {
        this.getStudents();
    },
    methods: {
        getStudents() {
            axios.get('https://api-students.test/api/v1/students').then(res => {
                this.students = res.data;
            })
        },
        deleteStudent(id) {
            this.$swal({
                text: 'Are you sure you want to delete this student?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, delete it!',
                cancelButtonText: 'No, keep it'
            }).then((result) => {
                if (result.isConfirmed) {
                    axios.delete('https://api-students.test/api/v1/students/' + id + '/delete')
                        .then(res => {
                            this.$swal('Success', res.data.message, 'success');
                            this.getStudents();
                        })
                } else if (result.dismiss === Swal.DismissReason.cancel) {
                    this.$swal('Cancelled', 'Your imaginary file is safe :)', 'error');
                }
            })
            .catch(error => {
                this.$swal('Error', error.response.data.message, 'error');
            });
        },
        formatDate(dateString) {
            const options = {
                year: 'numeric',
                month: 'numeric',
                day: 'numeric'
            };
            return new Date(dateString).toLocaleDateString('pt-BR', options);
        },
    }
}
</script>