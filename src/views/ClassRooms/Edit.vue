<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit Class Room</h4>
            </div>
            <div class="card-body">
                <!-- <ul v-if="Object.keys(errorList).length > 0" class="alert alert-warning">
                    <li v-for="(error, index) in errorList" :key="index" class="mb-0 ms-3">
                        {{ error[0] }}
                    </li>
                </ul> -->
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" class="form-control" v-model="model.classRoom.name">
                    <div class="mb-3">
                        <label for="">Teacher</label>
                        <input type="email" class="form-control" v-model="model.classRoom.teacher.name">
                    </div>
                    <div class="mb-3">
                        <label for="">Student Capacity</label>
                        <input type="text" class="form-control" v-model="model.classRoom.student_capacity">
                    </div>
                    <div class="mb-3">
                        <label for="">Description</label>
                        <input type="text" class="form-control" v-model="model.classRoom.description">
                    </div>
                    <div class="mb-3">
                        <label for="">Status</label>
                        <select class="form-select" v-model="model.classRoom.status"
                            aria-label="Default select example">
                            <option value="" selected>Select one status</option>
                            <option v-for="(status, index) in classEnum" :key="index" :value="status">{{ status }}</option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <button type="button" @click="" class="btn btn-primary">Update</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "classRoomsEdit",
    data() {
        return {
            classRoomId: "",
            errorList: [],
            model: {
                classRoom: {
                    name: "",
                    student_capacity: "",
                    description: "",
                    status: "",
                    created_at: "",
                    updated_at: "",
                    teacher: {
                        name: "",
                    }
                }
            },
            classEnum: [],
        }
    },
    mounted() {
        this.classRoomId = this.$route.params.id;
        this.getClassRoom(this.$route.params.id);
        this.getClassEnum();
    },
    methods: {
        getClassRoom(id) {
            axios.get(`https://api-students.test/api/v1/class-rooms/${id}/edit`)
                .then(response => {
                    this.model.classRoom = response.data.data;
                })
                .catch(error => {
                    console.log(error);
                });
        },
        getClassEnum() {
            axios.get(`https://api-students.test/api/v1/class-rooms/enums`)
                .then(response => {
                    this.classEnum = response.data.data.status;
                })
                .catch(error => {
                    console.log(error);
                });
        },
    }
}
</script>
