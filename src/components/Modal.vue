<template>
  <div class="modal flex item-center" @click="checkClick" ref="appWrap">
      <div class="modal-content">
          <div class="flex item-center">
              <img src="../assets/smile.svg" alt="">
          </div>
          <p>You’re now officially on our waitlist </p>
          <p> Stay tuned and jiggy</p>
      </div>
  </div>
</template>

<script>
export default {
    name:"Modal",
    props: [
        'toggleModal'
    ],
    methods: {
        checkClick(e) {
            if(e.target === this.$refs.appWrap){
                this.toggleModal()
            }
        }
    }

}
</script>

<style>
    .modal {
        z-index: 100;
        top: 0;
        position: fixed;
        justify-content: center;
        align-items: center;
        height: 100vh;
        width: 100%;
        background-color: rgba(0, 0, 0, 0.851);
    }

    .modal-content img {
        padding-top: 2rem;
        height: 250px;
        width: 250px;
    }
    
    .modal-content {
        padding: 48px 32px;
        width: 590px; 
        height: 384px;
        border-radius: 15px;
        background: #6F0977;
        color: #fff;
    }

    .modal-content p {
        text-align: center;
        font-weight: normal;
        font-size: 25px;
        color: #fff;
        font-family: 'DM Sans', sans-serif;
    }

    @media(max-width: 700px){
    .modal-content img {
        padding-top: 2rem;
        height: 150px;
        width: 150px;
    }
    
    .modal-content {
        margin-right: 1rem;
        padding: 48px 32px;
        width: 100%; 
        height: auto;
        border-radius: 12px;
        background: #6F0977;
        color: #fff;
    }

    .modal-content p {
        text-align: center;
        font-weight: normal;
        font-size: 15px;
        color: #fff;
        font-family: 'DM Sans', sans-serif;
    }
    }
</style>