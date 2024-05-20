<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit Teacher</h4>
            </div>
            <div class="card-body">
                <div class="mb-3 text-center">
                    <img :src="profilePicture" alt="Profile Picture" class="rounded-circle" style="width: 100px; height: 100px;">
                </div>
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
                        <label for="">Phone</label>
                        <input type="text" class="form-control" v-model="model.teacher.phone">
                    </div>
                    <div class="mb-3">
                        <label for="">Matery</label>
                        <input type="text" class="form-control" v-model="model.teacher.matery">
                    </div>
                    <div class="mb-3">
                        <button type="button" @click="updateTeacher" class="btn btn-primary">Update</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>

import axios from 'axios';
export default {
    name: "teacherEdit",
    data() {
        return {
            teacherId: '',
            errorList: [],
            model: {
                teacher: {
                    name: '',
                    matery: '',
                    email: '',
                    phone: '',
                    photos: [
                        {
                            url: '',
                        }
                    ]
                }
            }
        }
    },
    mounted() {
        this.teacherId = this.$route.params.id;
        this.getTeacherData(this.$route.params.id);
    },
    computed: {
        profilePicture() {
            return this.model.teacher.photos.length > 0 ? this.model.teacher.photos[0].url : '';
        }
    },
    methods: {
        getTeacherData(teacherId) {
            axios.get(`https://api-students.test/api/v1/teachers/${teacherId}/edit`)
                .then(res => {
                    this.model.teacher = res.data;
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
        updateTeacher() {
            let mythis = this;
            axios.put(`https://api-students.test/api/v1/teachers/${this.teacherId}/edit`, this.model.teacher)
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