<template>
    <div class="register">
        <div class="register-inner">
            <v-card>
                <v-img src="/header_logo.png"></v-img>
                <v-card-title class="card-title">NWSSU BOOKING SYSTEM</v-card-title>
                <v-card-text>
                    <span class="register-text">Register a new membership</span>
                </v-card-text>
                <v-card-text class="main-container">
                    <v-form ref="form" lazy-validation>
                        <v-text-field
                            label="Student Number"
                            placeholder="Student Number"
                            append-icon="mdi-snowflake"
                            outlined
                            dense
                            color="success"
                            :rules="[() => !!payload.student_number ||  '']"
                            v-model="payload.student_number"
                        ></v-text-field>
                        <v-text-field
                            label="Last Name"
                            placeholder="Last Name"
                            append-icon="mdi-account"
                            outlined
                            dense
                            color="success"
                            :rules="[() => !!payload.last_name ||  '']"
                            v-model="payload.last_name"
                        ></v-text-field>
                        <v-text-field
                            label="First Name"
                            placeholder="First Name"
                            append-icon="mdi-account"
                            outlined
                            dense
                            color="success"
                            :rules="[() => !!payload.first_name ||  '']"
                            v-model="payload.first_name"
                        ></v-text-field>
                        <v-text-field
                            label="Middle Name"
                            placeholder="Middle Name"
                            append-icon="mdi-account"
                            outlined
                            dense
                            color="success"
                        ></v-text-field>
                        <v-text-field
                            label="Email"
                            placeholder="Email"
                            append-icon="mdi-email"
                            outlined
                            dense
                            color="success"
                            :rules="[() => !!payload.email ||  '']"
                            v-model="payload.email"
                        ></v-text-field>
                        <v-text-field
                            label="Phone Number"
                            placeholder="Phone Number"
                            append-icon="mdi-cellphone"
                            outlined
                            dense
                            type="number"
                            color="success"
                            :rules="[() => !!payload.phone ||  '']"
                            v-model="payload.phone"
                        ></v-text-field>
                        <v-text-field
                            label="Password"
                            placeholder="Password"
                            append-icon="mdi-lock"
                            outlined
                            dense
                            color="success"
                            type="password"
                            :rules="[() => !!payload.new_password ||  '']"
                            v-model="payload.new_password"
                        ></v-text-field>
                        <v-text-field
                            label="Retype password"
                            placeholder="Retype password"
                            append-icon="mdi-repeat"
                            outlined
                            dense
                            color="success"
                            type="password"
                            :rules="[() => payload.c_password == payload.new_password ||  '']"
                            v-model="payload.c_password"
                        ></v-text-field>
                    </v-form>
                </v-card-text>
                <v-card-actions class="custom-card-action mr-2">
                    <v-btn class="save" @click="save" color="success">Register</v-btn>
                </v-card-actions>
            </v-card>
            <div class="alreadyhave-accnt">
                <v-card-text>I already have a membership <v-btn text @click="$router.push({path: '/login'})">Sign in</v-btn></v-card-text>
            </div>
        </div>
    </div>
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
            },
        }
    },
    methods:{
        cancel(){
            this.reset()
            this.$router.push({name:'login'})
        },
        save(){
            if(!this.$refs.form.validate()) return;
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
    .register {
        padding-top: 3%;
        padding-bottom: 3%;
        display: flex;
        justify-content: center;
        background-color: #d2d6de;
    }

    .register-inner {
        display: flex;
        justify-content: center;
        flex-direction: column;
    }

    .register .register-inner .v-card {
        background-position: top;
        background-size: 47%;
        width: 500px;
        padding: 15px;
        border-radius: 20px;
    }

    .register .card-title {
        font-size: 26px;
        color: #006400;
        font-weight: 800;
        justify-content: center;
    }

    .register .register-text {
        font-size: 18px;
        text-align: center;
        display: block;
    }

    .register .save {
        margin-left: auto;
    }

    .register .alreadyhave-accnt {
        text-align: center;
        margin-top: 20px;
        box-shadow: 0px 3px 1px -2px rgb(0 0 0 / 20%), 0px 2px 2px 0px rgb(0 0 0 / 14%), 0px 1px 5px 0px rgb(0 0 0 / 12%);
    }

    @media(max-width: 480px) {
        .register .register-inner .v-card {
            width: 400px !important;
        }
    }
</style>