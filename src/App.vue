<template>
  <div class="app">
    <div class="banner">
      <div class="banner-logo flex flex-column item-center"><img src="./assets/jaiye-logo.svg" alt=""></div>
      <div class="flex flex-column item-center banner-img"><img src="./assets/pic.svg" alt=""></div>
      <div class="flex flex-column item-center"><button class="banner-btn" @click="scrollToView"> Let's start the party</button></div>
    </div>

  <transition name="modal">
    <Modal :toggleModal="toggleModal" v-if="modalOpen"/>
  </transition>

  <div class="waitlist flex">
    <div class="waitlist-img">
      <img src="./assets/banner-img-2.svg" alt="">
    </div>

    <div  class="waitlist-form" id="scrollToMe" ref="scrollToMe">
      <span class="waitlist-title">Join our Waitlist</span>
      <p class="waitlist-para">and be the first to know when we launch</p>
      <transition-group name="form">
      <form @submit.prevent="submitForm">
         <p class="errorMessage" v-if="nameError">{{nameError}}</p>
         <p class="successMessage" v-else>{{successAction}}</p>
        <div class="input flex">
          <input type="text" v-model="firstname" placeholder="First Name">
        </div>
        <p class="errorMessage" v-if="emailError">{{emailError}}</p>
        <div class="input flex">
          <input type="text" v-model="email" placeholder="Email Address">
        </div>
        <div class="btn flex flex-column item-center">
          <button type="submit" class="banner-btn-2" style="background:#6F0977; color: #fff">
            <span v-if="loading">...Loading
            </span>
            <span v-else>Submit</span>
            </button>
        </div>
      </form>
      </transition-group>
    </div>
  </div>
  <div class="footer-banner flex item-center">
      <div class="footer-info">
        <p>
          Jaiyé is a lifestyle and entertainment sourcing platform that provides upscale
          event management services to young millennials in Nigeria.
        </p>
         <p>
           We are building the future of recreation and entertainment, one flex at a time.
         </p>
         <p>
           Join our waitlist to get updates on when we launch, and follow us via the links
          below.
         </p>

         <div class="flex item-center icons">
        <router-link to="https://instagram.com/jaiye.ng?utm_medium=copy_link"><img src="./assets/Group.svg" alt=""> </router-link>
         <img src="./assets/Vector.svg" alt="">
         <img src="./assets/icon1.svg" alt="">
       </div>

       </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Modal from './components/Modal.vue'

export default {
  name: 'App',
  data(){
    return {
      firstname: null,
      email: null,
      emailError: '',
      nameError: '',
      successAction: '',
      loading: false,
      modalOpen: true
    }
  },
  components: {
    Modal
  },
  methods: {
    async submitForm(){
      if(this.firstname < 2 || !this.firstname){
        this.nameError = "Name field can not be empty";
        return
      }
      this.nameError = '';
      if(this.email < 2 || !this.email){
        this.emailError = 'Email field can not be empty'
        return
      }
      this.emailError = ''
      this.successAction = "You're doing well"
        this.loading = true
        await axios.post('https://jaiyefeedback.herokuapp.com/api/users', {
          firstname : this.firstname,
          email: this.email
        }).then(response => {
          if(response.status === 200){
            this.modalOpen = true
            this.email = '';
            this.firstname = '',
            this.loading = false
          }
        })
        .catch(e => {
          if(e.response.status === 422){
            this.emailError = 'Invalid Email Address'
            this.loading = false
          }
        })
    },
    scrollToView(){
      const el = this.$refs['scrollToMe'];
      if(el){
        el.scrollIntoView({behavior: 'smooth'});
      }
    },
    toggleModal(){
      this.modalOpen = !this.modalOpen
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans&display=swap');

.app {
  position: relative;
  margin: 0;
  padding: 0;
}

/* transitions */
.modal-enter-active,
.modal-leave-active {
  transition: 0.8s ease all;
}

.modal-enter-from,
.modal-leave-to {
  transform: translateX(-700px);
}


.flex {
  display: flex;
}

.flex-column {
  flex-direction: column;
}

.item-center {
  justify-content: center;
  align-items: center;
}

.banner {
    background-color: #000000;
    background-image:  linear-gradient(rgba(0, 0, 0, 0.912), rgba(4, 1, 1, 0.7)), url('./assets/footer-banner.svg');
    height: 100vh;
}

.banner-img {
  height: 40%;
}

.banner-img img {
  height: auto;
  width: 60%;
}

.banner-btn-2 {
  margin-top: 1rem;
  padding: 1.5rem 3rem;
  border-radius: 60px;
  border: none;
  color: #fff;
  font-size: 20px;
  background: #6F0977;
  font-family: 'DM Sans', sans-serif;
  font-weight: 600;
  cursor: pointer;
  width: 250px;
}

.banner-btn {
  margin-top: 2rem;
  padding: 2rem 2rem;
  border-radius: 60px;
  border: none;
  color: #6F0977;
  font-size: 25px;
  background: #fff;
  font-family: 'DM Sans', sans-serif;
  font-weight: 600;
  cursor: pointer;
  width: 320px;
}

.waitlist {
  height: auto;
  background: #FFF3E7;
  padding: 5rem 3rem;
}

.success-title {
  font-size: 50px;
  font-family: 'DM Sans', sans-serif;
  color: #6F0977;
  text-align: center;
}

.success-info {
  text-align: center;
  font-family: 'DM Sans', sans-serif;
}

.successMessage {
  color: green;
  font-family: 'DM Sans', sans-serif;
  font-size: 15px;
}

.errorMessage {
  color: red;
  font-family: 'DM Sans', sans-serif;
  font-size:20px;
}

.waitlist-img {
  margin: 4rem 1rem;
  padding-right: 7rem;
}

.waitlist-img img {
  height: auto;
  width: 100%;
}

.waitlist-form {
  margin: 3rem 2rem;
  padding-top:5rem;
  /* padding-left: 3rem; */
  font-family: 'DM Sans', sans-serif;
  font-size: 40px;
  line-height: 32px;
}
.waitlist-title{
  font-family: 'DM Sans', sans-serif;
  font-size: 40px;
  line-height: 52px;
}
.waitlist-para {
   font-size: 20px;
}
.waitlist-form  {
  font-family: 'DM Sans', sans-serif;
  font-size: 20px;
  font-weight:400;
}
.input {
  padding-bottom: 2rem;
}

.input input {
  width: 500px;
  padding: 1.5rem 1rem;
  margin-bottom: 1rem;
  border: solid 3px #6F0977;
  background: transparent;
  outline: none;
  font-size: 20px;
}

.footer-banner {
  background-color: #000000;
  background-image: linear-gradient(rgba(0, 0, 0, 0.912), rgba(4, 1, 1, 0.7)), url('./assets/footer-banner.svg');
  height: 100vh;
}

.footer-info {
  padding-top: 1rem;
  width: 700px;
  text-align: center;

}

.footer-info p {
  text-align: center;
  color: #fff;
  line-height: 49px;
  font-size: 30px;
  font-family: 'DM Sans', sans-serif;
}

.icons img {
  padding-top: 2rem;
  padding-right: 4rem;
  width: 30px;
  height: 30px;
}


@media(max-width: 700px){
/* banner-heading */
  .banner {
    height: auto;
    padding: auto;
  }
  .banner-logo img {
    height: 200px;
    width: 200px;
  }

  .banner-img {
    height: 30%;
  }

  .banner-img img {
    height: auto;
    width: 100%;
  }

  .banner-btn {
    margin-bottom: 2rem;
    padding: 1.5rem 1.5rem;
    border-radius: 60px;
    border: none;
    color: #6F0977;
    font-size: 15px;
    background: #fff;
    font-family: 'DM Sans', sans-serif;
    font-weight: 400;
    cursor: pointer;
    width: 220px;
  }

.banner-btn-2 {
  margin-top: 1rem;
  padding: 1.5rem 2.5rem;
  border-radius: 60px;
  border: none;
  color: #fff;
  font-size: 15px;
  background: #6F0977;
  font-family: 'DM Sans', sans-serif;
  font-weight: 600;
  cursor: pointer;
  width: 180px;
}



  /* waitlist */
.waitlist {
  height: auto;
  background: #FFF3E7;
  padding: 3rem 0.8rem;
  flex-direction: column;
}

.waitlist-img {
  margin: 4rem 1rem;
  padding-right: 5.5rem;
}

.success-title {
  font-size: 30px;
  font-family: 'DM Sans', sans-serif;
  color: #6F0977;
  text-align: center;
}

.success-info {
  text-align: center;
  font-family: 'DM Sans', sans-serif;
  font-size: 25px;
}

.successMessage {
  color: green;
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
}

.errorMessage {
  color: red;
  font-family: 'DM Sans', sans-serif;
  font-size:12px;
}

.waitlist-img {
  margin: 0 auto;
  display: flex;
  justify-items: center;
  align-items: center;
}

.waitlist-img img {
  height: auto;
  width: 130%;
}

.waitlist-form {
  margin: 0rem 0rem;
  padding-top:2rem;
  padding-left: 0rem;
}

.waitlist-form span {
  font-family: 'DM Sans', sans-serif;
  line-height: 2px;
  display: flex;
  align-items: center;
  justify-content: center;
  /* margin-top: 2rem; */
}
.waitlist-title {
  font-size: 25px;
}
.waitlist-form p {
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
  font-weight:400;
  /* padding-bottom: 1rem; */
  text-align: center;
}
.input {
  padding-bottom: 1rem;
}

.input input {
  padding: 1.2rem 0.5rem;
  margin-bottom: 0.5rem;
  border: solid 3px #6F0977;
  background: transparent;
  outline: none;
  font-size: 20px;
}

/* footer */
.footer-banner {
  background-color: #000000;
  background-image: linear-gradient(rgba(0, 0, 0, 0.912), rgba(4, 1, 1, 0.7)), url('./assets/footer-banner.svg');
  height: 100vh;
}

.footer-info {
  padding-top: 1rem;
  width: 800px;
  text-align: center;

}

.footer-info p {
  text-align: center;
  color: #fff;
  line-height: 29px;
  font-size: 15px;
  padding: 0 0.5rem;
  font-family: 'DM Sans', sans-serif;
}

.icons img {
  padding-top: 1rem;
  padding-right: 2rem;
  width: 30px;
  height: 30px;
}
}
</style>
