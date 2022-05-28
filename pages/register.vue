<template>
    <v-card height="92vh">
        <v-card-title>
            <span class="text-h5">Register a new membership</span>
            </v-card-title>
        <v-card-text class="main-container">
            <v-text-field color="success" v-model="payload.residence" label="Student number"></v-text-field>
            <v-text-field color="success" v-model="payload.residence" label="Last name"></v-text-field>
            <v-text-field color="success" v-model="payload.residence" label="First name"></v-text-field>
            <v-text-field color="success" v-model="payload.residence" label="Middle name"></v-text-field>
            <v-text-field color="success" v-model="payload.residence" label="Email"></v-text-field>
            <v-text-field color="success" v-model="payload.residence" label="Phone number"></v-text-field>
            <v-text-field color="success" type="password" v-model="payload.residence" label="Password"></v-text-field>
            <v-text-field color="success" type="password" v-model="payload.residence" label="Retype password"></v-text-field>
        </v-card-text>
        <v-card-actions class="custom-card-action mr-2">
            <v-btn @click="save" color="success">Register</v-btn>
        </v-card-actions>
        
    </v-card>
</template>
<script>
export default {
    components:{
    },
    props:{
        show:{
            type:Boolean
        },
        details:{

        },
        selected_item:{

        },
        isedit:{
            type:Boolean
        }
    },
    data(){
        return{
            e1:1,
            ismultiple:false,
            payload:{
                image_base64:null,
                prof_exam_passed:'No',
                detail:{}
            },
        }
    },
    methods:{
        next(){
            this.e1 += 1 
        },
        prev(){
            this.e1 -= 1
        },
        preventReload() {
            window.onbeforeunload = function(){
                return "Are you sure you want to refresh the window?, selected file will be removed!";
            }
            this.isimport = false
        },
        cancel(){
            this.reset()
            this.$router.push({name:'login'})
        },
        save(){
            console.log(this.payload,"save payload")
            this.payload.section = this.payload.section.toUpperCase()
            this.$axios.post(`register`,this.payload).then(({data})=>{
                this.$router.push({name:'login'})
            })
        },
        reset(){
            this.isfetching = true
            this.payload.image_base64 = null
        }
    },
    
}
</script>
<style lang="scss" scoped>
    .class-images{
        display: flex;
        justify-content: center;

        .v-image{
            cursor: pointer;
        }
    }
    .image-text-field{
        margin-top: -19px !important;
    }
    .custom-card-action{
        display: flex;
        justify-content: flex-end;
    }
    .main-container{
        max-height: 84vh;
        overflow: auto;
    }
</style>