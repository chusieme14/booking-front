<template>
    <v-card height=80vh>
        <v-card-title>Book an appointment</v-card-title>
        <v-card-text>
            <v-autocomplete
                v-model="selectedService"
                :multiple="true"
                :items="services"
                color="success"
                label="Service"
                item-text="name"
                return-object
                @change="changeService"
                attach
            >
            </v-autocomplete>
            <template >
                <div class="class-main-content">
                    <div v-for="service in selectedService" :key="service.id">
                        <h1>{{service.name}}</h1>
                        <h3>Price: {{service.price}}</h3>
                        <small class="class-small">Attention: Please pay on the cashier first before going to registra..</small>
                        <h1 v-if="service.requirements" class="mt-3">Requirements:</h1>
                        <div class="class-reqs" v-if="service.requirements && !isUpload">
                            <div class="class-reqs-inner" v-for="requirement in service.requirements" :key="requirement.id">
                                <p>{{requirement.name}}</p>
                                <p>{{requirement.file_name}}</p>
                                <v-btn v-if="!requirement.file_name" @click="triggerUpload(service,requirement)" class="class-reqs-inner-btn" x-small>Upload file</v-btn>
                            </div>
                        </div>
                    </div>
                    <input 
                        ref="file_input"
                        type='file' class="hidden" 
                        accept="image/x-png,image/gif,image/jpeg" 
                        @change="onFileChange($event.target.files)"
                    />
                </div>
                <div v-if="selectedService.length" class="class-action">
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
            image_base64:[],
            clickService:{},
            clickrequirement:{},
            services:[],
            selectedService:[],
            haserror:false,
            issuccess:false,
            isUpload:false,
            message:null,
        }
    },
    methods:{
        _pluck(items, key) {
          return items.map(item => item[key]);
        },
        getServices(){
            this.$axios.get(`services`).then(({data})=>{
                this.services = data.data
            })
        },
        changeService(){
            this.payload.service_ids = this._pluck(this.selectedService, 'id')
        },
        save(){
            this.payload.client_id = this.$auth.user.id
            this.payload.documents = this.image_base64
            console.log(this.payload,"payloaddddddd")
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
        },
        triggerUpload(service, requirement) {
            this.clickService = service
            this.clickrequirement = requirement
            this.$refs.file_input.click()
        },

        async onFileChange(file) {
            this.isUpload = true
            let imageFile = file[0]
            let temp = {}
            if (file.length>0) {
                if (!imageFile.type.match('image.*')) {
                    this.errorDialog = true
                    this.errorText = 'Please choose an image file'
                } else {
                    let imageURL = URL.createObjectURL(imageFile)
                    console.log(imageFile.name,"imageURL")
                    this.clickrequirement.file_name = imageFile.name
                    temp.image_base64 = await this.createImageBase64(imageFile);
                    temp.service_id = this.clickService.id
                    temp.requirement_id = this.clickrequirement.id
                    this.image_base64.push(temp)
                    // this.avatar_blob = imageURL
                    // this.payload.image_base64 = await this.createImageBase64(imageFile);
                    
                }
            }
            this.isUpload = false
            console.log(this.selectedService,"skdjlsakjdlksajdkkkk")
            console.log(this.image_base64,"makasjsjndjshsjdhsjdsdhjhd")
            // this.payload.avatar = this.payload.image_base64
            // this.$axios.put(`update-graduate-profile/${this.payload.id}`,this.payload).then(({data})=>{
            //     this.getGraduate()
            // })

        },
        createImageBase64(file) {
            var reader = new FileReader();

            return new Promise((resolve, reject) => {
                reader.onload = e => {
                    let res = e.target.result;
                    resolve(res);
                };
                reader.readAsDataURL(file);
            });
        },
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
        .class-reqs-inner{
            display: flex;
            justify-content: space-between;
            .class-reqs-inner-btn{
                margin-right: 15px;
            }

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
    .class-main-content{
        max-height: 55vh !important;
        overflow: auto;
    }
</style>