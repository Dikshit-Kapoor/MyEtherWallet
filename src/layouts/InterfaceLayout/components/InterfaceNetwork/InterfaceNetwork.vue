<template>
  <div class="info-block-container">
    <interface-network-modal ref="network" />
    <div class="info-block network">
      <div class="block-image">
        <div class="network-type">
          <div class="icon-block">
            <img :src="network.type.icon" alt class="icon" />
          </div>
        </div>
      </div>
      <div class="block-content">
        <div class="information-container">
          <div class="title-and-helper">
            <h2>{{ $t('common.network') }}</h2>
          </div>
          <p v-if="account.identifier !== identifier">
            {{ network.service + '(' + network.type.name + ')' }}
          </p>
          <p v-else>{{ 'Web3 Provider' + '(' + network.type.name + ')' }}</p>
          <p>
            {{ $t('interface.network-modal.last-block') }}# :
            <span v-show="parsedNetwork !== ''"> {{ parsedNetwork }}</span>
            <i v-show="parsedNetwork === ''" class="fa fa-spinner fa-spin" />
          </p>
        </div>
        <div class="icon-container">
          <button
            v-if="account.identifier !== identifier"
            id="networkModal"
            class="change-button"
            @click="networkModalOpen"
          >
            {{ $t('interface.network-modal.button-change') }}
          </button>
          <b-popover
            :content="$t('interface.network-modal.popup-open-networks')"
            target="networkModal"
            placement="top"
            triggers="hover"
            title=""
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import InterfaceNetworkModal from '../InterfaceNetworkModal';
import { mapState } from 'vuex';
import { WEB3_WALLET } from '@/wallets/bip44/walletTypes';

export default {
  components: {
    'interface-network-modal': InterfaceNetworkModal
  },
  props: {
    blockNumber: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      parsedNetwork: 0,
      identifier: WEB3_WALLET
    };
  },
  computed: {
    ...mapState('main', ['network', 'account', 'web3'])
  },
  watch: {
    blockNumber(newVal) {
      this.parsedNetwork = parseInt(newVal);
    }
  },
  mounted() {
    if (this.blockNumber && this.blockNumber !== undefined) {
      this.parsedNetwork = parseInt(this.blockNumber);
    }
  },
  methods: {
    networkModalOpen() {
      if (this.account.identifier !== this.identifier) {
        this.$refs.network.$refs.network.show();
      }
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'InterfaceNetwork.scss';
</style>
