<template>
    <div class="container">
        <div class="row">
            <div class="col">
                <div v-if="success">messaggio inviato</div>
                <form @submit.prevent="sendForm">
                    <div class="mb-3">
                        <div v-for="(error, i) in errors.name" :key="i" >{{error}}</div>
                        <label for="name" class="form-label">nome</label>
                        <input v-model="name" type="text" class="form-control" id="name" name="name">
                    </div>
                    <div class="mb-3">
                        <div v-for="(error, i) in errors.email" :key="i" >{{error}}</div>
                        <label for="mail" class="form-label">mail</label>
                        <input v-model="email"  type="email" class="form-control" id="mail" name="email">
                    </div>
                    <div class="mb-3 form-check">
                        <div v-for="(error, i) in errors.message" :key="i" >{{error}}</div>
                        <label class="form-check-label" for="msg">messaggio</label>
                        <textarea v-model="message"  type="" col='20'  id="msg" name="message"> </textarea>
                        
                    </div>
                    <button type="submit" class="btn btn-primary mt-4">{{ sending ? 'sto inviando' : 'invia' }}</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:'Contact',
    data(){
        return{
            name:'',
            email:'',
            message:'',
            errors:{},
            success: false,
            sending: false
        }
    },
    methods: {
        sendForm(){
            this.sending = true;
            this.success = false;
            axios.post('/api/contacts', {
                'name': this.name,
                'email': this.email,
                'message': this.message
            })
            .then(response => {
                if(!response.data.success){
                    this.errors = response.data.errors;
                    this.success = false;
                    this.sending = false;
                }else{
                    this.success = true;
                    this.sending = false;
                    this.name = '';
                    this.email = '';
                    this.message = '';
                }
                

            })
            .catch(error=>{
                console.log(error)
            })
        }
    }
}
</script>

<style lang="scss" scoped>

</style>