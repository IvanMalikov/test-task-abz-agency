<template>
    <div class="about">
        <div class="about_content">
            <h1>Test assignment for front-end developer</h1>
            <p>
                What defines a good front-end developer is one that has skilled knowledge 
                of HTML, CSS, JS with a vast understanding of User design thinking as 
                they'll be building web interfaces with accessibility in mind. They should 
                also be excited to learn, as the world of Front-End Development keeps evolving.
            </p>
            <button class="header_buttons-signup">Sign Up</button>
        </div>
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>
.about {
    height: 650px;
    background: linear-gradient(0deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('../assets/pexels-alexandr-podvalny-1227513.jpeg');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    display: flex;
    text-align: center;
    margin-bottom: 140px;

    &_content {
        margin: auto;
        width: 380px;
        height: 323px;
        color: white;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;

        h1 {
            font-size: 40px;
            line-height: 40px;
        }

        p { 
            font-size: 16px;
            line-height: 26px;
        }

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
    }
}

@media (max-width: 768px) {
    .about {
        height: 500px;    
    }
}

@media (max-width: 400px) {
    .about {
        padding: 0 10px; 
        
        &_content {
            width: 328px;
            height: 389px;
        }
    }
}
</style>