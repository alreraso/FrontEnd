<template>
<div>
    <nav-bar-wallet :linkProp="this.link"/>
    <br>

    <!--Select scroll need update -->
    <div class="container p-2">
    </div>  <!--end scroll -->  
    <div class="container p-2">
        <div class="row pt-6">
            <!--Here start firstcard -->
            <div class="col">
                <div class="card">
                    <div class="card-header text-dark bg-light text-center">                        
                        <h3>
                            {{ (this.name.slice(-1) === 's') ? this.name : this.name + "'s" }} Enterprise Wallet
                        </h3>                                                  
                    </div>
                    <div class="card-body text-center">
                        <span id="idwallet" data-clipboard-action="copy" data-clipboard-target="#idwallet" style="cursor: pointer;">
                            {{ this.wallet.wallets[0].Wal_id }}
                        </span>
                    </div>
                </div>
                <br>
                <div class="card">
                    <div class="card-header text-dark bg-light">                        
                        <h5>
                            <em class="fas fa-history"/> Transaction History
                        </h5>                                                  
                    </div>
                    <div class="card-body">
                        <p class="card-text">
                            Keep the transaction history of both you and the wallets associated with your enterprise wallet handy.
                        </p> 
                        <ul>
                            <li>
                                <p class="card-text">                  
                                    Show my history
                                    <span class="float-right">
                                        <a v-on:click="operationsEnterpriseWallet('/wallet-enterprise')" href="/operations" class="btn btn-dark btn-sm">Show</a>
                                    </span>
                                </p>
                            </li>
                        </ul>
                        <ul>
                            <li>
                                <p class="card-text">
                                    Show history of my associated wallets
                                    <span class="float-right">
                                        <a v-on:click="associatedWallets('history')" href="/associated-wallets" class="btn btn-dark btn-sm">Show</a>
                                    </span>
                                </p>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
            <!--second card second row -->
            <div class="col-md-5">                                        
                <div class="card animated pulse">
                    <div class="card">
                        <div class="card-header text-dark bg-light" >
                            <span class="float-left">
                                <h5 class="card-title"><em class="fas fa-balance-scale"></em> Total Balance</h5>
                            </span>
                        </div>
                        <div class="card-body">
                            <h5 class="card-title">
                                ${{ new Intl.NumberFormat("de-DE").format(this.wallet.wallets[0].Wal_balance) }}
                            </h5>
                        </div>
                    </div>                  
                </div>
                <br>               
                <div class="card animated pulse">
                    <div class="card">
                        <div class="card-header text-dark bg-light" >
                            <span class="float-left">
                                <h5 class="card-title"><em class="fas fa-tools"/> Wallet Options</h5>
                            </span>
                        </div>
                        <div class="card-body">
                            <ul>
                                <li>
                                    <p class="card-text">                  
                                        Manage associated wallets
                                        <span class="float-right">
                                            <a v-on:click="associatedWallets('manage')" href="/associated-wallets" class="btn btn-dark btn-sm">Manage</a>
                                        </span>
                                    </p>
                                </li>
                            </ul>
                            <ul>
                                <li>
                                    <p class="card-text">
                                        Create associated wallets
                                        <span class="float-right">
                                            <a v-on:click="signup(3, wallet.wallets[0].Ent_id, 'Create Associated Wallet')" href="/signup" class="btn btn-dark btn-sm">Create</a>
                                        </span>
                                    </p>
                                </li>
                            </ul>
                        </div>
                    </div>                        
                </div>
                <br>
                <div class="card">
                    <h5 class="card-header text-dark bg-light "><em class="fas fa-dollar-sign"></em> Make Transfer
                        <span class="float-right">
                            <a v-on:click="maketransferEnterprise('Make Transfer', '/wallet-enterprise')" href="/make-transfer" class="btn btn-dark btn-sm">Send</a>
                        </span>
                    </h5>
                        
                </div>                                      
                <br>                                         
            </div>                                
        </div>
        <br>
    </div>       
</div>
</template>
    
<script>
import axios from 'axios'
import NavBarWallet from '../components/NavBarWallet.vue'
import clipboard from 'clipboard'
new clipboard('#idwallet');

export default {
    name: 'walletEnterprise',
    components: {
        NavBarWallet
    },
    data() {
        return {
            name: localStorage.getItem('name'),
            wallet: null,
            response: null,
            link: '/wallet-enterprise'
        }
    }, beforeCreate() {
        const pathEnterpise = '/wallet/find/all/' + localStorage.getItem('username');
        axios
            .get(this.$store.state.backURL + pathEnterpise, {
                headers: {
                    'access-token': localStorage.getItem('token')
                }
            })
            .then(response => {
                if(response.status !== 200) {
                    alert("Request error");
                } else {
                    this.wallet = response.data;
                }
            }).catch(error => {
                if(error.status === 500) {
                    alert("Server error");
                } else {
                    alert("Backent conection impossible");
                }
            });
    },
    methods: {
        associatedWallets(item) {
            localStorage.removeItem('typePageAW')
            localStorage.setItem('typePageAW', item);
        },
        signup(item1, item2, item3) {
            localStorage.removeItem('wallettypeSignup');
            localStorage.removeItem('enterprise_idSignup');
            localStorage.removeItem('nameSignup');
            localStorage.setItem('wallettypeSignup', item1);
            localStorage.setItem('enterprise_idSignup', item2);
            localStorage.setItem('nameSignup', item3);
        },
        operationsEnterpriseWallet(item) {
            localStorage.removeItem('usernameOperations');
            localStorage.removeItem('linkOperations');
            localStorage.setItem('linkOperations', item);
        },
        maketransferEnterprise(item1, item2) {
            localStorage.removeItem('namePageMakeTransfer');
            localStorage.removeItem('destWalletMakeTransfer');
            localStorage.removeItem('linkMakeTransfer');
            localStorage.setItem('namePageMakeTransfer', item1);
            localStorage.setItem('linkMakeTransfer', item2);
        }
    }
}
</script>

