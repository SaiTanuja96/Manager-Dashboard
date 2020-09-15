<template>
    <div id="dashboard">
<ul class="collection with-header">
 <li class="collection-header">
        <h4>Employees</h4>
    </li>
     <li v-for= "employee in employees"   class="collection-item">
         <div class="chip">{{employee.dept}}</div>
       {{employee.employee_id}}:{{employee.name}}
       <router-link  class ="secondary-content" v-bind:to="{name: 'view-employee', params: {employee_id:employee.employee_id}}">
           <i class="fa fa-eye eye"></i>
       </router-link>
    </li>
</ul>
    <div class="fixed-action-btn">
<router-link to="/new" class="btn-floating btn-large red">
<i class="fa fa-plus eye"></i>
</router-link>
    </div>
    </div>
</template>

<script>
import db from './firebaseinit'
import firebase from 'firebase'
export default {
    name: 'dashboard',
    data(){
        return{
     employees : []
        }
    },
    created (){
        
        if(firebase.auth().currentUser){
            db.collection('Employees').orderBy('dept').get().then(querySnapshot =>{

            querySnapshot.forEach(doc => {
                console.log(doc.id);
                const data = {
                    'id' : doc.id,
                    'employee_id': doc.data().Employee_id,
                    'name': doc.data().name,
                    'dept': doc.data().dept,
                    'position': doc.data().position
                }
                this.employees.push(data)
            })
        })
        }
        

    }
}
</script>

<style  scoped>

.eye{

    display: inline;
    float: right;
}

</style>