<template>
    <div class="add_employee">
        <h2>Working with POST request</h2>
        <div class="form-wrapper">
            <form action="" class="form" @submit.prevent="submitHandler">
                <input required v-model="formData.name" type="text" placeholder="Your name" class="form-name" minlength="2" maxlength="60">
                <input required v-model="formData.email" type="text" placeholder="Email" class="form-email" pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$">
                <input required v-model="formData.phone" type="text" placeholder="Phone" class="form-phone" pattern="[\+]{0,1}380([0-9]{9})">
                <span class="form-phone-pattern">+38 (XXX) XXX - XX - XX</span>
                <div class="radio-wrapper">
                    <p>Select your position</p>
                    <div 
                        class="radio-button"
                        v-for="(position, idx) in positions" 
                        :key="idx"
                    >
                        <input required v-model="formData.position_id" type="radio" :id="idx" name="position" :value="idx + 1">
                        <label :for="idx">{{position.name}}</label>
                    </div>
                </div>
                <div class="upload">
                    <input required @change="setImage($event)" type="file" id="upload-photo" class="upload-photo" accept="image/jpg, image/jpeg">
                    <label for="upload-photo">Upload</label>
                    <span class="uploaded-photo">{{uploadedPhoto}}</span>
                </div>
                <input type="submit" class="form-submit" value="Sign up" :disabled="isDisabled">
            </form>
        </div>
        <div v-if="success" class="success-message">
            <h1 class="success-message-header">User successfully registered</h1>
            <img src="../assets/success-image.png" alt="">
        </div>
        <div v-if="error" class="error-message">
            {{errorMessage}}
        </div>
    </div>
</template>

<script>
import { REFRESH_USERS } from '@/constants/events-constants.js'
import Bus from '../utils/event-bus.js'
    export default {
        data() {
            return {
                positions: [],
                formData: {
                    name: null,
                    email: null,
                    phone: null,
                    position_id: null,
                    photo: null
                },
                success: false,
                error: false,
                errorMessage: null,
                uploadedPhoto: 'Upload your photo'
            }
        },
        computed: {
            isDisabled() { 
                return !Boolean(this.formData.name && this.formData.email && this.formData.phone && this.formData.position_id && this.formData.photo)
            }
        },
        methods: {
            setImage(event) {
                this.formData.photo = event.target.files[0]
                this.uploadedPhoto = event.target.files[0].name
            },
            async submitHandler() {
                const token = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/token')
                    .then((res) => res.json())
                const formData = new FormData()
                formData.append('name', this.formData.name)
                formData.append('email', this.formData.email)
                formData.append('phone', this.formData.phone)
                formData.append('position_id', this.formData.position_id)
                formData.append('photo', this.formData.photo)
                await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/users', {
                    method: 'POST',
                    body: formData,
                    headers: {
                        'Token': token.token,
                    }
                }).then(function(response) { 
                    return response.json();
                }).then((data) => { 
                    if(data.success) {  
                        this.error = false
                        this.success = true
                    } else {
                        this.success = false
                        this.error = true
                        this.errorMessage = data.message
                    }
                })
                Bus.$emit(REFRESH_USERS)
            }
        },
        async created() {
            const positionsResult = await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/positions')
                .then((res) => res.json())
            this.positions = positionsResult.positions
        }
    }
</script>

<style lang="scss" scoped>
.add_employee {
    display: flex;
    flex-direction: column;
    align-items: center;


    h2 {
        text-align: center;
        font-size: 40px;
        line-height: 40px;
        margin-bottom: 50px;
    }

    .form {
        display: flex;
        flex-direction: column;

        &-name,
        &-email,
        &-phone {
            width: 380px;
            height: 54px;
            border: 1px solid #D0CFCF;
            border-radius: 4px;
            margin-bottom: 50px;
            padding-left: 16px;
            font-size: 16px;
            line-height: 26px;
        }

        &-phone {
            margin-bottom: 0;
        }

        &-phone-pattern {
            font-size: 12px;
            line-height: 14px;
            color: #7E7E7E;
            margin-top: 4px;
            margin-bottom: 50px;
            margin-left: 16px;
        }

        &-photo {
            width: 293px;
            height: 54px;
            border: 1px solid #D0CFCF;
            border-radius: 4px;
            padding-left: 16px;
            font-size: 16px;
            line-height: 26px;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
            border-left: none;
        }

        &-submit {
            margin: 50px auto;
            width: 100px;
            height: 34px;
            border: none;
            background-color: #F4E041;
            border-radius: 80px;
        }

        &-submit:hover {
            background-color: #FFE302;
            cursor: pointer;
        }

        &-submit:disabled {
            background-color: #B4B4B4;
            color: white;
            cursor: not-allowed;
        }

        .radio-wrapper {
            margin-bottom: 27px;

            p {
                margin-bottom: 20px;
            }

            .radio-button {
                margin-bottom: 20px;

                label {
                    display: inline-block;
                    padding-top: 1px;
                }

                input:where([type="radio"]){
                    -webkit-appearance : none;
                    appearance: none;
                    width: 20px;
                    height: 20px;
                    margin: calc(0.75em - 11px) 0.25rem 0 0;
                    vertical-align: top;
                    border: 1px solid #D0CFCF;
                    border-radius: 4px;
                    background: #F8F8F8 no-repeat center center;
                }

                input[type="radio"]{
                    border-radius : 50%;
                    margin-right: 12px;
                }

                input:where([type="radio"]):where(:active:not(:disabled), :focus){
                    border-color: #00BDD3;
                    outline: none;
                }

                input[type="radio"]:checked{
                    background-image: url('../assets/radio.svg');
                }

            }
        }
        .upload {
            display: flex;
        }

        input#upload-photo {
           display: none;
        }

        input#upload-photo + label {
            padding: 20px 15px 15px 15px;
            border: 1px solid black;
            border-radius: 4px 0px 0px 4px;
        }

        input#upload-photo + label:hover {
            cursor: pointer;
        }

        .uploaded-photo {
            display: inline-block;
            font-size: 16px;
            line-height: 26px;
            color: #7E7E7E;
            padding: 15px 0;
            width: 100%;
            padding-left: 15px;
            border: 1px solid #7E7E7E;
            border-radius: 0 4px 4px 0;
            border-left: none;
        }

        input::placeholder {
            color: #D0CFCF;
            font-size: 16px;
            line-height: 26px;
            font-family: 'Nunito';
        }
    }

    .success-message {
        text-align: center;
        margin-top: 50px;
        margin-bottom: 100px;
        &-header {
            font-size: 40px;
            line-height: 40px;
            margin-bottom: 50px;
        }
    }

    .error-message {
        margin-bottom: 100px;
        color: red;
        font-size: 24px;
        line-height: 32px;
    }
}

@media (max-width: 400px) {
    .add_employee {
        .form {
            &-name,
            &-email,
            &-phone {
                width: 328px;
            }
        }
    }
}
</style>