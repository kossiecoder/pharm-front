<template>
    <b-container>
        <h1 class="text-center mb-4">Member List</h1>
        <template v-if="!loading">
            <b-row>
                <b-col
                    cols="12"
                    md="3"
                >
                    <b-input
                        v-model="filter.firstName"
                        class="mb-2"
                        placeholder="Filter by First Name"
                        @keyup.enter="getMembers"
                    ></b-input>
                </b-col>
                <b-col
                    cols="12"
                    md="3"
                >
                    <b-input
                        v-model="filter.surname"
                        class="mb-2"
                        placeholder="Filter by Surname"
                        @keyup.enter="getMembers"
                    ></b-input>
                </b-col>
                <b-col
                    cols="12"
                    md="3"
                >
                    <b-input
                        v-model="filter.email"
                        class="mb-2"
                        placeholder="Filter by Email"
                        @keyup.enter="getMembers"
                    ></b-input>
                </b-col>
                <b-col
                    cols="12"
                    md="3"
                >
                    <b-button
                        @click="getMembers"
                        class="w-100"
                        variant="primary"
                    >Filter</b-button>
                </b-col>
            </b-row>

            <div class="mt-4">
                <b-table
                    :items="memberData.data"
                    responsive="sm"
                ></b-table>

                <b-pagination
                    v-model="memberData.current_page"
                    :total-rows="memberData.total"
                    :per-page="memberData.per_page"
                    aria-controls="my-table"
                    align="center"
                    @input="getMembers"
                ></b-pagination>
            </div>
        </template>


<!--        <LoadingSpinner v-else />-->
    </b-container>
</template>

<script>
    import axios from "axios";
    // import LoadingSpinner from "./LoadingSpinner";

    export default {
        components: {
            // LoadingSpinner
        },
        data() {
            return {
                loading: true,
                memberData: null,
                filter: {
                    firstName: '',
                    surname: '',
                    email: ''
                }
            }
        },
        async created() {
            await this.getMembers()
            this.loading = false;
        },
        methods: {
            async getMembers(page = 1) {
                try {
                    const res = await axios.get('http://localhost:8000/api/members', {
                        params: {
                            page,
                            firstname: this.filter.firstName,
                            surname: this.filter.surname,
                            email: this.filter.email
                        }
                    });

                    this.memberData = res.data.result;
                } catch (e) {
                    console.log(e)
                }
            },
        }
    }
</script>
