<template>
    <div class="container">
        <Header />
        <img class="banner-image"
             src="https://firebasestorage.googleapis.com/v0/b/dynamaze---pwa-1550221436823.appspot.com/o/public%2Fimg%2Fdeals%2Ffunfussball1.png?alt=media&token=448599c6-ffb7-43f4-afc9-dfb82ce426f0" 
        />
        <div class="time-box-container">
            <div 
                :id="time.id"
                :key="time" 
                class="time-box" 
                v-for="time in timeSlots"
                @click="selectTime"
            >
                <span> {{ time.begin }} </span>
                <span class="seperator"></span>
                <span> {{ time.price.amount }} {{ time.price.currency }} </span>
            </div>
        </div>
        <p class="text">Nur noch {{ ticketNumber }} Tickets vorhanden.</p>
        <div class="ticket-select-container">
            <button class="button"
                    @click="decreaseTicketNumber"
            >
                -
            </button>
            <span> {{ ticketNumber }} Tickets</span>
            <button class="button" 
                    @click="increaseTicketNumber"
            >
                +
            </button>
        </div>
        <span class="total-price"> {{ totalPrice }} {{currency}}</span>
        <span class="buy-button"
              @click="buy"
        >
            Jetzt Buchen
        </span>
    </div>
</template>

<script>
import Header from '@/components/Header.vue';

export default {
    name: "App",
    components: {Header},
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
                {
                    id: "3",
                    ticketAmount: 5,
                    begin: "12:00",
                    price: {
                        amount: 15,
                        currency: "EUR",
                    },
                },
            ],
            selectedTickets:[],
            currency:'EUR',
            selectedTicket:null,
            selectedSlot:null,
        };
    },
    computed:{
        totalPrice(){
            return this.selectedTickets.length>0 ? this.selectedTickets.map(item => item.count*item.price).reduce((prev, next) => prev + next, 0) : 0;
        },
        ticketNumber(){
            return this.selectedTickets.length>0 ? this.selectedTickets.map(item => item.count).reduce((prev, next) => prev + next, 0) : 0;
        },
    },
    methods:{
        increaseTicketNumber(){
            if(document.querySelector('.selected')){
                
                this.findSelectedSlot()
                let price=this.selectedSlot.price.amount;

                // if there are some tickets added before with same id
                if(this.selectedTicket && this.selectedTicket.count<this.selectedSlot.ticketAmount) 
                     this.selectedTicket.count++;
                // add first ticket for this slot
                else if(!this.selectedTicket)
                    this.selectedTickets.push({id:this.selectedSlot.id,count:1,price:price});
            }
            console.log(this.selectedTickets)

        },
        decreaseTicketNumber(){
            if(document.querySelector('.selected'))
            {
                this.findSelectedSlot()

                // if there are some tickets added before with same id
                
                if(this.selectedTicket && this.selectedTicket.count>0) 
                    this.selectedTicket.count--;
            
            }
        },
        // get selections
        findSelectedSlot(){
            const selectedTicketId=document.querySelector('.selected').id;
            this.selectedTicket=this.selectedTickets.find(x => x.id === selectedTicketId);
            this.selectedSlot=this.timeSlots.find(x => x.id === selectedTicketId);
        },
        // selected timeSlot changes
        selectTime(e){
            const selectedBox = e.target.classList.contains('time-box') ? e.target : e.target.parentElement ;

            this.resetBoxSelection();
            selectedBox.classList.add('selected');
            this.findSelectedSlot()
        },
        resetBoxSelection:function(){
            document.querySelectorAll('.time-box').forEach(box=>{
                box.classList.remove('selected');
            });
        },
        buy:function(){
            let alertText="Booking ";

            this.selectedTickets.forEach(ticket=>{
                const t=this.timeSlots.find(x => x.id === ticket.id);
                
                alertText+="at "+t.begin+", "+ticket.count+" tickets at "+ticket.count*ticket.price+this.currency+"; ";
            });
            alertText+="succeeded.";
            alert(alertText);
        },
    },
};
</script>

<style lang='scss'>
@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');
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
.time-box-container{
    display: flex;
    align-items: center;
    justify-content: space-around;
    margin-top: 20px;
    margin-bottom: 35px;

    .time-box{
        width: 70px;
        height: 60px;
        border-radius: 4px;
        font-style: normal;
        font-weight: 500;
        font-size: 16px;
        line-height: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        color: #55C57A;
        background:url('./assets/box-back.svg');
        cursor: pointer;
    }
    .selected{
        background: linear-gradient(225.08deg, #7ED56F 15.51%, #55C57A 100%);
        color: #FFFFFF;
        border:none;
        .seperator{
            border-top: 1px solid #fff;
        }
    }
    .seperator{
        height: 1;
        width: 56px;
        border-top: 1px solid #55C57A;
    }
}
.text{
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 24px;
    color: #222222;
}
.ticket-select-container{
    display: flex;
    align-items: center;
    justify-content: space-around;
    .button{
        display: flex;
        align-items: center;
        justify-content: center;
        width: 56px;
        height: 40px;
        background:url('./assets/button-back.svg');
        border:none;
        font-weight: 700;
        font-size: 30px;
        cursor: pointer;
    }
}
.total-price{
    font-style: normal;
    font-weight: bold;
    font-size: 18px;
    line-height: 22px;
    color: #55C57A;
    margin-top: 16px;
}
.buy-button{
    display: flex;
    align-items: center;
    justify-content: center;
    margin: auto auto 37px;
    width: 90%;
    padding: 9px 0;
    background: linear-gradient(87.6deg, #FF9232 -28.36%, #FAC162 104.8%);
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    line-height: 20px;
    color: #FFFFFF;
    cursor: pointer;
}
</style>
