<template>
  <v-container>
    <div class="text-center">
      <!-- Metamask status -->
      <template v-if="notEthBrowser">
        <v-layout align-center justify-center>
          <v-banner name="Install Metamask" class="elevation-13" single-line>
            <v-avatar
            slot="icon"
            color="red"
            size="40">
              <v-icon
                icon="mdi-lock"
                color="white">
                mdi-account-circle
              </v-icon>
            </v-avatar>
            Seems like you don't have Metamask Chrome extention.
            <template v-slot:actions>
              <v-btn 
                href="https://metamask.io/"
                text
                target="_blank"
                color="info"
                rounded>
                <span class="mr-2">Install Metamask</span>
              </v-btn>
            </template>
          </v-banner>
        </v-layout>
      </template>
      <template v-else>
        <!-- account is locked  -->
        <template v-if="ethBrowserButNotConnected"> 
          <v-layout align-center justify-center>
            <v-btn rounded color="pink" @click="connectToMetamask">Connect your Metamask</v-btn>
          </v-layout>
        </template>
        <!-- Everything ready to go -->
        <template v-else>
          <v-layout align-center justify-center>
            <h1>Connected and ready to go!</h1>
          </v-layout>
          <v-layout align-center justify-center>
            <h3>Your address: {{ userAddress }}</h3>
          </v-layout>
        </template>
      </template>
      <br><br>

      <!-- steppers -->
      <template v-if="userAddress">
        <v-stepper v-model="e1">
          <v-stepper-header>
            <v-stepper-step :complete="e1 > 1" step="1">Positive Sum</v-stepper-step>
            <v-divider></v-divider>
            <v-stepper-step :complete="e1 > 2" step="2">Better Coordination</v-stepper-step>
            <v-divider></v-divider>
            <v-stepper-step :complete="e1 > 3" step="3">Your pledge 3</v-stepper-step>
            <v-divider></v-divider>
            <v-stepper-step step="4">Your pledge 4</v-stepper-step>
          </v-stepper-header>

          <v-stepper-items>
            <v-stepper-content step="1">
                <div class="positive-sum">
                  <v-layout align-center justify-center>
                    <h1 class="font-weight-light">
                      Value creation and additions into the world, by building new tools and Dapps for Individuals. #BUIDL
                    </h1>
                  </v-layout>
                </div>
                <br>

                <v-layout align-center justify-center>
                  <v-btn outlined rounded color="primary" @click="e1 = 2">All in</v-btn>
                </v-layout>
            </v-stepper-content>

            <v-stepper-content step="2">
              <div class="Better Coordination">
                <v-layout justify-center>
                  <h1 class="font-weight-light">
                    Coordination faluires are sources of conflict, tragedies and suffering. Let's build new communities and DAOs to provide solutions for better cooperation, less corruption, less wage slavery, less tyranny.
                  </h1>
                </v-layout>
                <br>

                <v-layout align-center justify-center>
                  <v-btn outlined rounded color="primary" @click="e1 = 3">All in</v-btn>
                </v-layout>
              </div>
            </v-stepper-content>

            <v-stepper-content step="3">
              <div class="pledge3">
                <v-layout align-center justify-center>
                  <h1 class="font-weight-light">
                    Write your 3rd pledge then hit enter.
                  </h1>
                </v-layout>
                <br>

                <v-card>
                  <form @submit.prevent>
                    <v-toolbar color="blue lighten-4">
                      <v-flex>
                        <v-text-field
                          label="Write your 3rd pledge then hit enter. [next step]"
                          v-model="pledge3"
                          @keyup.13.prevent="e1 = 4">	
                        </v-text-field>
                      </v-flex>
                    </v-toolbar>
                  </form>
                </v-card>
              </div>
              <br>
            </v-stepper-content>

            <v-stepper-content step="4">
              <div class="pledge4">
                <v-layout align-center justify-center>
                  <h1 class="font-weight-light">
                    Write your 4th pledge then hit enter.
                  </h1>
                </v-layout>
                <br>

                <v-card>
                  <form @submit.prevent>
                    <v-toolbar color="blue lighten-4">
                      <v-flex>
                        <v-text-field
                          label="Write your 4th pledge then hit enter. [next step]"
                          v-model="pledge4"
                          @keyup.13.prevent="signPledge">	
                        </v-text-field>
                      </v-flex>
                    </v-toolbar>
                  </form>
                </v-card>
              </div>
              <br>
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
      </template>

      <br><br><br>

      <template v-if="userSignature">
        <v-card outlined>
          <v-card-text>
            Your signature: {{ userSignature }}
          </v-card-text>
        </v-card>
      </template>

    </div>
  </v-container>
</template>

<script>
const { ethers } = require('ethers');

let provider, signer;

// User has Metamask already installed
if (window.ethereum) {
  provider = new ethers.providers.Web3Provider(window.ethereum);
  signer = provider.getSigner();
}

export default {
  name: 'HelloWorld',
  data() {
    return {
      userAddress: '',
      userSignature: '',
      e1: 1,
      pledge3: '',
      pledge4: ''
    }
  },
  computed: {
    notEthBrowser: () => !window.ethereum ? true : false,
		ethBrowserButNotConnected: function () { return this.userAddress === '' ? true : false}
  },
  methods: {
    connectToMetamask() {
      console.info("Hit [connect To Metamask]");
      // new way to ethereum.enable
      window.ethereum.send('eth_requestAccounts').then((res) => {
        this.userAddress = res.result[0];
      })
      provider = provider || new ethers.providers.Web3Provider(window.ethereum);
      signer = signer || provider.getSigner();
    },
    signPledge() {
      let typedData = {
        types: {
          EIP712Domain: [
            {name: "name", type: "string"},
            {name: "version", type: "string"},
            {name: "chainId", type: "uint256"},
            {name: "verifyingContract", type: "address"},
          ],
          Pledge: [
            {name: "action", type: "string"},
            {name: "pledge1", type: "string"},
            {name: "pledge2", type: "string"},
            {name: "pledge3", type: "string"},
            {name: "pledge4", type: "string"}
          ]
        },
        primaryType: 'Pledge',
        domain: {
          name: 'Strong Individuals',
          version: '1',
          chainId: 1,
          // random verify (verified) contract lol
          verifyingContract: '0x7abc37a70439cb4119f89acd953e5f54776b9381'
        },
        message: {
          'action': 'Signing Buidlers pledge',
          'pledge1': 'Positive-sum / #BUIDL',
          'pledge2': 'Better Coordination',
          'pledge3': this.pledge3,
          'pledge4': this.pledge4
        }
      };

      // Thanks ajb413 https://github.com/arcadeum/ethers-eip712/issues/1
      let signature = signer.provider.jsonRpcFetchFunc(
        'eth_signTypedData_v4',
        [ this.userAddress, JSON.stringify(typedData)]
      ).then(res => {
        console.log("From inside SIG: ", res)
        this.userSignature = res;
        return res;
      })
      console.log(">>> signature: ", signature)
    }
  }
}
</script>