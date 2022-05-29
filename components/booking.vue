<template>
    <v-card height=80vh>
        <v-card-title>Book an appointment</v-card-title>
        <v-card-text>
            <v-autocomplete
                v-model="selectedService"
                :items="services"
                color="success"
                label="Service"
                item-text="name"
                return-object
                @change="changeService"
                attach
            >
            </v-autocomplete>
            <template v-if="selectedService.id">
                <h1>Price: {{selectedService.price}}</h1>
                <small class="class-small">Attention: Please pay on the cashier first before going to registra..</small>
                <h1 v-if="selectedService.requirements" class="mt-3">Requirements:</h1>
                <div class="class-reqs" v-if="selectedService.requirements">
                    <p v-for="requirement in selectedService.requirements" 
                        :key="requirement.id">{{requirement.name}}</p>
                </div>
                <div class="class-action">
                    <v-btn @click="save" color="success">Book</v-btn>
                </div>
            </template>
        </v-card-text>
        <v-snackbar
            :timeout="-1"
            v-model="issuccess"
            tile
            :color="haserror?'error':'success'"
        >
            {{message}}
        </v-snackbar>
    </v-card>
</template>
<script>
export default {
    data(){
        return{
            payload:{},
            services:[],
            selectedService:{},
            haserror:false,
            issuccess:false,
            message:null,
        }
    },
    methods:{
        getServices(){
            this.$axios.get(`services`).then(({data})=>{
                this.services = data.data
            })
        },
        changeService(){
            this.payload.service_id = this.selectedService.id
        },
        save(){
            this.payload.client_id = this.$auth.user.id
            this.$axios.post(`bookings`,this.payload).then(({data})=>{
                if(data.message){
                    this.haserror = true
                    this.issuccess = true
                    this.message = data.message

                    setTimeout(() => {
                        this.haserror = false
                        this.issuccess = false
                    }, 3000);
                }else{
                    this.issuccess = true
                     setTimeout(() => {
                         this.message = "Booking successfully send"
                        this.issuccess = false
                    }, 3000);
                    this.$emit('book')
                }
            })
        }
    },
    mounted(){
        this.getServices()
    }
}
</script>
<style lang="scss" scoped>
    .class-reqs{
        margin-top: 15px;
        max-height: 500px;
        overflow: auto;
        p{
            margin-left: 15px;
            font-size: 16px;
        }
    }
    .class-action{
        margin-top: 15px;
        display: flex;
        justify-content: flex-end;
    }
    .class-small{
        color: red;
    }
</style>