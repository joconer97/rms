<template>
    <div style="margin-bottom : 10px">
        <div class="row" style="background:red;margin:0 auto">
            <input type="text" placeholder="Employee #" v-model="employeeID">
            <button class="btn btn-primary" @click="switchCamera">Enter</button>
        </div>

        <div class="row" style="background:green;height:600px">
            <div class="col-md-8" style="background:blue">
                <template v-if="isOn">
                    <attendance-camera :employee="employee"></attendance-camera>
                </template>
                <template v-else>
                    <div>
                        Waiting...
                    </div>
                </template>
            </div>

            <div class="col-md-4">
                <table>
                    <tr v-for="history in attendanceHistory" :key="history.id">
                        <td><img :src="`/images/employees/`+history.profile_pic" height="180" width="180" alt=""></td>
                        <td>{{history.firstname}}</td>
                        <td>{{history.lastname}}</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</template>
<script>
import AttendanceCamera from './AttendanceCamera'
import {bus} from '../../main.js'
export default {
    data(){
        return {
            employeeID : undefined,
            isOn : false,
            attendanceHistory : [],
            employee : null
        }
    },
    components : {AttendanceCamera},
    methods : {
        switchCamera(){
            if(this.employeeID == undefined) return
            if(this.isOn){
                this.isOn = false
            }else{
                this.employee = this.getEmployee(this.employeeID)
                console.log(this.employee)
                this.isOn = true
                this.employeeID = undefined
            }
        },
        getEmployee(id){
            let index = this.employees.findIndex(employee => employee.id == id)

            return this.employees[index]
        },
    },
    computed : {
        employees () {
            return this.$store.getters.employees
        }
    },
    mounted(){
        if (this.employees.length)
            return

        this.$store.dispatch('getEmployee')
        
    },
    created(){
        bus.$on('testing', (data) => {
            var today = new Date();
            var time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            var attendance = {user_id : data.id,attendance : time}
            console.log(attendance)
            axios.post('/api/attendance',attendance)
                .then(response => {
                    console.log(response)
                })
            this.attendanceHistory.push(data)
            this.isOn = false
        })
    }
}
</script>
