<template>
    <div v-if="message == 'edited'"
        class="flex p-4 justify-between text-sm text-yellow-800 border border-yellow-300 rounded-lg bg-yellow-50 dark:bg-gray-800 dark:text-yellow-300 dark:border-yellow-800"
        role="alert">
        <i class="fa-solid fa-bell mr-2 fa-xl"></i>
        <span class="sr-only">Info</span>
        <div>
            <span class="font-medium">User's Role</span> {{ this.message }} successfully.
        </div>
        <button @click="this.message = ''">
            <i class="fa-solid fa-circle-xmark fa-xl"></i>
        </button>
    </div>

    <div v-if="message == 'deleted'"
        class="flex p-4 justify-between text-sm text-red-800 border border-red-300 rounded-lg bg-red-50 dark:bg-gray-800 dark:text-red-400 dark:border-red-800"
        role="alert">
        <i class="fa-solid fa-bell mr-2 fa-xl"></i>
        <span class="sr-only">Info</span>
        <div>
            <span class="font-medium">User</span> {{ this.message }} successfully.
        </div>
        <button @click="this.message = ''">
            <i class="fa-solid fa-circle-xmark fa-xl"></i>
        </button>
    </div>

    <div class=" h-12 px-10 py-2 flex justify-end border-b-2">
        <input type="text " v-model="filter" @keyup="setFilterData"
            class="border-slate-300 focus:outline-cyan-600 px-2 rounded-md border-2">
        <button @click="getUser" class="bg-cyan-400 rounded-full border-2 border-cyan-500 ml-2 w-8  h-8 px-2">
            <i class="fa-solid fa-magnifying-glass  text-cyan-800"></i>
        </button>
    </div>

    <div class="w-100 h-screen flex justify-center items-center" v-if="pageLoading">
        <n-space>
            <n-spin size="medium" />
        </n-space>
    </div>



    <div class="flex justify-center items-center center overflow-auto" v-if="!pageLoading">
        <table class="w-full text-sm text-left text-gray-500 overflow-auto dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                <tr>
                    <th scope="col" class="px-6 py-3">
                        ID
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Name
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Email
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Role
                    </th>
                    <th scope="col" class=" py-3">
                        Action
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr class="bg-white border-b dark:bg-gray-900 dark:border-gray-700 hover:bg-slate-200 cursor-pointer"
                    v-for="row, index in rows" :key="row.id" @click="this.userRoleHandler(row)">
                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                        {{ row.id }}
                    </th>
                    <td class="px-6 py-4">
                        {{ row.name }}
                    </td>
                    <td class="px-6 py-4">
                        {{ row.email }}
                    </td>
                    <td class="px-6 py-4  flex  ">

                        <div>
                            <div v-if="row.role.length == 4" class="mr-2 text-red-500" @click="roleSubmit('admin',)">
                                Admin
                            </div>
                            <div v-if="row.role.length == 3" class="mr-2 text-yellow-500" @click="roleSubmit('gold')">
                                Gold
                            </div>
                            <div v-if="row.role.length == 2" class="mr-2 text-slate-500" @click="roleSubmit('silver')">
                                Silver
                            </div>
                            <div v-if="row.role.length == 1" class="mr-2 text-green-500" @click="roleSubmit('free')">
                                Free
                            </div>
                        </div>

                        <div v-if="index === this.indexNumber && this.showOrIndex">
                            <button v-if="index === this.indexNumber && this.showOrIndex"
                                @click="this.roleDropDown = !this.roleDropDown"
                                class="text-slate-500 border-2 border-slate-300 hover:cursor-pointer bg-white hover:bg-slate-100 focus:outline-none  font-medium rounded-sm text-sm px-1 py-1 text-center inline-flex items-center   "
                                type="button"> Role Edit <svg class="w-4 h-4 ml-2" aria-hidden="true" fill="none"
                                    stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M19 9l-7 7-7-7"></path>
                                </svg></button>
                            <div id="dropdown"
                                class="z-10  bg-white divide-y divide-gray-100 absolute rounded-lg shadow w-44 dark:bg-gray-700"
                                v-if="roleDropDown">
                                <ul class="py-2 text-sm text-gray-700 dark:text-gray-200"
                                    aria-labelledby="dropdownDefaultButton">
                                    <li>
                                        <h1 @click="roleSubmit('admin', row.id)"
                                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">
                                            Admin</h1>
                                    </li>
                                    <li>
                                        <h1 @click="roleSubmit('silver', row.id)"
                                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">
                                            Sliver</h1>
                                    </li>
                                    <li>
                                        <h1 @click="roleSubmit('gold', row.id)"
                                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">
                                            Gold</h1>
                                    </li>
                                    <li>
                                        <h1 @click="roleSubmit('free', row.id)"
                                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">
                                            Free</h1>
                                    </li>
                                </ul>
                            </div>
                        </div>

                        <button @click="showOrIndexHandler(index)" v-if="index != this.indexNumber" class="ml-2">
                            <i class="fa-solid fa-pen-to-square">e</i>
                        </button>
                        <button @click="roleHandlerClose(index)" v-if="index === this.indexNumber" class="ml-2">
                            <i class="fa-solid fa-xmark">c</i>
                        </button>

                    </td>
                    <td class="px-1 w-32 ">
                        <button class="bg-slate-100 w-10 h-10 ml-2 rounded-md  hover:text-red-500"
                            @click="deleteUser(row.id)">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <div class="hidden sm:flex sm:flex-1 sm:items-center px-12 py-2 shadow-md sm:justify-between">

        <div>
            <p class="text-sm text-gray-700">
                Showing
                <span class="font-medium">{{ this.from }}</span>
                to
                <span class="font-medium">{{ this.to }}</span>
                of
                <span class="font-medium">{{ this.total }}</span>
                results
            </p>
        </div>
        <div>
            <nav class="isolate inline-flex -space-x-px rounded-md shadow-sm" aria-label="Pagination">
                <button
                    class="relative inline-flex items-center rounded-l-md px-2 py-2 text-gray-400 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0"
                    @click="prev" v-if="this.prev_page_url">
                    <i class="fa-solid fa-angle-left"></i>
                </button>
                <div v-for="page in totalPage">
                    <button @click="pageChange($event)" :id="page"
                        class="relative z-10 inline-flex items-center bg-cyan-400 hover:bg-cyan-500  border-x-2 rounded-md px-4 py-2 text-sm font-semibold text-white focus:z-20 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">{{
                            page
                        }}</button>
                </div>
                <button
                    class=" relative inline-flex items-center rounded-r-md px-2 py-2 text-gray-400 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0"
                    @click="next" v-if="this.next_page_url">
                    <i class="fa-solid fa-chevron-right"></i>
                </button>
            </nav>
        </div>
    </div>
</template>

<script>
import ApiServices from '../../../../services/ApiServices';

export default {
    data() {
        return {
            filter: window.localStorage.getItem('filter_user'),
            rows: '',
            roleDropDown: false,
            showOrIndex: false,
            indexNumber: null,
            rows: '',
            total: '',
            from: '',
            showData: {},
            to: '',
            perPage: '',
            totalPage: '',
            next_page_url: '',
            prev_page_url: '',
            message: '',
            pageLoading : false
        }
    },
    methods: {
        setFilterData() {
            window.localStorage.setItem('filter_user', this.filter)
            if (!window.localStorage.getItem('filter_user', this.filter)) {
                this.getUser()
            }
        },
        deleteUser(id) {
            ApiServices.get('/deleteuser/' + id).then((res) => {
                this.getUser();
                this.message = 'deleted'
                setTimeout(() => { this.message = '' }, 2000);
            }).catch(res => console.log(res))
        },
        getUser(params = '', url = '?page=') {
            this.pageLoading = true
            ApiServices.get('alluser/' + window.localStorage.getItem('filter_user') + url + params).then((res) => {
                this.rows = res.data.data.data;
                this.total = res.data.data.total;
                this.from = res.data.data.from;
                this.to = res.data.data.to;
                this.totalPage = res.data.data.last_page;
                this.next_page_url = res.data.data.next_page_url;
                this.prev_page_url = res.data.data.prev_page_url;
                this.pageLoading = false
            }
            ).catch(res => console.log(res))

        },
        pageChange($event) {
            this.getUser($event.target.id);
        },
        next() {
            this.getUser('', this.next_page_url)
        },
        prev() {
            this.getUser('', this.prev_page_url)
            console.log();
        },
        roleHandlerClose(number) {
            this.roleDropDown = false;
            this.indexNumber = null
            this.showOrIndex = false
        },
        showOrIndexHandler(number) {
            this.roleDropDown = false;
            this.indexNumber = number
            this.showOrIndex = true
        },
        roleSubmit(role, user) {
            ApiServices.get(`/role/${role}/${user}`).then((res) => {
                this.getUser();
                this.message = 'edited'
                setTimeout(() => { this.message = '' }, 2000);
            }
            ).catch(res => console.log(res))
        },

    },
    mounted() {
        this.getUser();
    }
}
</script>