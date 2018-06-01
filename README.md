# Assem - hodler function preview

[Assem.app](http://assem.app)

Sorry for full-sourcecode now,

We will open after 30% of ICO is filled
## Hodler function

```sh
      function submit_hodler() public{
        bool already = false;
        for(uint i=0;i<hodlerLength;i++){
            if(hodler[i] == msg.sender){
                already = true;
                break;
            }
        }
        if(!already){
            uint tmp_token_balance = tokenContract.balanceOf(msg.sender);
            if(tmp_token_balance >= 100000000000000000000){
                hodler.push(msg.sender);
                hodlerLength++;
            }
        }
    }
    
    function add_hodler(address hodl) public onlyBy (owner){
        hodler.push(hodl);
    }
    
    function snapShot() public onlyBy (owner){
        uint thisMonthProfit = mm_profit+krl_profit+jpl_profit+omk_profit;
        uint now_full_token = 0;
        uint hodl_Profit = thisMonthProfit*9/10;
        for(uint i=0;i<hodlerLength;i++){
            uint tmp_token_balance = tokenContract.balanceOf(hodler[i]);
            if(tmp_token_balance >= 100000000000000000000){
                now_full_token += tokenContract.balanceOf(hodler[i]);
            }
        }
        uint spreaded_Profit = hodl_Profit;
        uint this_hodler_Profit = 0;
        for(uint j=0;j<hodlerLength;j++){
            tmp_token_balance = tokenContract.balanceOf(hodler[j]);
            if(tmp_token_balance >= 100000000000000000000){
                this_hodler_Profit = hodl_Profit*percent(tmp_token_balance,now_full_token,3)/1000;
                if((spreaded_Profit + this_hodler_Profit) <= hodl_Profit){
                    lastSanpEther[hodler[j]] += this_hodler_Profit;
                    spreaded_Profit += this_hodler_Profit;
                }
                else{
                    lastSanpEther[hodler[j]] += (hodl_Profit - spreaded_Profit);
                    spreaded_Profit += this_hodler_Profit;
                }
            }
        }
        ownerBalance += (thisMonthProfit - hodl_Profit);
        mm_profit = 0;
        krl_profit = 0;
        jpl_profit = 0;
        omk_profit = 0;
    }
    
    function hodler_withdraw() public returns(bool){
        if(lastSanpEther[msg.sender] > 0){
            msg.sender.transfer(lastSanpEther[msg.sender]);
            lastSanpEther[msg.sender] = 0;
            return true;
        }
        else{
            return false;
        }
    }
    
    function owner_withdraw() public onlyBy (owner){
        owner.transfer(ownerBalance);
        ownerBalance = 0;
    }
```

## User Guide

* [Generate HTML](#generate-html)
* [Adding Custom Environment Variables](#adding-custom-environment-variables)
  * [Referencing Environment Variables in the JavaScript](#referencing-environment-variables-in-the-javascript)
  * [Referencing Environment Variables in the HTML](#referencing-environment-variables-in-the-html)
  * [Adding Temporary Environment Variables In Your Shell](#adding-temporary-environment-variables-in-your-shell)
  * [Adding Development Environment Variables via `.hero-cli.json`](#adding-development-environment-variables-via-hero-clijson)
* [Proxying API Requests in Development](#proxying-api-requests-in-development)
* [Build Native App](#build-native-app)
  * [Android](#android)
  * [iOS](#ios)
* [Build Scripts](#build-scripts)
  * [`hero start`](#hero-start)
  * [`hero build`](#hero-build)
  * [`hero serve`](#hero-serve)
  * [`hero init`](#hero-init)
  * [`hero platform build android`](`#hero-platform-build-android)
