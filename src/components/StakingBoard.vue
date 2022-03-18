<template>
  <div class="block pt-10 pb-20" >
      <h1 class="text-white font-bold text-4xl font-mono">KosiCashStaker for <span class="text-2xl">{{account}}</span></h1>
    </div>
    <div>
        <h1 class="text-white font-bold text-2xl font-mono mb-2">Your KosiCash Balance: {{balance}}</h1>
        <h1 class="text-white font-bold text-2xl font-mono mb-2">Your KosiCash Stake: {{stake}}</h1>
        <h1 class="text-white font-bold text-2xl font-mono mb-2">Your KosiCash Rewards: {{reward}}</h1>
    </div>
    <div class="w-80 ml-auto mr-auto">
         <div class="flex w-90 mt-4">
                <input v-model="transferAddress" type="text" class="h-20 mt-2 mr-3 rounded text-center" placeholder="receiver address">
                <input v-model="transferAmount" type="number" class="h-20 mt-2 mr-3 rounded">
                <div class="h-10 w-60 p-10 bg-gradient-to-r from-indigo-500 hover:from-pink-500 via-purple-500 to-pink-500 hover:to-indigo-500 justify-center flex items-center text-white font-bold rounded mt-3 mb-3" @click="$emit('transfertoken', transferAddress, transferAmount)">Transfer Tokens</div>
          </div>
          <div class="flex w-90 mt-4">
                <input v-model="buyAmount" type="number" class="h-20 mt-2 mr-5 rounded">
                <div class="h-10 w-60 p-10 bg-gradient-to-r from-indigo-500 hover:from-pink-500 via-purple-500 to-pink-500 hover:to-indigo-500 justify-center flex items-center text-white font-bold rounded mt-3 mb-3" @click="$emit('buytoken', buyAmount)">Buy Tokens</div>
          </div>

          <div class="flex w-90 mt-4">
                <input v-model="stakeAmount" type="number" class="h-20 mt-2 mr-5 rounded">
                <div class="h-10 w-60 p-10 bg-gradient-to-r from-indigo-500 hover:from-pink-500 via-purple-500 to-pink-500 hover:to-indigo-500 justify-center flex items-center text-white font-bold rounded mt-3 mb-3" @click="$emit('staketoken', stakeAmount)">Stake Tokens</div>
          </div>

          <div class="flex w-90 mt-4">
                <input v-model="removeStakeAmount" type="number" class="h-20 mt-2 mr-5 rounded">
                <div class="h-10 w-60 p-10 bg-gradient-to-r from-indigo-500 hover:from-pink-500 via-purple-500 to-pink-500 hover:to-indigo-500 justify-center flex items-center text-white font-bold rounded mt-3 mb-3" @click="$emit('removestake', removeStakeAmount)">Remove Staked Tokens</div>
          </div>

          <div class="h-10 w-60 p-10 bg-gradient-to-r from-indigo-500 hover:from-pink-500 via-purple-500 to-pink-500 hover:to-indigo-500 justify-center flex items-center text-white font-bold rounded mt-6 mb-3" @click="$emit('withdrawreward')">Withdraw Token Rewards</div>
          

    </div>
</template>

<script>
export default {
    name: "Staking Board",
    props: ["account", "balance", "stake", "reward"],

    data(){
        return {
            transferAddress: "",
            transferAmount: 0,
            stakeAmount: 0,
            buyAmount: 0,
            removeStakeAmount: 0,
        }

    }
}
</script>
