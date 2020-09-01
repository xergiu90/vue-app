<template>
    <div class="d-flex w-100 align-items-center justify-content-center">
        <form @submit="checkForm">
            <div class="form-group">
                <label for="username">User</label>
                <input class="form-control" id="username" aria-describedby="usernameHelp" placeholder="Enter username" required  v-model="username">
                <div class="invalid-feedback">
                    User is required!
                </div>
            </div>
            <div class="form-group">
                <label for="password">Password</label>
                <input type="password" class="form-control" id="password" placeholder="Password" required  v-model="password">
                <div class="invalid-feedback">
                    Password is required!
                </div>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</template>


<script>
    export default {
        name: 'login',
        data() {
            return {
                username: null,
                password: null
            }
        },
        methods: {
            checkForm(){
                if(this.username && this.password)
                   this.$emit('isValidLogin')
            }
        }
    }
</script>
