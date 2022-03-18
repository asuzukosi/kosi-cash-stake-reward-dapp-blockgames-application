<template>
  <div class="block pt-10 pb-40" >
      <h1 class="text-white font-bold text-4xl font-mono mb-20">Welcome to KosiCashStaker staking application for the Blockgames program</h1>
      <h5 class="text-white font-bold text-2xl font-mono" >connect to your web3 meta mask wallet to begin.........</h5>
    </div>
    <div class="w-80 ml-auto mr-auto">
          <div class="h-10 w-30 p-10 bg-gradient-to-r from-indigo-500 hover:from-pink-500 via-purple-500 to-pink-500 hover:to-indigo-500 justify-center flex items-center text-white font-bold rounded" @click="$emit('connectwallet')">Connect to MetaMask</div>
    </div>
</template>

<script>
export default {
    name: "Welcome Message"
}
</script>
