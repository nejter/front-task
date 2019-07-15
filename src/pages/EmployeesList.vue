<template>
    <div class="page employees-list">
        <h1 class="employees-list__header">Employees</h1>
        <div v-if="loading" class="employees-list__loading">Loading...</div>
        <table v-else class="employees-list__list">
            <tr class="employees-list__list-header">
                <th>id</th>
                <th>Name</th>
                <th>Address</th>
                <th>Phone</th>
                <th>Email</th>
                <th>Edit</th>
            </tr>
            <tr v-for="employee in employees" :key="employee.id" class="employees-list__list-row">
                <td>{{employee.id}}</td>
                <td>{{employee.name}}</td>
                <td>{{employee.address.street}} {{employee.address.suite}} {{employee.address.city}}</td>
                <td>{{employee.phone}}</td>
                <td><a :href="`mailto:${ employee.email }`">{{employee.email}}</a></td>
                <td><EditEmployeeButton @click.native="handleModalOpen(employee)" /></td>
            </tr>
        </table>
        <Modal v-if="modalOpen" @closeModal="handleModalClose()" :employee="employee" />
    </div>
</template>
<script>
    import axios from 'axios';
    import EditEmployeeButton from "@components/EditEmployeeButton";
    import Modal from "@components/Modal";

    export default {
        data() {
            return {
                loading: false,
                employees: [],
                modalOpen: false,
                employee: {}
            }
        },
        components: {
            EditEmployeeButton,
            Modal
        },
        created () {
            this.fetchData()
        },
        watch: {
            '$route': 'fetchData'
        },
        methods: {
            fetchData () {
                this.loading = true;

                axios.get('https://jsonplaceholder.typicode.com/users')
                    .then(({data}) => {
                        this.employees = data;
                    })
                    .finally(() => {
                        this.loading = false;
                    })
            },
            saveToApi() {
                axios.patch("https://jsonplaceholder.typicode.com/users/" + this.employee.id, {
                    method: "PUT",  
                    name: this.employee.name,
                    address: this.employee.address,
                    phone: this.employee.phone,
                    email: this.employee.email,
                    headers: {
                        "Content-type": "application/json; charset=UTF-8"
                    }
                }).then(response => console.log(response));
            },
            handleModalOpen(employee) {
                this.modalOpen = true;
                this.employee = employee;
            },
            handleModalClose() {
                this.modalOpen = false;
                this.saveToApi();
            }
        }
    };

</script>
<style lang="scss" scoped>
    .employees-list {
        overflow-x:auto;

        &__header {
            font-size: 20px;
            padding: 0 0 10px;
        }

        &__loading {
            color: #999d9d;
            text-align: center;
        }

        &__list {
            font-size: 14px;
            width: 100%;
            border-collapse: collapse;
        }

        &__list-header {
            background: #f7f8f9;
            border-bottom: 1px solid #999d9d;

            th {
                padding: 8px;
            }
        }

        &__list-row {
            background: #fff;

            td {
                padding: 8px;
            }
        }
    }
</style>