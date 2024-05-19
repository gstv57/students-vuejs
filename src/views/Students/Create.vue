<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Create Student</h4>
            </div>
            <div class="card-body">
                <ul v-if="Object.keys(errorList).length > 0" class="alert alert-warning">
                    <li v-for="(error, index) in errorList" :key="index" class="mb-0 ms-3">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" class="form-control" v-model="model.student.name" </div>
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
                        <button type="button" @click="saveStudent" class="btn btn-primary">Save</button>
                    </div>
                </div>
            </div>
        </div>
</template>
<script>
import axios from 'axios';
export default {
    name: "studentCreate",
    data() {
        return {
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

    methods: {
        saveStudent() {
            let mythis = this;
            axios.post('https://api-students.test/api/v1/students', this.model.student)
                .then(response => {
                    console.log(response.data)
                    this.model.student = {
                        name: '',
                        course: '',
                        email: '',
                        phone: ''
                    }
                    this.errorList = [];
                    this.$swal('Success', response.data.message, 'success');
                })
                .catch(function (error) {
                    if (error.response) {
                        if (error.response.status === 422) {
                            mythis.errorList = error.response.data.errors;
                        }

                        console.log(error.response.data)
                        console.log(error.response.headers)
                    } else if (error.request) {
                        console.log(error.request)
                    } else {
                        console.log('Error', error.message)
                    }
                })
        }
    }
}
</script>