<template>
  <v-card>
    <v-card-title>Personal Informations</v-card-title>
    <v-card-text>
      <v-form ref="form" lazy-validation>
        <v-text-field 
          color="success" 
          v-model="payload.student_number"
          :rules="[() => !!payload.student_number ||  '']" 
          label="Student number"></v-text-field>
        <v-text-field 
          color="success" 
          v-model="payload.last_name"
          :rules="[() => !!payload.last_name ||  '']" 
          label="Last name"></v-text-field>
        <v-text-field 
          color="success"
          :rules="[() => !!payload.first_name ||  '']" 
          v-model="payload.first_name" 
          label="First name"></v-text-field>
        <v-text-field 
          color="success"
          v-model="payload.middle_name" 
          label="Middle name"></v-text-field>
        <v-text-field 
          color="success"
          :rules="[() => !!payload.email ||  '']" 
          v-model="payload.email" 
          label="Email"></v-text-field>
        <v-text-field 
          color="success"
          :rules="[() => !!payload.phone ||  '']" 
          v-model="payload.phone" 
          label="Phone number"></v-text-field>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <!-- <v-btn @click="copy" color="success">Copy shared link</v-btn> -->
      <v-btn class="mr-2" @click="update" color="success">Update</v-btn>
    </v-card-actions>
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

<style scoped>
    .civil-status,
    .gender {
        margin-top: 30px;
    }
</style>


<script>
  export default {
    data: () => ({
      appUrl:process.env.appUrl,
      payload:{},
      issuccess:false
    }),
    watch: {
    },
    methods: {
      update(){
        if(!this.$refs.form.validate()) return;
        this.$axios.put(`clients/${this.payload.id}`, this.payload).then(({data})=>{
          this.issuccess = true

          setTimeout(() => {
            this.issuccess = false
          }, 3000);
        })
      },
      getClient(){
        this.$axios.get(`clients/${this.$auth.user.id}`).then(({data})=>{
          this.payload = data
        })
      }
    },
    computed:{
      
    },
    created(){
      this.getClient()
    }
  }
</script>