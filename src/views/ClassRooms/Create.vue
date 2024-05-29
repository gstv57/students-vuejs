<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Create Class Room</h4>
            </div>
            <div class="card-body">
                <ul v-if="Object.keys(errorList).length > 0" class="alert alert-warning">
                    <li v-for="(error, index) in errorList" :key="index" class="mb-0 ms-3">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" class="form-control" v-model="model.classRoom.name">
                </div>
                <div class="mb-3">
                    <select class="form-select" v-model="model.classRoom.teacher_id"
                        aria-label="Default select example">
                        <option value="" selected>Select one teacher</option>
                        <option v-for="(teacher, index) in model.teachers" :key="index" :value="teacher.id">{{
                            teacher.name }}</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label for="">Student Capacity</label>
                    <input type="tel" class="form-control" v-model="model.classRoom.student_capacity">
                </div>
                <div class="mb-3">
                    <label for="">Description</label>
                    <input type="text" class="form-control" v-model="model.classRoom.description">
                </div>
                <div class="mb-3">
                    <label for="">Status</label>
                    <select class="form-select" v-model="model.classRoom.status" aria-label="Default select example">
                        <option value="" selected>Select one status</option>
                        <option v-for="(status, index) in model.statusEnum" :key="index" :value="status">{{ status }}
                        </option>
                    </select>
                </div>
                <div class="mb-3">
                    <button type="button" @click="saveClassRoom" class="btn btn-primary">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "classRoomCreate",
    data() {
        return {
            errorList: [],
            model: {
                classRoom: {
                    name: '',
                    teacher_id: '',
                    student_capacity: '',
                    description: '',
                    status: '',
                }
            },
            statusEnum: []
        }
    },
    mounted() {
        this.getStatusEnum();
        this.getTeachers();
    },
    methods: {
        saveClassRoom() {
            let mythis = this;
            axios.post('https://api-students.test/api/v1/class-rooms', this.model.classRoom)
                .then(response => {
                    console.log(response.data)
                    this.model.classRoom = {
                        name: '',
                        student_capacity: '',
                        description: '',
                        status: '',
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
        getTeachers() {
            axios.get('https://api-students.test/api/v1/teachers')
                .then(response => {
                    this.model.teachers = response.data;
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
        getStatusEnum() {
            axios.get('https://api-students.test/api/v1/class-rooms/enums')
                .then(response => {
                    this.model.statusEnum = response.data.data.status;
                    this.errorList = [];
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