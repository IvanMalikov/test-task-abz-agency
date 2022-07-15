<template>
    <header class="header">
        <div class="header_inner">
            <div class="header_logo">
                <img src="../assets/Logo.svg" alt="">
            </div>
            <div class="header_buttons">
                <button class="header_buttons-users">Users</button>
                <button class="header_buttons-signup">Sign Up</button>
            </div>
        </div>
        
    </header>
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>
.header {
    background-color: #fff;

    &_inner {
        width: 1170px;
        height: 60px;
        margin: 0 auto;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    &_buttons {
        font-size: 16px;
        line-height: 26px;
        button {
            width: 100px;
            height: 34px;
            border: none;
            background-color: #F4E041;
            border-radius: 80px;
        }

        button:hover {
            background-color: #FFE302;
            cursor: pointer;
        }

        &-users {
            margin-right: 10px;
        }
    }
}

@media (max-width: 1170px) {
    .header {
        &_inner {
            padding: 0 60px;
            width: inherit;
        }
    }
}

@media (max-width: 768px) {
    .header {
        &_inner {
            padding: 0 32px;
            width: inherit;
        }
    }
}

@media (max-width: 400px) {
    .header {
        &_inner {
            padding: 0 16px;
            width: inherit;
        }
    }
}
</style>