<template>
    <div>
        <img class="user_photo" :src="user.photo" alt="">
        <p class="user_name">{{user.name}}</p>
        <p class="user_position">{{user.position}}</p>
        <p class="user_email">{{user.email}}</p>
        <p class="user_phone">{{user.phone}}</p>
    </div>
</template>

<script>
    export default {
        props: ['user']
    }
</script>

<style lang="scss" scoped>
    p {
        text-overflow: ellipsis;
        overflow: hidden; 
        white-space: nowrap;
        font-size: 16px;
        line-height: 26px;
    }

    .user_photo {
        margin: 20px 0;
        border-radius: 100%;
        width: 70px;
        height: 70px;
    }

    .user_name {
        margin-bottom: 20px;
    }

    .user_email {
        word-break: break-all;
    }
</style>