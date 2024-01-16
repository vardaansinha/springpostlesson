---
comments: true
layout: base
title: Account
permalink: account
---

{% include header.html %}

<html>
    <style>
        .logindiv {
            position: fixed;
            top: 20%;
            left: 15%;
            width: 30%;
            height: 30%;
            border: 2px solid #3E3C3D;
            outline: #6A6A6A solid 10px;
            background-color: #f0f0f0;
            box-sizing: border-box;
            padding: 10px;
        }
        .signupdiv {
            position: fixed;
            top: 20%;
            right: 15%;
            width: 30%;
            height: 30%;
            border: 2px solid #3E3C3D;
            outline: #6A6A6A solid 10px;
            background-color: #f0f0f0;
            box-sizing: border-box;
            padding: 10px;
        }
        .accountdiv {
            position: fixed;
            bottom: 10%;
            left: 30%;
            width: 40%;
            height: 30%;
            border: 2px solid #3E3C3D;
            outline: #6A6A6A solid 10px;
            background-color: #f0f0f0;
            box-sizing: border-box;
            padding: 10px;
        }
        h {
            font-size: 40px;
        }
    </style>
<body>
    <div class="logindiv">
        <div>
            <h>Login</h>
            <br>
            <br>
            <input id="loginusername" placeholder="Username">
            <br>
            <input id="loginpassword" placeholder="Password">
            <br>
            <button onclick="loginenter()">Login</button>
        </div>
        <div class="signupdiv">
            <h>Sign Up</h>
            <br>
            <br>
            <input id="signupemail" placeholder="Email">
            <br>
            <input id="signupusername" placeholder="Username">
            <br>
            <input id="signuppassword" placeholder="Password">
            <br>
            <button onclick="signupenter()">Sign Up</button>
        </div>
    </div>
    <div class="accountdiv">
        <h>My Account</h>
        <br>
        <br>
        Username: 
        <text id="accountusername"></text>
            <br>
            <input id="changeusername" placeholder="Change Username">
            <br>
            <button onclick="changeusernameenter()">Change Username</button>
        <br>
        <br>
        Email: 
        <text id="accountemail"></text>
            <br>
            <input id="changeemail" placeholder="Change Email">
            <br>
            <button onclick="changeemailenter()">Change Email</button>
    </div>
</body>
</html>

<script>

    function loginenter(){
        var Lusername = document.getElementById("loginusername").value;
        if (Lusername == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("loginusername").value = ""
        console.log("Login Username: " + Lusername)

        var Lpassword = document.getElementById("loginpassword").value;
        if (Lpassword == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("loginpassword").value = ""
        console.log("Login Password: " + Lpassword)
    }
    function signupenter(){
        var Semail = document.getElementById("signupemail").value;
        if (Semail == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("signupemail").value = ""
        console.log("Sign Up Email: " + Semail)

        var Susername = document.getElementById("signupusername").value;
        if (Susername == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("signupusername").value = ""
        console.log("Sign Up Username: " + Susername)

        var Spassword = document.getElementById("signuppassword").value;
        if (Spassword == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("signuppassword").value = ""
        console.log("Sign Up Password: " + Spassword)
    }
    function changeusernameenter(){
        var newusername = document.getElementById("changeusername").value;
        if (newusername == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("changeusername").value = ""
        console.log("New Username: " + newusername)
    }
    function changeemailenter(){
        var newemail = document.getElementById("changeemail").value;
        if (newemail == ""){
            alert("Try Again.")
            return;
        }
        document.getElementById("changeemail").value = ""
        console.log("New Email: " + newemail)
    }
</script>