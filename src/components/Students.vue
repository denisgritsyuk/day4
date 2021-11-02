<template>
    <div>  
        <table>
                <tr>
                    <th>Photo</th>
                    <th>Name</th>
                    <th>Group</th>
                    <th>Mark</th>
                    <th>PR is Done</th>
                    
                </tr>
                <tr v-for="stud in students" v-bind:key="stud._id" >
                    <tr v-for="stud in students" v-bind:key="stud._id">
                    <td><img v-bind:src="stud.photo" width="50px"></td>
                    <td v-bind:style="stud.name.indexOf(search)>-1 && search.length >0 ? 'background-color:#CA2C2C' : 'background-color:#fff'" ><router-link v-bind:to="'/student-info/'+stud._id">
{{stud.name}}
</router-link></td>

                        <td>{{stud.group}}</td>
                    <td>{{stud.mark}}</td>
                    <td><input type="checkbox" v-bind:checked="stud.isDonePr"></td>
                    <td><a href="#" v-on:click="deleteStudent(stud._id)">Видалити</a></td>
                    <td><button v-on:click="getData(stud._id,stud.name,stud.group,stud.isDonePr)"><img src="components/Pencil.svg.png" width="25px"></button></td>
                </tr>
            </table><h1>{{reload}}</h1>
            <p><h3>Add new student</h3>
            <br>Input Name: <input v-model="name">
            <br> Input Group:
             <select name="group" v-model="group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Pr Is Done: <input type="checkbox" v-model="isDonePr">
            <br><button v-on:click="addStudent">Add student</button>

            <p><h3>Update student</h3>
            <br>Input Name: <input v-model="newStudent.name">
            <br> Input Group: 
            <select name="group" v-model="newStudent.group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Pr Is Done: <input type="checkbox" v-model="newStudent.isDonePr">
            <br><button v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'">Update student</button>

             

             
   </div>
   
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
   export default {
        data: function() {
           return {
            name:"",
            group:"",
            isDonePr:false,
            showInput:false,
            students: [],
            newStudent: {},
            
            reload:"",
        }},
        mounted: function(){
            
                axios.get("http://46.101.212.195:3000/students").then((response)=>{
                    console.log(response.data);
                    this.students = response.data;
                })
                axios.get("https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5").then((response)=>{
                    console.log(response.data);
                    this.currency = response.data;
                })
            
        },
        methods:{
        
        addStudent:function(){
            Vue.axios.post("http://46.101.212.195:3000/students", {
                name: this.name,
                group: this.group,
                isDonePr: this.isDonePr,
            })
            .then((response) => {
                console.log(response.data)
            })
            this.students.push({photo:this.students[1].photo,mark:0,name: this.name, group: this.group, isDonePr:this.isDonePr});
            this.reload = "";
             
        },
        deleteStudent:function(id){
            Vue.axios.delete("http://46.101.212.195:3000/students/"+id, {
            })
            this.reload = "";
            let i = this.students.indexOf(id);
            this.students.splice(i,1)
        },
        getData: function(id,name,group,isDone){
            this.newStudent.id = id;
            this.newStudent.name = name;
            this.newStudent.group = group;
            this.newStudent.isDonePr = isDone;
            this.showInput = true;
        },
        updateStudent:function(){
            Vue.axios.put("http://46.101.212.195:3000/students/"+this.newStudent.id, {
                 name: this.newStudent.name,
                group: this.newStudent.group,
                isDonePr: this.newStudent.isDonePr,
            })
            this.showInput = false;
            console.log(this.newStudent.name)
           
        },
   
        }
    }
</script>
<style scoped>

</style>