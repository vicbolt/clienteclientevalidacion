<template>
    <div class="login"> 
    <v-text-field v-model="username" placeholder="Nombre de Usuario"></v-text-field>
    <v-text-field v-model="password1" placeholder="Contraseña" type="password"></v-text-field>
    <v-text-field v-model="password2" placeholder="Repita la contraseña" type="password"></v-text-field>
    <v-btn block elevation="2" x-large @click="registro"> REGISTRARME </v-btn>
    </div>
</template>

<script>
export default {
    data() {
        return {
                username: "",
                password1: "",
                password2: "",
            }
    },

    methods: {
        async registro(){

            const body = {
                username: this.username,
                password: this.password1,
            }
            

        try{
            const res = await fetch('http://localhost:4500/user/inicioSesion',{
                method: 'post',
                headers: {
                    'Content-type': "application/json",
                },
                body: JSON.stringify(body),
            })

            const data = await res.json()
            if(data.error) {
                alert(data.error)
            }

        this.$router.push('/users')

        } catch (err) {
            alert (err)
        }
        },
    },
}
</script>