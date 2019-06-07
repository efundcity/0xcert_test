<template>

  <v-btn
    color="primary"
    @click="getTest"
  >
    Check
    <v-icon
      class="orange--text">
      keyboard_arrow_right
    </v-icon>
  </v-btn>
    
</template>

<script>
import { Cert } from '@0xcert/cert'
import { schema88 } from '@0xcert/conventions'
import { AssetLedger } from '@0xcert/ethereum-asset-ledger'
import { Mutation } from '@0xcert/ethereum-metamask-provider'
import { MetamaskProvider } from '@0xcert/ethereum-metamask-provider'

export default {
  components: {},
  props: {
    item: {
      type: Object,
      default: null
    }
  },
  data: () => ({}),
  computed: {
    address_url: function() {
      return `https://maps.google.com/maps?q=
      ${this.$props.item.address}
      &t=m&z=10&ie=UTF8&iwloc=&output=embed&height=100%`
    },
    imgs_list: function() {
      var ret = [this.$props.item.image]
      if (this.$props.item.image_1 != '') ret.push(this.$props.item.image_1)
      if (this.$props.item.image_2 != '') ret.push(this.$props.item.image_2)
      if (this.$props.item.image_3 != '') ret.push(this.$props.item.image_3)
      if (this.$props.item.image_4 != '') ret.push(this.$props.item.image_4)
      if (this.$props.item.image_5 != '') ret.push(this.$props.item.image_5)
      return ret
    }
  },
  methods: {
    async getTest() {
      const provider = new MetamaskProvider({
        gasPriceMultiplier: 2.1,
        sandbox: true
      })
      await provider.enable()
      const isSupported = provider.isSupported()
      const isEnabled = await provider.isEnabled()
      const accountId = provider.accountId
      console.log('Is metamask supported:', isSupported)
      console.log('Is metamask enabled:', isEnabled)
      console.log('accountId:', accountId)
      const recipe = {
        name: 'Fundin',
        symbol: 'din',
        uriBase: 'http://www.fundin.us',
        schemaId:
          '0x3f4a0870cd6039e6c987b067b0d28de54efea17449175d7a8cd6ec10ab23cc5d', // base asset schemaId
        capabilities: [3, 1]
      }
      console.log(provider)
      const mutation = await AssetLedger.deploy(provider, recipe).then(
        mutation => {
          return mutation.complete() // wait until first confirmation
        }
      )
      console.log('mutation:', mutation)
    },
    async connect() {
      await this.accountIdprovider.enable()
      const isSupported = this.$0xcert.provider.isSupported()
      const isEnabled = await this.$0xcert.provider.isEnabled()
      console.log('Is metamask supported:', isSupported)
      console.log('Is metamask enabled:', isEnabled)
      if (isSupported && isEnabled) return true
      else return false
    },
    async getData() {
      const state = await this.connect()
      if (!state) {
        console.log('MetaMask Not Ready!!')
      } else {
        const provider = this.$0xcert.provider
        const matches = provider.isCurrentAccount(provider.accountId)
        console.log('accountId:', provider.accountId)
        console.log('isCurrentAccount:', matches)
        this.deploy()
      }
    },
    async deploy() {
      const client = this.$0xcert
      const provider = this.$0xcert.provider
      const ledgerId = provider.accountId
      //const ledger = new AssetLedger(provider, ledgerId)
      //const ledgerInfo = await ledger.getInfo()
      //consloe.log(ledgerInfo)
      const capabilities = [3, 1]
      const recipe = {
        name: 'Fundin',
        symbol: 'din',
        uriBase: 'http://www.fundin.us',
        schemaId:
          '0x3f4a0870cd6039e6c987b067b0d28de54efea17449175d7a8cd6ec10ab23cc5d', // base asset schemaId
        capabilities: capabilities
      }
      const mutation = await AssetLedger.deploy(provider, recipe).then(
        mutation => {
          return mutation.complete() // wait until first confirmation
        }
      )
      // const mutation = await client.deployAssetLedger({
      //   name: 'fundin',
      //   symbol: 'din',
      //   uriBase: 'http://www.fundin.us',
      //   schemaId:
      //     '0x3f4a0870cd6039e6c987b067b0d28de54efea17449175d7a8cd6ec10ab23cc5d',
      //   capabilities: capabilities
      // })
      // await mutation.complete()
      // console.log(mutation)
      // console.log(client.provider.requiredConfirmations)
      console.log('Contract deployed:', {
        tx: mutation.id,
        address: mutation.receiverId
      })
    }
  }
}
</script>

<style>
</style>
