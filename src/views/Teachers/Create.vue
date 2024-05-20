<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Create Teacher</h4>
            </div>

            <div class="card-body">
                <ul v-if="Object.keys(errorList).length > 0" class="alert alert-warning">
                    <li v-for="(error, index) in errorList" :key="index" class="mb-0 ms-3">
                        {{ error[0] }}
                    </li>
                </ul>

                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" class="form-control" v-model="model.teacher.name">
                    <div class="mb-3">
                        <label for="">Email</label>
                        <input type="email" class="form-control" v-model="model.teacher.email">
                    </div>
                    <div class="mb-3">
                        <label for="">Scholl Matery</label>
                        <input type="text" class="form-control" v-model="model.teacher.matery">
                    </div>
                    <div class="mb-3">
                        <label for="">Phone</label>
                        <input type="tel" class="form-control" v-model="model.teacher.phone">
                    </div>
                    <div class="mb-3">
                        <button type="button" @click="saveTeacher" class="btn btn-primary">Save</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "teacherCreate",
    data() {
        return {
            errorList: [],
            model: {
                teacher: {
                    name: '',
                    email: '',
                    school_mastery: '',
                    phone: ''
                }
            }
        }
    },
    methods: {
        saveTeacher() {
            let mythis = this;
            axios.post('https://api-students.test/api/v1/teachers', this.model.teacher)
                .then(response => {
                    console.log(response)

                    this.model.teacher = {
                        name: '',
                        email: '',
                        school_mastery: '',
                        phone: '',
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
        },
    }
}
</script>