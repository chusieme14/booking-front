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
                    <!-- <template v-if="appointment.status==2">
                        <small>Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small>Schedule date: {{$moment(appointment.date_accepted).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <p class="class-blue">Accepted</p>
                    </template> -->
                    <template v-if="appointment.status==3||appointment.status==4 || appointment.status==2 || appointment.status==5">
                        <small>Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small v-if="appointment.status==3 || appointment.status==2 || appointment.status==5">Schedule date: {{$moment(appointment.date_accepted).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <small v-if="appointment.status==4">Declined reason: {{appointment.declined_reason}}</small>
                        <br v-if="appointment.status==3 || appointment.status==5">
                        <small v-if="appointment.status==3 || appointment.status==5">Completed date: {{$moment(appointment.date_completed).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small v-if="appointment.status==2" class="class-success">Accepted</small>
                        <small v-if="appointment.status==3" class="class-success">Done</small>
                        <small v-if="appointment.status==4" class="class-error">Declined</small>
                        <small v-if="appointment.status==5" class="class-success">Rated</small>
                        <br>

                        <template v-if="!appointment.rating && israting && selected_id==appointment.id">
                            <small class="class-success">Waiting Time</small>
                            <v-icon class="waiting-time" @click="payload.star=1" :color="payload.star>=1?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star=2" :color="payload.star>=2?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star=3" :color="payload.star>=3?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star=4" :color="payload.star>=4?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star=5" :color="payload.star>=5?'yellow darken-1':''">mdi-star</v-icon>
                            <br>
                        </template>
                        <template v-if="!appointment.rating && israting && selected_id==appointment.id">
                            <small class="class-success">Service Provided</small>
                            <v-icon @click="payload.star1=1" :color="payload.star1>=1?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star1=2" :color="payload.star1>=2?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star1=3" :color="payload.star1>=3?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star1=4" :color="payload.star1>=4?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star1=5" :color="payload.star1>=5?'yellow darken-1':''">mdi-star</v-icon>
                            <br>
                        </template>
                        <template v-if="!appointment.rating && israting && selected_id==appointment.id">
                            <small class="class-success">Office Staff</small>
                            <v-icon class="office-staff" @click="payload.star2=1" :color="payload.star2>=1?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star2=2" :color="payload.star2>=2?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star2=3" :color="payload.star2>=3?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star2=4" :color="payload.star2>=4?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon @click="payload.star2=5" :color="payload.star2>=5?'yellow darken-1':''">mdi-star</v-icon>
                            <v-textarea
                                color="success"
                                filled
                                label="Suggestion"
                                placeholder="Suggestion"
                                v-model="payload.suggestion"
                            >

                            </v-textarea>
                        </template>

                        <template v-if="appointment.rating">
                            <small class="class-success">Waiting Time</small>
                            <v-icon class="waiting-time" :color="appointment.rating.star_number>=1?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.rating.star_number>=2?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.rating.star_number>=3?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.rating.star_number>=4?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.rating.star_number>=5?'yellow darken-1':''">mdi-star</v-icon>
                            <br>
                        </template>
                        <template v-if="appointment.service_provided">
                            <small class="class-success">Service Provided</small>
                            <v-icon :color="appointment.service_provided.star_number>=1?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.service_provided.star_number>=2?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.service_provided.star_number>=3?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.service_provided.star_number>=4?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.service_provided.star_number>=5?'yellow darken-1':''">mdi-star</v-icon>
                            <br>
                        </template>
                        <template v-if="appointment.rating">
                            <small class="class-success">Office Staff</small>
                            <v-icon class="office-staff" :color="appointment.rating.star_number>=1?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.office_staff.star_number>=2?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.office_staff.star_number>=3?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.office_staff.star_number>=4?'yellow darken-1':''">mdi-star</v-icon>
                            <v-icon :color="appointment.office_staff.star_number>=5?'yellow darken-1':''">mdi-star</v-icon>
                            <br>
                            <small>Suggestion: {{appointment.rating.suggestion}}</small>
                        </template>
                        <div class="class-action-rate">
                            <v-btn v-if="!appointment.rating && selected_id!=appointment.id" small @click="israting=true, selected_id=appointment.id">Document received</v-btn>
                            <template v-if="!appointment.rating && israting && selected_id==appointment.id">
                                <v-btn class="mr-2" small @click="israting=false, selected_id=0">Cancel</v-btn>
                                <v-btn small @click="saveRating(appointment)">Save</v-btn>
                            </template>
                        </div>
                    </template>
                    <!-- <template v-if="appointment.status==4">
                        <small>Requested date: {{$moment(appointment.date_requested).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                        <br>
                        <small>Declined reason: {{appointment.declined_reason}}</small>
                        <p class="class-error">Declined</p>
                    </template> -->
               </v-card-text>
           </v-card>
        </v-card-text>
        <v-snackbar
            :timeout="-1"
            v-model="issuccess"
            tile
            :color="haserror?'error':'success'"
        >
            {{message}}

            <template v-slot:action="{ attrs }">
                <v-btn
                    text
                    v-bind="attrs"
                    @click="issuccess = false"
                >
                Close
                </v-btn>
            </template>
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
            payload:{
                star:0,
                star1:0,
                star2:0,
                suggestion:''
            },
            issuccess:false,
            star:0,
            israting:false,
            haserror:false,
            message:null,
            selected_id:0
        }
      },
      methods:{
            saveRating(val){
                if(this.payload.star==0 || this.payload.star1==0 || this.payload.star2==0){
                    this.issuccess = true
                    this.haserror = true
                    this.message = 'Please complete all rating details'
                    setTimeout(() => {
                        this.issuccess = false
                        this.haserror = false
                    }, 3000);
                    return
                }
                this.$axios.put(`rate-booking/${val.id}`, this.payload).then(({data})=>{
                    this.getAppointments()
                    this.payload.star = 0 
                    this.payload.star1 = 0 
                    this.payload.star2 = 0 
                    this.payload.suggestion = ''
                    this.issuccess = true
                    this.message = 'Thank you for your feedback'
                    setTimeout(() => {
                        this.issuccess = false
                    }, 3000);
                })
            },
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
    .class-action-rate
    {
        display: flex;
        justify-content: flex-end;
    }
    .waiting-time{
        margin-left: 18px;
    }
    .office-staff{
        margin-left: 26px;
    }
</style>