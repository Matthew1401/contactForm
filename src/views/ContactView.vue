 <template>
  <main>
    <div class="form-container">
      <h1>Send me a message</h1>
      <p>Well just write down and send us your thoughts if you want or whatever.</p>

      <!-- submit.prevent co robi? -->
      <!-- @submit to event, a .prevent to modyfikacja do eventu zapobiegające przeładowywaniu się strony. -->
      <form @submit.prevent="submitForm" id="form">
        <input type="text" name="name" v-model="data.name" class="input" placeholder='Name' required>
        <input type="email" name="email" v-model="data.email" class="input" placeholder='Email' required>
        <div>{{ data.validEmail }}</div>
        <input type="text" name="title" v-model="data.subtitle" class="input" placeholder='Title' required>
        <textarea name="message" v-model="data.message" class="input textarea" placeholder='Message' required></textarea>
        <button type="submit" class="input submit">Send</button>
        <div class="response">{{ data.response }}</div>
      </form>
    </div>
  </main>
</template>

<script setup>
  import { reactive } from 'vue'
  import axios from 'axios'

  // const axios = require('axios');
  const WEB3FORMS_ACCESS_KEY = "c508eb53-1f31-4afd-a2f0-4e5f5cbab661";

  const data = reactive( {
    name: "",
    email: "",
    subtitle: "",
    message: "",
    response: "",
    validEmail: ""
  })


  const submitForm = () => {
    if (! /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(data.email)) {
      data.validEmail = 'Please enter a valid email address'
    } 
    else {
      let myPromise = new Promise(function(myResolve, myReject) {
        axios.post('https://api.web3forms.com/submit', {
          access_key: WEB3FORMS_ACCESS_KEY,
          name: data.name,
          email: data.email,
          subject: data.subtitle,
          message: data.message,
        })
        .then(function (response) {
          myResolve(response)    
        })
        .catch(function (error) {
          myReject(error)
        });
      });
      
      myPromise.then(
        function(value) {
          console.log(value.data.message);
          data.name = ''
          data.email = ''
          data.subtitle = ''
          data.message = ''
          data.validEmail = ''
          data.response = value.data.message
          setTimeout(() => {
            data.response = '';
          }, 3000);
        },
        function(error) {
          console.log(error);
        }
      );
    }
  }

  // const submitForm = async function() {
  //   if (! /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(data.email)) {
  //     data.validEmail = 'Please enter a valid email address'
  //   } 
  //   else {
  //     data.validEmail = ''
  //     // Przerobić na promise, różnica między promise a callback
  //     const response = await fetch("https://api.web3forms.com/submit", {
  //       method: "POST",
  //       headers: {
  //         "Content-Type": "application/json",
  //         Accept: "application/json",
  //       },
  //       body: JSON.stringify({
  //         access_key: WEB3FORMS_ACCESS_KEY,
  //         name: data.name,
  //         email: data.email,
  //         subject: data.subtitle,
  //         message: data.message,
  //       }),
  //     });
  //     const result = await response.json();
  //     if (result.success) {
  //       console.log('sended sucessfully');
  //       data.name = ''
  //       data.email = ''
  //       data.subtitle = ''
  //       data.message = ''
  //       data.validEmail = ''
  //       data.response = 'Sended sucesfully!'
  //       console.log(result);
  //       setTimeout(() => {
  //         data.response = '';
  //       }, 3000);
  //     }
  //   }       
  // }
</script>

<style scoped>
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .form-container {
    text-align: center;
    background-color: whitesmoke;
    border-radius: 15px;
    padding: 0 1.6em;
    width: 600px;
    height: 650px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    font-size: 15px;
  }

  h1 {
    margin-bottom: 0.4em;
    font-size: 3.5em;
    font-weight: bold;
    margin-top: 35px;
  }

  p {
    margin-bottom: 1.3em;
    font-size: 1.2em;
  }

  .input {
    width: 70%;
    height: 3.5em;
    padding: 15px;
    border-radius: 3px;
    margin-bottom: 1.5em;
    border: 1px solid black;
    font-size: .9em;
  }

  .input:focus {
    color: rgb(87, 87, 233);
  }

   .input:focus::placeholder {
    color: rgb(87, 87, 233);
  }

  .textarea {
    height: 6.5em;
    resize: none;
  }

  .submit {
    background-image: linear-gradient(to bottom right, rgb(88, 147, 224), rgb(87, 87, 233));
    border: none;
    color: whitesmoke;
    cursor: pointer;
  }

  .submit:hover {
    background-image: linear-gradient(to bottom right, rgb(129, 129, 241), rgb(146, 187, 240));
    color: whitesmoke;
  }

  /*MEDIA QUERIES--------------------------------------------- */

  /* 700px */

  @media only screen and (max-width: 700px) {
    .form-container {
      font-size: 13px;
      width: 500px;
      height: 550px;
    }
  }

  /* 550px */

  @media only screen and (max-width: 550px) {
    .form-container {
      font-size: 11px;
      width: 450px;
      height: 450px;
    }
  }

  /* 480px */

  @media only screen and (max-width: 480px) {
    .form-container {
      font-size: 10px;
      width: 380px;
      height: 450px;
    }
  }

  /* 400px */

  @media only screen and (max-width: 400px) {
    .form-container {
      font-size: 9px;
      width: 300px;
      height: 390px;
    }

    h1 {
      font-size: 2.8em;
    }

    p {
      font-size: 0.5;
    }
  }

</style>