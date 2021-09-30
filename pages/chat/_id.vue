<template>
    
    <div class="chat"> 
        
        <div class="container-message" v-if="userId && paramId">
            <div class="message" v-for="message in messages" :key="message._id">
                <p v-if="message.userOwner == userId"> MINE </p>
                <p v-else>SUYO22</p>
                <p> {{ message.text }}</p>
            </div>


        <v-text-field v-model="message"> </v-text-field>
        <v-btn @click="enviarMsj"> ENVIAR </v-btn>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            messages: [],
            paramId: undefined,
            userId: undefined,
            message: '',
        }
    },

   async beforeMount () {
        this.paramId = this.$route.params.id
        this.userId = window.localStorage.getItem('userId')

       const token = window.localStorage.getItem('token')
       if (!token){
           this.$router.push('/login')
       }

       await this.getMessages(token)
    },
    methods: {

        async enviarMsj() {
      try {
        const token = window.localStorage.getItem('token')
        const userOneId = window.localStorage.getItem('userId')
        const userTwoId = this.$route.params.id
        const body = JSON.stringify({
          userOneId,
          userTwoId,
          userOwnerId: userOneId,
          text: this.message,
        })
        const res = await fetch('http://localhost:4500/message/create', {
          method: 'post',
          headers: {
            token,
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          this.messages = []
          await this.getMessages(token)
        }
      } catch (err) {
        alert(err)
      }
    },

        async getMessages (token) {
            try{
                const userOneId = window.localStorage.getItem('userId')
                const userTwoId = this.$route.params.id
            
                console.log(this.$route.params)

                const body = JSON.stringify({
                    userOneId,
                    userTwoId,
                })

                

                 const res = await fetch('http://localhost:4500/message/chat', {
                     method: 'post',
                     headers: {
                         'Content-type': 'application/json',
                         token,
                     },
                     body,
                 })

            const data = await res.json()
            if(data.error){
                alert (data.error)
                return this.$router.push('/login')
            }

            const messages = data.messages
            for (let i = 0; i < messages.length; i++) {
            this.messages.push(messages[i])
            }

            console.log({ data })

            } catch (err){
                alert(err)
            }
        },
    },
}

</script>
