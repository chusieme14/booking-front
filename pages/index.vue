<template>
    <div class="profile">
        <div class="categ">
            <ul>
                <li :class="tabs==0?'active':''"><v-btn @click="changeTab(0)">Change password</v-btn></li>
                <li :class="tabs==1?'active':''"><v-btn @click="changeTab(1)">Personal information</v-btn></li>
                <li :class="tabs==2?'active':''"><v-btn @click="changeTab(2)">Book an appointment</v-btn></li>
                <li :class="tabs==3?'active':''"><v-btn @click="changeTab(3)">View appointment</v-btn></li>
                <!-- <li :class="tabs==4?'active':''"><v-btn @click="changeTab(4)">Rate appointment</v-btn></li> -->
            </ul>
        </div>
        <div class="infos">
            <password-profile v-if="tabs==0"></password-profile>
            <personal-info v-if="tabs==1"></personal-info>
            <booking v-if="tabs==2" @book="changeTab(3)"></booking>
            <view-appointment v-if="tabs==3" ></view-appointment>
            <!-- <rate v-if="tabs==4"></rate> -->
        </div>
    </div>
</template>
<script>
export default {
    middleware: 'auth',
    data(){
        return{
            tabs:0,
            appUrl:process.env.appUrl,
        }
    },
    methods:{
        changeTab(tab){
            this.tabs = tab
            localStorage.setItem('tab',tab)
        }
    },
    mounted(){
        if(localStorage.getItem('tab')){
            this.tabs = parseInt(localStorage.getItem('tab'))
        }
    }
}
</script>
<style lang='scss' scoped>
    .class-share{
        display: flex;
        justify-content: center;

        div{
            display: flex;
            justify-content: center;
            flex-direction: column;
        }
    }
    .active{
        background-color: #006048;
        button{
            color: white;
        }
    }
    .profile {
        margin-right: 28px;
        max-width: 935px;
        width: 100%;
        background: transparent;
        width: 100%;
        margin: auto;
        margin-top: 30px;
        display: flex;
        border: 1px solid rgba(var(--b6a,219,219,219),1);
    }

    .profile .categ {
        width: 30%;
        padding-top: 20px;
        border-right: 1px solid rgba(var(--b6a,219,219,219),1);
    }

    .profile .infos {
        width: 70%;
        padding-top: 20px;
        padding-left: 6%;
        padding-right: 6%;
        padding-bottom: 3%;
    }

    .profile ul {
        padding-left: 0;
    }

    .profile ul li {
        list-style: none;
        margin-bottom: 6px;
    }

    .profile ul li button {
        box-shadow: none;
        width: 100%;
        background-color: transparent !important;
    }

    .profile ul li button span.v-btn__content {
        justify-content: start !important;
    }

    @media(max-width: 480px) {
        .profile {
            display: block;
        }

        .profile .categ,
        .profile .infos {
            width: 100%;
        }
    }

</style>