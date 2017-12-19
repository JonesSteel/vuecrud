<template>
    <div>
        <div class="form-group">
            <router-link :to="{name: 'createCompany'}" class="btn btn-success">Create New Company</router-link>
        </div>

        <div class="panel panel-default">
            <div class="panel-heading">Companies List</div>
            <div class="panel-body">
                <table class="table table-bordered table-striped">
                    <thead>
                    <tr>
                        <th style="text-align: center">Name</th>
                        <th style="text-align: center">Address</th>
                        <th style="text-align: center">Website</th>
                        <th style="text-align: center">Email</th>
                        <th width="130" style="text-align: center">Acciones</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="company, index in companies">
                        <td>{{ company.name }}</td>
                        <td>{{ company.address }}</td>
                        <td>{{ company.website }}</td>
                        <td>{{ company.email }}</td>
                        <td>
                            <router-link :to="{name: 'editCompany', params: {id: company.id}}" class="btn btn-xs btn-default">Edit</router-link>
                            <a href="#" class="btn btn-xs btn-danger" v-on:click="deleteEntry(company.id, index)">Delete</a>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                companies: []
            }
        },
        mounted() {
            var app = this;
            axios.get('/api/v1/companies')
                .then(function (resp) {
                    app.companies = resp.data;
                })
                .catch(function (resp) {
                    console.log(resp);
                    alert("Couldn't load companies");
                });
        },
        methods: {
            deleteEntry(id, index) {
                if (confirm("Do you really want to delete it?")) {
                    var app = this;
                    axios.delete('/api/v1/companies/' + id)
                        .then(function (resp) {
                            app.companies.splice(index, 1);
                        })
                        .catch(function (resp) {
                            alert("Couldn't delete company");
                        });
                }
            }
        }
    }
</script>

<style scoped>

</style>