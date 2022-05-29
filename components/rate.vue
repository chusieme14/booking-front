<template>
    <v-card>
        <v-card-title>View/Rate appointment</v-card-title>
        <v-card-text>
           <v-card>
               <v-card-text>
                    <h3>Certificate of English as Medium of Instruction</h3>
                    <small>{{$moment(start).format('MMMM Do YYYY, h:mm:ss a')}} - {{$moment(end).format('MMMM Do YYYY, h:mm:ss a')}}</small>
                    <template v-if="israting">
                        <v-icon @click="star=1" :color="star>=1?'yellow darken-1':''">mdi-star</v-icon>
                        <v-icon @click="star=2" :color="star>=2?'yellow darken-1':''">mdi-star</v-icon>
                        <v-icon @click="star=3" :color="star>=3?'yellow darken-1':''">mdi-star</v-icon>
                        <v-icon @click="star=4" :color="star>=4?'yellow darken-1':''">mdi-star</v-icon>
                        <v-icon @click="star=5" :color="star>=5?'yellow darken-1':''">mdi-star</v-icon>
                    </template>
                    <template v-if="israting">
                        <v-textarea
                            v-model="payload.suggestion"
                            filled
                            label="Suggestions"
                        >
                        </v-textarea>
                    </template>
                    <div class="class-action">
                        <v-btn v-if="israting" color="success">save</v-btn>
                    </div>
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
            courses: [],
            payload:{
                suggestion:''
            },
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
            getGraduate(){
                this.$axios.get(`appointments/${this.$auth.user.id}`).then(({data})=>{
                    this.payload = data
                })
            }
      },
      created(){
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
</style>