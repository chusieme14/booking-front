<template>
    <div class="header">
        <div class="inner-header">
            <div class="logo">
                <v-img @click="$router.push({path:'/'})" width="65" src="/logo.png"></v-img>
            </div>
            <div class="search">
                <h2>NwSSU BOOKING SYSTEM</h2>
            </div>
            <div class="header-icons text-right">
                <v-tooltip bottom z-index=100>
                    <template v-slot:activator="{ on, attrs }">
                        <v-icon 
                            @click="logout"
                            v-bind="attrs"
                            v-on="on"
                        >mdi-logout</v-icon>
                    </template>
                    <span>Sign out</span>
                </v-tooltip>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    methods:{
        logout(){
            this.$auth.logout()
        },
    }
}
</script>

<style>
    .header {
        height: 60px;
        position: fixed;
        left: 0;
        right: 0;
        top: 0;
        background: #fff;
        z-index: 99;
        border-bottom: 1px solid rgba(var(--b6a,219,219,219),1);
    }

    .inner-header {
        display: flex;
        flex-direction: row;
        align-items: center;
        max-width: 975px;
        padding: 0 20px;
        width: 100%;
        z-index: 10;
        margin: auto;
        height: 60px;
        transition: height .2s ease-in-out;
    }


    .logo {
        -webkit-box-flex: 1;
        flex: 1 0 127px;
    }

    .header-icons {
        flex: 1 0 127px;
        -webkit-box-flex: 1;
        -webkit-flex: 1 0 127px;
        -ms-flex: 1 0 127px;
        display: flex;
        justify-content: end;
    }

    .header-icons i {
        font-size: 30px !important;
        margin-left: 10px;
    }

    .header .for-logout {
        display: none;
    }

    @media(max-width: 480px) {
        .search {
            display: none;
        }

        .header-icons button {
            margin-left: 10px;
        }

        .header .for-logout {
            display: block;
        }
    }

</style>