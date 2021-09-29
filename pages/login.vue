<template>
    <div class="login"> 
    <v-text-field v-model="username" placeholder="Username"></v-text-field>
    <v-text-field v-model="password" placeholder="Password" type="password"></v-text-field>
    <v-btn block elevation="2" x-large @click="onLogin"> SEND </v-btn>
    </div>
</template>

<script>
export default {
    data() {
        return {
            username: "",
            password: "",
        }
    },
    methods: {
        async onLogin(){

            const body = {
                username: this.username,
                password: this.password,
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

        window.localStorage.setItem('token', data.token)
        window.localStorage.setItem('userId', data.userId)
        this.$router.push('/users')

        } catch (err) {
            alert (err)
        }
        },
    },
}
</script>
