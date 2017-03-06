<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <form @submit.prevent="addNotebook">
                    <div class="form-group">
                        <label for="">Name</label>
                        <input type="text" class="form-control" placeholder="Name of Notebook" v-model="notebookData.name">
                        <span class="text-danger">{{ errors.name ? errors.name[0] : "" }}</span>
                    </div>
                    <div class="form-group">
                        <label for="">Body</label>
                        <input type="text" class="form-control" id="" placeholder="Name of Notebook" v-model="notebookData.body">
                        <span class="text-danger">{{ errors.body ? errors.body[0] : "" }}</span>
                    </div>
                    <button type="submit" class="btn btn-primary">Submit</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                notebookData: {
                    name: '',
                    body: ''
                },
                errors: {}
            }
        },
        methods: {
            addNotebook() {
                var self = this;
                axios.post('/notebook', this.notebookData).then(function(response){
                    self.notebookData = "";
                    self.errors = "";
                    self.$router.push('/');
                }).catch(function(error){
                    self.errors = error.response.data;
                })
            }    
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
