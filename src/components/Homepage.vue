<template>
    <div :class="theme">
        <nav class="navbar navbar-expand-sm" :class="navClass">
            <div class="container">
                <a class="navbar-brand" href="#">AI Assistant</a>
                <div class="d-flex ms-auto">
                    <button class="btn" :class="toggle_button" @click="toggleTheme">{{buttonText}}</button>
                </div>
            </div>
        </nav>
        <div class="jumbotron" id="jumbotron">
            <div class="container">
                <div class='message_area'></div>
                <hr class="my-4 horizontal-rule" >
                <form @submit.prevent="submitPrompt">
                    <div class="form-group horizontal-rule d-flex align-center-items"  >
                      <label for="prompt_message"></label>
                      <input type="text" v-model="promptMessage" name="prompt_message" id="prompt_message" class="form-control" placeholder="&nbsp;&nbsp;&nbsp;Ask me anything!!!" autocomplete="off">
                      <button class="btn submit_button" :class="sendButton" type="submit" id="submit_button"><i class="fa fa-paper-plane"></i></button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: "HomePage",
        data(){
            return {
                theme: 'light-mode',
                promptMessage: '',
                responseMessage: '',
                loading: false,
                error: null
            }
        },
        computed: {
            navClass(){
                return this.theme === 'light-mode' ? 'navbar-light bg-light' : 'navbar-dark bg-dark';
            },
            buttonText(){
                return this.theme === 'light-mode' ? 'Dark Mode' : 'Light Mode';
            },
            sendButton(){
                return this.theme === 'light-mode' ? 'btn-light' : 'btn-dark';
            },
            toggle_button(){
                return this.theme === 'light-mode' ? 'btn-outline-dark' : 'btn-outline-light'; 
            }
        },
        methods: {
            toggleTheme(){
                this.theme = this.theme === 'light-mode' ? 'dark-mode' : 'light-mode';
            },
            async submitPrompt(){
                if (this.promptMessage === ""){
                    alert("Please enter a prompt!!!")
                } else {
                    const messageContainer=document.querySelector(".message_area");
                    const sendElement=document.createElement('div');
                    sendElement.innerText=this.promptMessage;
                    sendElement.classList.add('message_send');
                    sendElement.classList.add('right');
                    messageContainer.append(sendElement);
                    let button=document.querySelector("#submit_button");
                    button.disabled=true;
                    const response = await axios.post('http://localhost:3000/text_generation',{message: this.promptMessage});
                    const receiveElement=document.createElement('div');
                    receiveElement.innerText=response.data;
                    receiveElement.classList.add('message_receive');
                    receiveElement.classList.add('left');
                    messageContainer.append(receiveElement);
                    button.disabled=false; 
                    this.promptMessage="";
                }
            }
        }
    }
</script>

<style>
#jumbotron{
    height: 88vh;
    padding-top: 30px;
    margin: 20px 20px 20px 20px;
    background-color: rgb(203, 200, 200);
    border-radius: 20px;
}
.container {
    position: relative;
    height: 100%; 
}
.message_area{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
    overflow-y: auto;
    height: calc(100% - 100px);;
}
.horizontal-rule {
    position: absolute;
    bottom: 25px;
    left: 0;
    right: 0;
    margin: 0;
    border: 0; 
}
#prompt_message{
    border-radius: 20px;
    margin-top: 20px;
    height: 45px;
}
.submit_button{
    margin-left: 10px;
    margin-top: 20px;
    border-radius: 30px;
}
.left { 
    float: left;
}
.right { 
    float: right;
    margin-left: auto; 
}
.message_receive, .message_send {
    max-width: 70%; /* Limit width to prevent overly wide messages */
    padding: 6px;
    border-radius: 15px;
    word-wrap: break-word;
    text-align: left;
}
.message_receive {
    background-color: rgb(180, 180, 170);
    border: 1px solid rgb(180, 180, 170);
    color: black;
}
.message_send {
    background-color: black;
    border: 1px solid black;
    color: white;
    align-self: flex-end; /* Align sent messages to the right */
}
</style>

