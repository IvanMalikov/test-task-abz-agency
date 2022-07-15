<template>
     <div class="employees">
        <h2>Working with GET request</h2>
        <div class="users">
            <div
                class="user"
                v-for="user in users"
                :key="user.id"
            >
                <EmployeeComponent :user="user" />
            </div>
        </div>
        <button @click="showMore">Show more</button>
    </div>
</template>

<script>
import Bus from '../utils/event-bus.js'
import {REFRESH_USERS} from '../constants/events-constants.js'
import EmployeeComponent from './EmployeeComponent.vue'
    export default {
        components: {
            EmployeeComponent
        },
        data() {
            return {
                users: [],
                page: 1
            }
        },

        methods: {
            getUsers() {
                return fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users?page=${this.page}&count=6`)
                    .then((res) => res.json())
            },
            async showMore() {
                this.page++
                const result = await this.getUsers()
                this.users.push(...result.users)
            },
            initListeners() {
                Bus.$on(REFRESH_USERS, this.refreshUsers)
            },
            removeListeners() {
                Bus.$off(REFRESH_USERS, this.refreshUsers)
            },
            async refreshUsers() {
                this.page = 1
                const usersResult = await this.getUsers()
                this.users = usersResult.users
            }
        },

        created() {
            this.refreshUsers()
            this.initListeners()
        },
        beforeDestroy() {
            this.removeListeners()
        }
    }
</script>

<style lang="scss" scoped>
.employees {
    text-align: center;

    h2 {
        font-size: 40px;
        line-height: 40px;
        margin-bottom: 30px;
    }

    button {
        width: 100px;
        height: 34px;
        border: none;
        background-color: #F4E041;
        border-radius: 80px;
        margin-bottom: 140px;
    }

    button:hover {
        background-color: #FFE302;
        cursor: pointer;
    }

    .users {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        margin-bottom: 50px;

        .user {
            width: 370px;
            height: 254px;
            margin-top: 20px;
            border-radius: 16px;
            background-color: #fff;

            p {
                text-overflow: ellipsis;
                overflow: hidden; 
                white-space: nowrap;
                font-size: 16px;
                line-height: 26px;
            }

            &_photo {
                margin: 20px 0;
                border-radius: 100%;
                width: 70px;
                height: 70px;
            }

            &_name {
                margin-bottom: 20px;
            }

            &_email {
                word-break: break-all;
            }
        }
    }
}

@media (max-width: 1120px) {
    .employees {
        .users {
            width: 904px;
            margin: 0 auto;
            margin-bottom: 50px;
            .user {
                width: 282px;
                height: 254px;
            }
        }
    }
}

@media (max-width: 904px) {
    .employees {
        .users {
            width: 704px;
            margin: 0 auto;
            margin-bottom: 50px;
            .user {
                width: 344px;
                height: 254px;
            }
        }
    }
}

@media (max-width: 704px) {
    .employees {
        .users {
            justify-content: center;
            width: inherit;
            margin: 0 auto;
            margin-bottom: 50px;
            .user {
                width: 328px;
                height: 254px;
            }
        }
    }
}
</style>