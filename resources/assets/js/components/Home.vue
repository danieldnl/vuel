<template>
    <div class="container">
        <div class="row pull-right">
            <router-link to="/create">
                <a class="btn btn-primary">Create Notebook</a>
            </router-link>
        </div>
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div v-if="isLoading">Loading...</div>
                <div class="panel panel-default" v-for="notebook in notebooks">
                    <div @click="deleteIt(notebook.id)" class="btn pull-right"><i class="fa fa-times"></i></div>
                    <div @click="edit(notebook.id)" class="btn pull-right"><i class="fa fa-pencil"></i></div>
                    <form @submit.prevent="update(notebook.id)">
                        <div class="panel-heading clearfix">
                            <span class="pull-left" v-show="!showForm(notebook.id)">{{ notebook.name }}</span>
                            <input style="width: 200px;" type="text" class="form-control" v-show="showForm(notebook.id)" v-model="nbEditData.name">
                            <span class="pull-right" v-show="!showForm(notebook.id)">{{ notebook.user.name }}</span>
                        </div>
                        <div class="panel-body">
                            <span v-show="!showForm(notebook.id)">{{ notebook.body }}</span>
                            <input type="text" class="form-control" v-show="showForm(notebook.id)" v-model="nbEditData.body">
                            <button class="btn btn-primary btn-panel" v-show="showForm(notebook.id)">OK</button>
                            <button class="btn btn-primary btn-panel" v-show="showForm(notebook.id)" @click.prevent="editForm=false">Cancel</button>
                        </div>    
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                notebooks: [],
                isLoading: false,
                editForm: "",
                nbEditData: {
                    name: '',
                    body: ''
                }
            }
        },
        methods: {
            edit(id) {
                var self = this;
                this.notebooks.forEach(function(notebook, i) {
                    if(notebook.id == id) {
                        self.nbEditData = notebook;
                    }
                });
               this.editForm = id;
            },
            showForm(id) {
                if(this.editForm === id) {
                    return true;
                }
                return false
            },
            update(id) {
                var self = this;
                axios.put('/notebook/' + id, this.nbEditData).then(function(response){
                    console.log(response);
                    self.editForm = false;
                    self.nbEditData = '';
                    self.$router.push('');
                }).catch(function(error){
                    console.log(error.response);
                })
            },
            deleteIt(id) {
                var self = this;
                let ok = confirm("Are you sure?");
                if(ok) {
                    axios.delete('/notebook/' + id).then(function(response){
                        console.log(response);
                        self.showAll();
                    });
                }
            },
            showAll() {
                var self = this;
                this.isLoading = true;
                axios.get('/notebook').then(function(response){
                    self.notebooks = response.data;
                    self.isLoading = false;
                })
            }
        },
        mounted() {
            this.showAll();
        }
    }
</script>

<style scoped>
    .btn-panel {
        margin-top: 10px;
        margin-left: 10px;
        float: right;
    }
</style>
