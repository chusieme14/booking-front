<template>
    <v-card>
        <v-card-title>You're Appointment</v-card-title>
        <v-card-text class="view-main-container">
           <v-card class="mb-2" v-for="appointment in appointments" :key="appointment.id">
               <v-card-text>
                    <h3>{{appointment.service.name}}</h3>
                    <template v-if="appointment.status==1">
                        <small >Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <p class="class-primary">Waiting</p>
                    </template>
                    <template v-if="appointment.status==2">
                        <small>Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small>Schedule date: {{$moment(appointment.date_accepted).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <p class="class-blue">Accepted</p>
                    </template>
                    <template v-if="appointment.status==3">
                        <small>Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small>Schedule date: {{$moment(appointment.date_accepted).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small>Completed date: {{$moment(appointment.date_completed).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <p class="class-success">Done</p>
                    </template>
                    <template v-if="appointment.status==4">
                        <small>Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small>Declined reason: {{appointment.declined_reason}}</small>
                        <p class="class-error">Declined</p>
                    </template>
               </v-card-text>
           </v-card>
        </v-card-text>
        <!-- <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="update" color="success">Save</v-btn>
        </v-card-actions> -->
        <v-snackbar
            :timeout="-1"
            v-model="issuccess"
            tile
            color="success"
        >
            Change already save
        </v-snackbar>
    </v-card>
</template>

<script>
  export default {
      data(){
        return{
            start:new Date(),
            end:new Date(),
            appointments: [],
            issuccess:false,
            star:0,
            israting:true
        }
      },
      methods:{
            update(){
                if(this.$route.params.code && this.$route.params.code!=this.$auth.user.share_code) {
                    this.payload.code = this.$route.params.code
                }
                this.$axios.put(`appointments/${this.payload.id}`, this.payload).then(({data})=>{
                    this.issuccess = true

                    setTimeout(() => {
                        this.issuccess = false
                    }, 3000);
                })
            },
            getAppointments(){
                this.$axios.get(`view-appointments/${this.$auth.user.id}`).then(({data})=>{
                    this.appointments = data
                })
            }
      },
      created(){
          this.getAppointments()
      },
      computed:{
           
      }
  }
</script>

<style scoped>
    .class-action{
        margin-top:-10px;
        display: flex;
        justify-content: flex-end;
    }
    h1 {
        margin-bottom: 20px;
    }
    .educ-attainment,
    .examp-pass,
    .employ,
    .course,
    .graduated {
        margin-bottom: 30px;
    }

    .award,
    .section {
        margin-bottom: 40px;
        margin-top: -10px;
    }
    .class-primary{
        color: #f39c12;
    }
    .class-blue{
        color: #1976D2;
    }
    .class-success{
        color: #006048;
    }
    .class-error{
        color: red;
    }
    .view-main-container{
        max-height: 76vh;
        overflow: auto;
    }
</style>