<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit Student</h4>
            </div>
            <div class="card-body">
                <ul v-if="Object.keys(errorList).length > 0" class="alert alert-warning">
                    <li v-for="(error, index) in errorList" :key="index" class="mb-0 ms-3">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" class="form-control" v-model="model.student.name">
                    <div class="mb-3">
                        <label for="">Course</label>
                        <input type="text" class="form-control" v-model="model.student.course">
                    </div>
                    <div class="mb-3">
                        <label for="">Email</label>
                        <input type="email" class="form-control" v-model="model.student.email">
                    </div>
                    <div class="mb-3">
                        <label for="">Phone</label>
                        <input type="text" class="form-control" v-model="model.student.phone">
                    </div>
                    <div class="mb-3">
                        <button type="button" @click="updateStudent" class="btn btn-primary">Update</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>

import axios from 'axios';
export default {
    name: "studentEdit",
    data() {
        return {
            studentId: '',
            errorList: [],
            model: {
                student: {
                    name: '',
                    course: '',
                    email: '',
                    phone: ''
                }
            }
        }
    },
    mounted() {
        this.studentId = this.$route.params.id;
        this.getStudentData(this.$route.params.id);
    },
    methods: {
        getStudentData(studentId) {
            axios.get(`https://api-students.test/api/v1/students/${studentId}/edit`)
                .then(res => {
                    this.model.student = res.data;
                })
                .catch(function (error) {
                    if (error.response) {
                        if (error.response.status === 404) {
                            alert(error.response.data.message);
                        }
                        console.log(error.response.data)
                        console.log(error.response.headers)
                    } else if (error.request) {
                        console.log(error.request)
                    } else {
                        console.log('Error', error.message)
                    }
                });
        },
        updateStudent() {
            let mythis = this;
            axios.put(`https://api-students.test/api/v1/students/${this.studentId}/edit`, this.model.student)
                .then(response => {
                    this.errorList = [];
                    this.$swal('Success', response.data.message, 'success');
                })
                .catch(function (error) {
                    if (error.response) {
                        if (error.response.status === 422) {
                            mythis.errorList = error.response.data.errors;
                        }
                    } else if (error.request) {
                        if (error.request.status === 404) {
                            alert(error.request.data.errors)
                        }
                    } else {
                        console.log('Error', error.message)
                    }
                })
        },
    }
}
</script>