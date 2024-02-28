<template>
    <section class="bg-gray-50 dark:bg-gray-900">
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0">
            <a href="#" class="flex items-center mb-6 text-2xl font-semibold text-gray-900 dark:text-white">
                <img class="w-8 h-8 mr-2" src="https://flowbite.s3.amazonaws.com/blocks/marketing-ui/logo.svg" alt="logo">
                Flowbite    
            </a>
            <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
                <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                    <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl dark:text-white">
                        Create and account
                    </h1>
                    <form class="space-y-4 md:space-y-6" action="#">
                        <div>
                            <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Your email</label>
                            <input type="email" name="email" id="email" v-model="formRegis.email" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="name@company.com" required="">
                        </div>
                        <div>
                            <label for="fname" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Your first name</label>
                            <input type="text" name="fname" id="fname" v-model="formRegis.fname" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="first name" required="">
                        </div>
                        <div>
                            <label for="lname" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Your last name</label>
                            <input type="lname" name="lname" id="lname" v-model="formRegis.lname" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="last name" required="">
                        </div>
                        <div>
                            <label for="tel" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Your telephone</label>
                            <input type="text" name="tel" id="tel" v-model="formRegis.tel" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="telephone" required="">
                        </div>
                        <div>
                            <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Password</label>
                            <input type="password" name="password" id="password" v-model="formRegis.password" placeholder="••••••••" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required="">
                        </div>
                        <div>
                            <label for="confirm-password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Confirm password</label>
                            <input type="password" name="confirm-password" v-model="formRegis.confirmPassword" id="confirm-password" placeholder="••••••••" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required="">
                        </div>
                        <button type="button" @click="onRegsiter" class="w-full text-white bg-blue-600 hover:bg-blue-700 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Create an account</button>
                    </form>
                </div>
            </div>
        </div>
    </section>
</template>

<script lang="ts" generic="T">
import axios from 'axios';
import Swal from 'sweetalert2';

interface FormRegister {
    fname: string
    lname: string
    email: string
    tel: string
    password: string
    confirmPassword: string
}

interface Users {
    fname: string
    lname: string
    email: string
    tel: string
}

interface Response {
    status: boolean
    message: string
    data?: Users
}

export default {
    name: "Register",
    data(){
        return {
            formRegis: {
                fname: '',
                lname: '',
                email: '',
                tel: '',
                password: '',
                confirmPassword: ''
            }
        }
    },
    mounted(){

    },
    methods:{
       async onRegsiter(): Promise<void>{
            let formRegister: FormRegister = this.formRegis;
            if(
                this.onValidate(formRegister.email) && 
                this.onValidate(formRegister.fname) && 
                this.onValidate(formRegister.lname) && 
                this.onValidate(formRegister.password) && 
                this.onValidate(formRegister.confirmPassword) &&
                this.onValidate(formRegister.tel)
            ){
                if(!this.onMatchPassword(formRegister.password, formRegister.confirmPassword)){
                    Swal.fire({
                        position: "center",
                        icon: "error",
                        title: "รหัสผ่านไม่ตรงกัน",
                        showConfirmButton: false,
                        timer: 1500
                    });
                    return
                }
                
                const {confirmPassword, ...body} = formRegister
                
                let response: Response = await axios.post("http://localhost:4000/users/register", body).then((res) => res) as Response
                if(response.status){
                    Swal.fire({
                        position: "center",
                        icon: "success",
                        title: "สมัครสมาชิกสำเร็จ",
                        showConfirmButton: false,
                        timer: 1500
                    });
                    return
                }
                Swal.fire({
                    position: "center",
                    icon: "error",
                    title: "กรุณากรอกข้อมูลให้ครบ",
                    showConfirmButton: false,
                    timer: 1500
                });
                return
            }

            return
        },
        onValidate(val: string): boolean {
            if(!val) return false
            return true
        },
        onMatchPassword(password: string, confirmPassword: string): boolean {
            if(password === confirmPassword) return true
            return false
        }
    }
}
</script>
