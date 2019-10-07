<template>
<div class="container">
    <div v-if="employees.length">
        <button class="btn btn-primary" @click="editMode">Edit</button>
        <template  v-if="edit">
            <div class="row">
                <img :src="`/images/employees/`+employee.profile_pic" height="180" width="180" alt="">
                <ul class="list-group">
                    <li class="list-group-item"><h4>{{employee.firstname + ' '+ employee.middleinitial + ' ' + employee.lastname}}</h4></li>
                    <li class="list-group-item"><h4>19</h4></li>
                    <li class="list-group-item"><h4>Manager</h4></li>
                </ul> 
            </div>
        </template>
        <template v-else>
            <h1>Editing Mode</h1>
            <div class="row">
                <img :src="`/images/employees/`+employee.profile_pic" height="180" width="180" alt="">
                <ul class="list-group">
                    <div class="form-row">
                        <div class="form-group col-md">
                        <input type="text" class="form-control" id="inputFirstname" placeholder="Firstname" v-model="employee.firstname">
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group col-md">
                        <input type="text" class="form-control" id="inputMiddleinitial" placeholder="Middle Initial" v-model="employee.middleinitial">
                        </div>
                    </div>

                    <div class="form-row">
                        <div class="form-group col-md">
                        <input type="text" class="form-control" id="inputLastname" placeholder="Lastname" v-model="employee.lastname">
                        </div>
                    </div>

                    <div class="form-row">
                        <select id="inputState" class="form-control" v-model="employee.position">
                            <option>Admin</option>
                            <option>Manager</option>
                            <option>Housewife</option>
                            <option>Maintenance</option>
                            <option>Chef</option>
                        </select>
                    </div>
                </ul> 
            </div>
        </template>
        <div>
            <h1>Facial Recognition Model</h1>
            <b-list-group horizontal>
                <b-list-group-item v-for="(image,index) in images" :key="index"> 
                    <img :src="image" height="120" width="120" alt="">
                </b-list-group-item>
            </b-list-group>
        </div>
    </div>
    <button class="btn btn-primary" @click="update">Save</button>
</div>

</template>
<script>
export default {
    data(){
        return{
            edit : true,
            id : 0,
            employee : '',
            image : '',
            images : [],
            day : ''
        }
    },
    computed : {
        employees(){
            return this.$store.getters.employees
        }
    },
    methods : {
        loadImages(){

            for(let i = 1; i <= 4; i++){
                var path = 'http://vuespatest.test/public/images/models/' + this.employee.id + '-' + this.employee.firstname + '/' + i + '.jpg'
                console.log(path)
                this.images.push(path)
            }
            
        },
        editMode(){
            if(this.edit){
                this.edit = false
            }else{
                this.edit = true
            }
            console.log(this.edit)
        },
        update(){
            axios.post('/api/auth/update',this.employee).then(response => {
                this.$router.push('/employee')
            })
        }
    },
    created(){
        if(!this.employees.length || this.id != 0) 
            this.$router.push('/employee')

       
        this.employee = this.employees.find(employee => employee.id == this.$route.params.id)
        this.loadImages()

        var d = new Date()
        this.day = d.getDay()
    },
}
</script>
<style scoped>

    .active-pink-4 input[type=text]:focus:not([readonly]) {
    border: 1px solid #f48fb1;
    box-shadow: 0 0 0 1px #f48fb1;
    }
    .active-pink-3 input[type=text] {
    border: 1px solid #f48fb1;
    box-shadow: 0 0 0 1px #f48fb1;
    }
    .active-purple-4 input[type=text]:focus:not([readonly]) {
    border: 1px solid #ce93d8;
    box-shadow: 0 0 0 1px #ce93d8;
    }
    .active-purple-3 input[type=text] {
    border: 1px solid #ce93d8;
    box-shadow: 0 0 0 1px #ce93d8;
    }
    .active-cyan-4 input[type=text]:focus:not([readonly]) {
    border: 1px solid #4dd0e1;
    box-shadow: 0 0 0 1px #4dd0e1;
    }
    .active-cyan-3 input[type=text] {
    border: 1px solid #4dd0e1;
    box-shadow: 0 0 0 1px #4dd0e1;
    }
</style>