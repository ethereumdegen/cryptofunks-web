<template>

<div>

   <div class="section  bg-gray-200  border-b-4 border-black px-0 lg:px-1">

     <div class=" ">
        <Navbar 
        v-bind:web3Plug="web3Plug"
        
       />
     </div>


   </div>

  

   <div class="section  border-b-2 border-black" style="background:#1d1d1d;">
     <div class=" ">
        
       <div class="  flex  flex-col   ">
 
         <div class="   text-center">
  
            <img src="/CryptoFunksLP.jpg" class="p-8" style="margin:0 auto;" />

         </div>
         <div class="   w-full  text-center ">
          
            Audio 
             <audio controls class="m-8">
              
              <source src="/CryptoFunks_LP.mp3" type="audio/mpeg">
                 Your browser does not support the audio element.
            </audio>

         </div>
       </div>
     </div>
   </div>


    <div class="section  text-white  border-b-2 border-black " style="background:#222;">
     <div class="w-container  ">

         

          <div class=" w-2/3  mt-8 py-8" style="margin: 0 auto;">
           
                <div class="text-2xl text-center"> Tracklist </div>
 

                <ul class=" "> 
                    <li class="my-4"> 0:00 - Dreamsound </li>
                    <li class="my-4" > 2:03 - Send It (Remix) </li> 
                    <li class="my-4" > 3:56 - Jackdaw </li>
                    <li class="my-4" > 5:39 - Darkstarr </li>
                    <li class="my-4" > 7:10 - Soulsphere </li>
                    <li class="my-4" > 8:16 - Thembeats </li>
                    <li class="my-4" > 9:09 - Ghost </li>
                    <li class="my-4" > 10:45 - Phantomlancer </li>
               </ul>

                
         </div>

        
         


     </div>
   </div>


    <div class="section  text-white  border-b-2 border-black " style="background:#222;">
     <div class="w-container  ">

         

          <div class=" w-2/3  mt-8 py-8" style="margin: 0 auto;">
           
                <div class="text-2xl text-center hidden"> Mint your own copy </div>

                <p class="text-xs my-8">InfernalToast is an Ethereum Solidity developer and Cryptopunk since 2018. Owner of Punk 1164, he launched the first PoW mineable token to Mainnet on Feb 6, 2018 spawning a large community of degens and the Miners Guild DAO.  </p>
            <p class="text-xs my-8">In June 2021 he launched this limited run of Music NFTs named 'CryptoFunks' containing original work that he produced with FL Studio and Serum while locked down. Music for degens. </p>


                <div class=" my-4 mb-16 text-center">
                   <a href="https://opensea.io/collection/cryptofunks-lp" target="_blank" class="text-xs text-center"> View the Collection on OpenSea </a>
                </div>

                <div v-if="!connectedToWeb3">

                   <div class="text-md my-2"> Please connect to Web3  </div>
 
                  <div  @click="connectToWeb3" class="button bg-blue-500 hover:bg-blue-700 text-sm text-black font-bold my-2 py-1 px-2 rounded cursor-pointer inline ">Login with Web3</div>


                </div>

                 <div v-if="connectedToWeb3">

                  <div class="text-md my-2">   {{ ( amountMinted) }}  of 420 minted. </div>

                 <div class="text-md my-2"> Minting price: 1 <a href="https://0xbitcoin.org" target="_blank">0xBTC</a> </div>
 

                <div   @click="mintAlbum()" class="bg-purple-500 hover:bg-purple-600 my-8 p-4 rounded text-center cursor-pointer"> Mint 'Cryptofunks LP' </div>
            </div>
        </div>
            
         


     </div>
   </div>



    
  <Footer/>

</div>
</template>


<script>



import Web3Plug from '../js/web3-plug.js'  

 
import FrontPageMedia from './components/FrontPageMedia.vue';
 
import Navbar from './components/Navbar.vue';
 
import Footer from './components/Footer.vue';
import TabsBar from './components/TabsBar.vue';
  
import StarflaskAPIHelper from '../js/starflask-api-helper.js';
import FrontendHelper from '../js/frontend-helper.js';


const albumContractABI = require('../../src/contracts/cryptofunks.json')

export default {
  name: 'Home',
  props: [],
  components: {Navbar, Footer, TabsBar, FrontPageMedia },
  data() {
    return {
      web3Plug: new Web3Plug()  ,
      amountMinted: 0,
      connectedToWeb3: false
       

      
    }
  },

  created(){

 
    this.web3Plug.getPlugEventEmitter().on('stateChanged', function(connectionState) {
        console.log('stateChanged',connectionState);
         
        this.activeAccountAddress = connectionState.activeAccountAddress
        this.activeNetworkId = connectionState.activeNetworkId 

        this.connectedToWeb3 = this.web3Plug.connectedToWeb3()

           this.findAmountMinted()
         
      }.bind(this));
   this.web3Plug.getPlugEventEmitter().on('error', function(errormessage) {
        console.error('error',errormessage);
         
        this.web3error = errormessage
       
      }.bind(this));

      this.web3Plug.reconnectWeb()
   
       
     
  },
  mounted: function () {
    //  setInterval(this.pollTokens.bind(this), 5000)
      this.connectedToWeb3 = this.web3Plug.connectedToWeb3()  
    this.findAmountMinted()
    
  }, 
  methods: {
          
          connectToWeb3(){
            this.web3Plug.connectWeb3( )
          },

          getRouteTo(dest){
            return FrontendHelper.getRouteTo(dest)
          },

          approveTokens(){

            const approveAmount = '2100000000000000' //21 million 



            console.log('mint album ')
          },

           async findAmountMinted(){
               

             const albumContract = this.web3Plug.getCustomContract(albumContractABI, '0x232ac5da68f539132f181306b6357363e1496918')


             this.amountMinted = await albumContract.methods.totalSupply().call();

             console.log('amountMinted',this.amountMinted)
           },

          async mintAlbum(){

            let senderAddress = this.web3Plug.getActiveAccountAddress()

            let albumContract = '0x232ac5da68f539132f181306b6357363e1496918'

            let approveAmount = '100000000'

            const tokenContract = this.web3Plug.getTokenContract('0xb6ed7644c69416d67b522e20bc294a9a9b405b31')

            let success = await tokenContract.methods.approveAndCall(albumContract,approveAmount,'0x0' ).send({from: senderAddress })  

            console.log('response:', success)
          } 

       
 

  }
}
</script>
