<template>
     <div class="container">
        <Header />
        <img class="banner-image"
             src="https://firebasestorage.googleapis.com/v0/b/dynamaze---pwa-1550221436823.appspot.com/o/public%2Fimg%2Fdeals%2Ffunfussball1.png?alt=media&token=448599c6-ffb7-43f4-afc9-dfb82ce426f0" 
        />
     </div>
</template>

<script>
import Header from '@/components/Header.vue';

export default {
    name: 'App',
    components: {Header},
    components: {
    },
    data: function() {
        return {
            timeSlots: [
                {
                    id: "1",
                    ticketAmount: 10,
                    begin: "10:00",
                    price: {
                        amount: 10,
                        currency: "EUR",
                    },
                },
                {
                    id: "2",
                    ticketAmount: 5,
                    begin: "11:00",
                    price: {
                        amount: 10,
                        currency: "EUR",
                    },
                },
            ],
            selectedTickets:[],
            currency:'EUR',
        };
    },
};
</script>

<style>
#app {
    font-family: Montserrat;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 5px;
}
body{
    width: 100%;
    max-width: 375px;
    margin: 0;
    padding: 0;
}
.container{
    display: flex;
    flex-direction: column;
    height: 100vh;
}
.banner-image{
    margin-top: 6px;
    border-radius: 20px 0px;
    width: 100%;
    height: auto;
}
</style>
