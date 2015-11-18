---
title:  "Vignere Cipher"
date:   2014-12-10 9:00:00
description: Vignere Cipher
---
<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
While studying for exams on computer security, I decided to just write up a Javascript caesar cipher and a Vigenère cipher given the key below. A Vigenère cipher works similar to caesar cipher with shifting letters by an off set. A Vigenere cipher works by transcribing a key repeatedly under the plaintext, and according to the letter of the corresponding key the shift is determined. And the matrix below can accompany new characters by generating a new matrix, the matrix is read according to the first column A and given the corresponding key character on the top row, will give the according letter in return.

<div id="charset" style="width: 50%; margin: 0 auto;"><input id="charsetinput" style="width: 80%;" type="text" value="charset"> <button id="charsetupdate">Update</button></div>
<div id="venmatrix" style="overflow-x:scroll"></div>
<div id="textencipher" style="width: 80%; margin: 0 auto;"><input id="key" type="text" value="key"> <button id="encipherbutton">Encipher</button> <input id="plaintext" type="text" value="plaintext"> <button id="decipherbutton">Decipher</button> <input id="cipheredtext" type="text" value="cipheredtext"></div>

#### The code used to create the cipher
```javascript
var alphabets = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
var alpha = {};

for(var i = 0; i < alphabets.length; i++){
	alpha[alphabets[i]] = i; //assigns each alphabet a integer key
}

function newAlpha(inputChar){
	alphabets.push(inputChar);
	mapfunc();
	genVernier("venmatrix");
}

function alphaset(inputString){
	alphabets = inputString.split("");
	mapfunc();
	genVernier("venmatrix");
}

function returnsalphaset(){
	return 	alphabets.join("");
}

function mapfunc(){ //creates a mapping of the alphabets
	for(var i = 0; i < alphabets.length; i++){
		alpha[alphabets[i]] = i; //assigns each alphabet a integer key
	}
}

Number.prototype.mod = function(n) { return ((this%n)+n)%n; }

function verniercipher(inputString, keyString, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var keyArray = keyString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{
		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		if((counter + 1) >= keyArray.length)
		{
			counter = 0; //Resets the keyArray Counter so it continues from zero again
		}
		else
		{
			counter ++;
		}
	}
	return outputArray.join("");
}

function ceasercipher(inputString, shift, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{

		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + shift).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - shift).mod(alphabets.length-1)];		
		}
	}
	return outputArray.join("");
}

function Encipher(){
	document.getElementById("cipheredtext").value = verniercipher(document.getElementById("plaintext").value , document.getElementById("key").value, true);
}

function Decipher(){
	document.getElementById("plaintext").value = verniercipher(document.getElementById("cipheredtext").value , document.getElementById("key").value, false);
}

function genVernier(name) {
    var myTableDiv = document.getElementById(name);
    if(document.getElementById("genVernier")){
    	$('#genVernier').remove();
    }

    var table = document.createElement('TABLE');
    table.setAttribute("id", "genVernier");
    var tableBody = document.createElement('TBODY');

    table.border = '0'
    table.appendChild(tableBody);


    //TABLE COLUMNS
    var tr = document.createElement('TR');

    for (i = 0; i <= alphabets.length; i++) {
        var th = document.createElement('TH')
        th.width = '10';
        if(i == 0){
        	th.appendChild(document.createTextNode(" "));
        	tr.appendChild(th);
        }
        else{
	        th.appendChild(document.createTextNode(alphabets[i-1]));
	        tr.appendChild(th);
        }
    }
    tableBody.appendChild(tr);

    //TABLE ROWS
    for (i = 0; i < alphabets.length; i++) {
        var tr = document.createElement('TR');

        for (j = 0; j <= alphabets.length; j++) {
            var td = document.createElement('TD')
            if(j == 0){
        		td.appendChild(document.createTextNode(alphabets[i]));
        		tr.appendChild(td);
	        }
	        else{
            	td.appendChild(document.createTextNode(alphabets[(alpha[alphabets[i]] + alpha[alphabets[j-1]]).mod(alphabets.length)]));
            	tr.appendChild(td)
            }
        }
        tableBody.appendChild(tr);
    }  
    myTableDiv.appendChild(table)
}

$(document).ready(function() {
    	genVernier("venmatrix");
    	document.getElementById("charsetinput").value = returnsalphaset();
    	$("#charsetupdate").click(function(){
			alphabets = document.getElementById("charsetinput").value.split("");
			mapfunc();
			genVernier("venmatrix");
		});
		$("#encipherbutton").click(function(){
			Encipher();
		});
		$("#decipherbutton").click(function(){
			Decipher();
		});
});
```

<script type="text/javascript">
var alphabets = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
var alpha = {};

for(var i = 0; i < alphabets.length; i++){
	alpha[alphabets[i]] = i; //assigns each alphabet a integer key
}

function newAlpha(inputChar){
	alphabets.push(inputChar);
	mapfunc();
	genVernier("venmatrix");
}

function alphaset(inputString){
	alphabets = inputString.split("");
	mapfunc();
	genVernier("venmatrix");
}

function returnsalphaset(){
	return 	alphabets.join("");
}

function mapfunc(){ //creates a mapping of the alphabets
	for(var i = 0; i < alphabets.length; i++){
		alpha[alphabets[i]] = i; //assigns each alphabet a integer key
	}
}

Number.prototype.mod = function(n) { return ((this%n)+n)%n; }

function verniercipher(inputString, keyString, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var keyArray = keyString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{
		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		if((counter + 1) >= keyArray.length)
		{
			counter = 0; //Resets the keyArray Counter so it continues from zero again
		}
		else
		{
			counter ++;
		}
	}
	return outputArray.join("");
}

function ceasercipher(inputString, shift, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{

		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + shift).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - shift).mod(alphabets.length-1)];		
		}
	}
	return outputArray.join("");
}

function Encipher(){
	document.getElementById("cipheredtext").value = verniercipher(document.getElementById("plaintext").value , document.getElementById("key").value, true);
}

function Decipher(){
	document.getElementById("plaintext").value = verniercipher(document.getElementById("cipheredtext").value , document.getElementById("key").value, false);
}

function genVernier(name) {
    var myTableDiv = document.getElementById(name);
    if(document.getElementById("genVernier")){
    	$('#genVernier').remove();
    }

    var table = document.createElement('TABLE');
    table.setAttribute("id", "genVernier");
    var tableBody = document.createElement('TBODY');

    table.border = '0'
    table.appendChild(tableBody);


    //TABLE COLUMNS
    var tr = document.createElement('TR');

    for (i = 0; i <= alphabets.length; i++) {
        var th = document.createElement('TH')
        th.width = '10';
        if(i == 0){
        	th.appendChild(document.createTextNode(" "));
        	tr.appendChild(th);
        }
        else{
	        th.appendChild(document.createTextNode(alphabets[i-1]));
	        tr.appendChild(th);
        }
    }
    tableBody.appendChild(tr);

    //TABLE ROWS
    for (i = 0; i < alphabets.length; i++) {
        var tr = document.createElement('TR');

        for (j = 0; j <= alphabets.length; j++) {
            var td = document.createElement('TD')
            if(j == 0){
        		td.appendChild(document.createTextNode(alphabets[i]));
        		tr.appendChild(td);
	        }
	        else{
            	td.appendChild(document.createTextNode(alphabets[(alpha[alphabets[i]] + alpha[alphabets[j-1]]).mod(alphabets.length)]));
            	tr.appendChild(td)
            }
        }
        tableBody.appendChild(tr);
    }  
    myTableDiv.appendChild(table)
}

$(document).ready(function() {
    	genVernier("venmatrix");
    	document.getElementById("charsetinput").value = returnsalphaset();
    	$("#charsetupdate").click(function(){
			alphabets = document.getElementById("charsetinput").value.split("");
			mapfunc();
			genVernier("venmatrix");
		});
		$("#encipherbutton").click(function(){
			Encipher();
		});
		$("#decipherbutton").click(function(){
			Decipher();
		});
});
</script>
