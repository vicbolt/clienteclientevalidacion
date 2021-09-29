<template>
    <div class="users"> 
        <div v-for="user in users" :key="user._id">
            <p> {{ user.username}} </p>
            <v-btn class="btn btn-warning" @click="redirigir(user_id)"> CHAT </v-btn>

        </div>
        
    </div>
</template>

<script>

export default{
    data(){
        return {
            users: [],
        }
    },

    async beforeMount(){
    const token = window.localStorage.getItem('token');
        if (!token) {
            this.$router.push('/login')
        }

        await this.getAllUsers(token)
    },
    methods: {
        redirigir(id) {
            this.$router.push(`/chat/${id}`)
        },

        async getAllUsers (token){
            try{
                const res = await fetch('http://localhost:4500/user/all', 
                {
                    headers: {
                        token,
                    }
                })
                const data = await res.json()
                if (data.error){
                    alert(data.error)
                    return this.$router.push('/login')
                }

                const users = data.users
                for (let i = 0; i < users.length; i++){
                    this.users.push(users[i])
                }

            } catch (err) {
                alert(err)
            }
        }
    }
}
</script>
