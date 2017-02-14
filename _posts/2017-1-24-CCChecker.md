---
title:  "Credit Card Checker"
date:   2017-01-24 9:00:00
description: Credit Card Checker for 4TB3 using regular expressions
---

<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>

Credit Card Number Checker
##########################


<form id="cc">
<input type="number" name="ccnumber" pattern="">
</form>

<script>
    function isValidCreditCard(sText){
        var res = sText.match(/^[4][0-9]{15}$|^[4][0-9]{12}$|^[5][1-5][0-9]{14}|^[3][(4|7)][0-9]{13}/);
        if(Number(res[0]) == Number(sText)){
            //continue
            var count = 0;
            for(var i = 0; i < sText.length; i++ ){
                console.log('current Value',Number(sText.charAt(i)));
                var currentVal = Number(sText.charAt(i));
                if(i%2 == 0){
                    currentVal = Number(sText.charAt(i))*2;
                    console.log('Currently a check digit', currentVal);
                    if(currentVal > 9){
                        currentVal -= 9;
                    }
                }
                count+=currentVal;
            }
            if(count%10 == 0){
                return true;
            }
        }
        return false;
    }
</script>