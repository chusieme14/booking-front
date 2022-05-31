<template>
    <div class="login">
        <div class="login-inner">
            <v-card class="login-cont">
                <v-img src="/header_logo.png"></v-img>
                <v-card-title>NWSSU BOOKING SYSTEM</v-card-title>
                <v-card-text class="sign-text">Forgot password?</v-card-text>
                <v-card-text>
                    <v-form ref="form" lazy-validation>
                        <template v-if="!hascredential">
                            <v-text-field
                                v-model="payload.student_number"
                                color="success"
                                label="Student number"
                                :rules="[() => !!payload.student_number ||  '']"
                                placeholder="Student number"
                                append-icon="mdi-account"
                                @keydown.enter="login"
                                outlined
                                dense
                            ></v-text-field>
                            <v-text-field
                                v-model="payload.email"
                                color="success"
                                :rules="[() => !!payload.email ||  '']"
                                append-icon="mdi-mail"
                                label="Email"
                                placeholder="Email"
                                @keydown.enter="checkCredentials"
                                outlined
                                dense
                            ></v-text-field>
                        </template>
                        <template v-else>
                            <v-text-field
                                v-model="payload.password"
                                color="success"
                                label="New password"
                                :rules="[() => !!payload.password ||  '']"
                                placeholder="New password"
                                append-icon="mdi-lock"
                                type="password"
                                outlined
                                dense
                            ></v-text-field>
                            <v-text-field
                                v-model="payload.c_password"
                                color="success"
                                label="Confirm password"
                                :rules="[() => payload.c_password==payload.password ||  '']"
                                placeholder="Confirm password"
                                append-icon="mdi-lock"
                                type="password"
                                outlined
                                @keydown.enter="login"
                                dense
                            ></v-text-field>
                        </template>
                    </v-form>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn v-if="!hascredential" class="login-btn mr-2" @click="checkCredentials" color="success">Send</v-btn>
                    <v-btn v-else class="login-btn mr-2" @click="changePassword" color="success">Save</v-btn>
                </v-card-actions>
                 <!-- <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn class="forgot-pass">Forgot password ?</v-btn>
                </v-card-actions> -->
            </v-card>
        <v-snackbar
            :timeout="-1"
            v-model="hasError"
            tile
            color="red accent-2"
        >
            You're credential not found.
        </v-snackbar>
            <div class="no-accnt">
                <v-card-text>Remember the password? <v-btn text @click="$router.push({path: '/login'})">Sign in</v-btn></v-card-text>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    head(){
        return {
            title:'Login',
            meta:[
                {
                hid:"description",
                name:"description",
                content:"this is the login page"
                }
            ]
        }
    },
    data(){
        return{
            payload:{},
            hasError:false,
            show3: false,
            hascredential:false,
        }
    },
    methods:{
        checkCredentials(){
            if(!this.$refs.form.validate()) return;
            this.$axios.post(`forgot-password`,this.payload).then(({data})=>{
                if(data){
                    this.$refs.form.resetValidation()
                    this.hascredential = data
                    // this.$router.push({name:'login'})
                }else{
                    this.hasError = true
                    setTimeout(() => {
                        this.hasError = false
                    }, 3000);
                }
            })
        },
        changePassword(){
            if(!this.$refs.form.validate()) return;
            this.$axios.post(`change-password`,this.payload).then(({data})=>{
                this.$router.push({name:'login'})
            })
        },
        saveCurrentRoute(){
            localStorage.setItem('current_path', )
        }
    },
    created(){
    }
}
</script>
<style scoped>
    .login {
        display: flex;
        justify-content: center;
        background-color: #d2d6de;
    }

    .login-inner {
        height: 100vh;
        display: flex;
        justify-content: center;
        flex-direction: column;
    }

    .login-inner .v-card {
        background-position: top;
        background-size: 47%;
        width: 500px;
        padding: 15px;
    }

    .login-inner .v-card .v-card__title {
        font-size: 26px;
        color: #006400;
        font-weight: 800;
        justify-content: center;
    }

    .login .v-card {
        font-size: 20px;
        text-align: center;
        font-weight: 400;
        border-radius: 20px;
    }

    .login-inner .v-card,
    .no-accnt {
        border: 1px solid rgba(var(--b6a,219,219,219),1);
    }

    .login-inner .v-card .theme--light.v-input input {
            color: rgba(0, 0, 0, 1) !important;
    }

    .no-accnt {
        text-align: center;
        margin-top: 20px;
        box-shadow: 0px 3px 1px -2px rgb(0 0 0 / 20%), 0px 2px 2px 0px rgb(0 0 0 / 14%), 0px 1px 5px 0px rgb(0 0 0 / 12%);
    }

     .login-inner .v-card .v-card__text {
        font-size: 18px;
         padding-bottom: 0 !important;
     }

     .login-inner .v-card .login-btn {
        width: 25%;
        height: 40px;
        display: block;
        margin-left: auto;
     }

     .login-inner .v-card .v-card__actions {
        display: block;
        text-align: center;
     }

    .forgot-pass,
    .forgot-pass:hover {
        box-shadow: none;
        background-color: transparent !important;
    }

    .login-form {
        background-color: #fff;
    }

    .login input {
        background: #fafafa;
        background: rgba(var(--b3f,250,250,250),1);
        border: 0;
        -webkit-box-flex: 1;
        -webkit-flex: 1 0 auto;
        -ms-flex: 1 0 auto;
        flex: 1 0 auto;
        margin: 0;
        outline: 0;
        overflow: hidden;
        padding: 9px 0 7px 8px;
        text-overflow: ellipsis;
    }

    @media(max-width: 480px) {
        .login .login-inner .login-cont {
            width: 400px !important;
        }
    }
</style>