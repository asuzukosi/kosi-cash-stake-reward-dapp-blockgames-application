<template class="font-mono">
  <div class="home bg-gradient-to-r from-cyan-500 to-blue-500 h-screen justify-center items-center">
    <div v-if="!isConnected">
      <WelcomeMessage v-if="hasMetaMask" @connectwallet="connectWallet"/>
      <NoMetaMask v-else/>
    </div>
    <div v-else>
      <StakingBoard :account="account" :balance="totalBalance" :stake="totalStake" :reward="totalReward" @transfertoken="transferToken" @buytoken="buyToken" @staketoken="stakeToken" @removestake="removeStake" @withdrawreward="withdrawReward"/>
    </div>
    </div>
    
  </template>

  <script>
// @ is an alias to /src
import WelcomeMessage from '@/components/WelcomeMessage.vue'
import NoMetaMask from '@/components/NoMetaMask.vue'
import StakingBoard from '@/components/StakingBoard.vue'
import ContractABI from '../contracts/artifacts/StakeRewardToken.json'

import Web3 from 'web3/dist/web3.min.js'
import { withDirectives } from '@vue/runtime-core'


export default {
  name: 'HomeView',
  components: {
    WelcomeMessage,
    NoMetaMask,
    StakingBoard
  },

  data(){
    return {
      hasMetaMask: false,
      isConnected: false,
      contractAbi: ContractABI,
      account: '',
      totalBalance: 0,
      totalStake: 0, 
      totalReward: 0,
      errorMessage: '',
      hasError: false,
      successMessage: '',
      hasSuccess: false,
    }
  },

  methods: {
    async connectWallet(){
      console.log("connecting wallet...")
      const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' })
      this.account = accounts[0]
      this.isConnected = true

      this.web3 = new Web3(window.ethereum)
      this.web3.eth.defaultAccount = this.account
      this.getAllAccountInfo()
    },

    async getAllAccountInfo(){
      console.log("getting all account information...")
      let stakeRewardTokenContract = new this.web3.eth.Contract(this.contractAbi, '0xEff33Efb13C5503cE8ed7EF692Ce4ec1221e4866')
      this.totalBalance = await stakeRewardTokenContract.methods.balanceOf(this.account).call() / (10**18)
      this.totalStake = await stakeRewardTokenContract.methods.stakeOf(this.account).call() / (10**18)
      this.totalReward = await stakeRewardTokenContract.methods.rewardOf(this.account).call() / (10**18)
    },

    async buyToken(amount_in_kch){
      console.log("buying Kosi cash token of amount " + amount_in_kch)
      let kshs_in_wei = (amount_in_kch/1000) * (10 ** 18) // the value of the requested amount of Kosi cash token is wei
      let stakeRewardTokenContract = new this.web3.eth.Contract(this.contractAbi, '0xEff33Efb13C5503cE8ed7EF692Ce4ec1221e4866')
      try {
          stakeRewardTokenContract.methods.buyToken().send({"from": this.account, "value": kshs_in_wei})
          this.successMessage = "Successfully bought/minted new Kosi Cash tokens into your address"
          this.hasSuccess = true
          setTimeout(() => {
          this.successMessage = ""
          this.hasSuccess = false
        }, 5000)

      } catch (e) {
        this.errorMessage = e.message
        this.hasError = true
        setTimeout(() => {
          this.errorMessage = ""
          this.hasError = false
        }, 5000)

      }
      this.getAllAccountInfo()
    },

    async transferToken(receiver, amount){
      console.log("Transfering "+amount+"  amount of KosiCash to "+receiver)
      let kshs = amount* (10 ** 18) // the value of the requested amount of Kosi cash token is wei
      let stakeRewardTokenContract = new this.web3.eth.Contract(this.contractAbi, '0xEff33Efb13C5503cE8ed7EF692Ce4ec1221e4866')
      try {
          stakeRewardTokenContract.methods.transfer(receiver, kshs).send({"from": this.account})
          this.successMessage = "Successfully transfered some of your KosiCash token to another address"
          this.hasSuccess = true
          setTimeout(() => {
          this.successMessage = ""
          this.hasSuccess = false
        }, 5000)

      } catch (e) {
        this.errorMessage = e.message
        this.hasError = true
        setTimeout(() => {
          this.errorMessage = ""
          this.hasError = false
        }, 5000)

      }
      this.getAllAccountInfo()
    }, 

    async stakeTokens(amount){
      console.log("Staking "+amount + " amount of tokens")
      let kshs = amount* (10 ** 18) // the value of the requested amount of Kosi cash token is wei
      let stakeRewardTokenContract = new this.web3.eth.Contract(this.contractAbi, '0xEff33Efb13C5503cE8ed7EF692Ce4ec1221e4866')

      try {
          stakeRewardTokenContract.methods.createStake(kshs).send({"from": this.account})
          this.successMessage = "Successfully staked some of your KosiCash tokens"
          this.hasSuccess = true
          setTimeout(() => {
          this.successMessage = ""
          this.hasSuccess = false
        }, 5000)

      } catch (e) {
        this.errorMessage = e.message
        this.hasError = true
        setTimeout(() => {
          this.errorMessage = ""
          this.hasError = false
        }, 5000)

      }
      this.getAllAccountInfo()
    },

    async withdrawReward(){
       console.log("Cashing out rewards from staked tokens")
       let stakeRewardTokenContract = new this.web3.eth.Contract(this.contractAbi, '0xEff33Efb13C5503cE8ed7EF692Ce4ec1221e4866')
       
       try {
          stakeRewardTokenContract.methods.withdrawReward().send({"from": this.account})
          this.successMessage = "Successfully withdrew all your staked tokens"
          this.hasSuccess = true
          setTimeout(() => {
          this.successMessage = ""
          this.hasSuccess = false
        }, 5000)

      } catch (e) {
        this.errorMessage = e.message
        this.hasError = true
        setTimeout(() => {
          this.errorMessage = ""
          this.hasError = false
        }, 5000)

      }
      this.getAllAccountInfo()
    },

    async removeStake(amount){
      console.log("Removing "+amount + " amount of tokens from stake")
      let kshs = amount* (10 ** 18) // the value of the requested amount of Kosi cash token is wei
      let stakeRewardTokenContract = new this.web3.eth.Contract(this.contractAbi, '0xEff33Efb13C5503cE8ed7EF692Ce4ec1221e4866')

      try {
          stakeRewardTokenContract.methods.removeStake(kshs).send({"from": this.account})
          this.successMessage = "Successfully removed some of your KosiCash token stake"
          this.hasSuccess = true
          setTimeout(() => {
          this.successMessage = ""
          this.hasSuccess = false
        }, 5000)

      } catch (e) {
        this.errorMessage = e.message
        this.hasError = true
        setTimeout(() => {
          this.errorMessage = ""
          this.hasError = false
        }, 5000)

      }
      this.getAllAccountInfo()
    }
    

  },

  mounted(){
    if (typeof window.ethereum !== 'undefined'){
      this.hasMetaMask = true

    }
  }
}
</script>
