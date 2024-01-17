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
<script>
    function loginenter(){
        var Lusername = document.getElementById("loginusername").value;
        if (Lusername == ""){
            alert("Try Again.");
            return;
        }
        document.getElementById("loginusername").value = "";
        console.log("Login Username: " + Lusername);
        var Lpassword = document.getElementById("loginpassword").value;
        if (Lpassword == ""){
            alert("Try Again.");
            return;
        }
        document.getElementById("loginpassword").value = "";
        console.log("Login Password: " + Lpassword);
    }
    function signupenter(){
        var Semail = document.getElementById("signupemail").value;
        if (Semail == ""){
            alert("Try Again.");
            return;
        }
        document.getElementById("signupemail").value = "";
        console.log("Sign Up Email: " + Semail);
        var Susername = document.getElementById("signupusername").value;
        if (Susername == ""){
            alert("Try Again.");
            return;
        }
        document.getElementById("signupusername").value = "";
        console.log("Sign Up Username: " + Susername);
        var Spassword = document.getElementById("signuppassword").value;
        if (Spassword == ""){
            alert("Try Again.");
            return;
        }
        document.getElementById("signuppassword").value = "";
        console.log("Sign Up Password: " + Spassword);
    }
    function changepasswordenter(){
        var newpassword = document.getElementById("changepassword").value;
        if (newpassword == ""){
            alert("Try Again.");
            return;
        }
        document.getElementById("changepassword").value = "";
        console.log("New password: " + newpassword);
    }
    function deleteaccount() {
        var userId = document.getElementById("userIdToDelete").value;
        if (!userId || isNaN(userId)) {
            alert("Please enter a valid user ID.");
            return;
        }
        var confirmDelete = confirm("Are you sure you want to delete this account?");
        if (confirmDelete) {
            console.log("Deleting user with ID: " + userId);
        } else {
            console.log("Account deletion canceled.");
        }
    }
</script>

<body>
    <div class="logindiv">
        <div>
            <h>Login</h>
            <br>
            <br>
            <input id="loginusername" placeholder="Email">
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
        Password Change: 
        <text id="accountusername"></text>
            <br>
            <input id="changepassword" placeholder="Enter New Password">
            <br>
            <input id="userIdToUpdate" placeholder="Enter ID">
            <br>
            <button onclick="changepasswordenter()">Change Password</button>
        <br>
        <br>
        Account Delete: 
        <text id="accountemail"></text>
            <br>
            <input id="userIdToDelete" placeholder="Enter ID">
            <br>
            <button onclick="deleteaccount()">Delete Account</button>
        <br>
    </div>
</body>


<!-- <script>

    
    const login_url = "http://localhost:8085/authenticate";
    const read_url = "http://localhost:8085/mvc/person/read";
    const signup_url = "http://localhost:8085/api/person/create";
    const post_url = "http://localhost:8085/api/person/post";
    const update_url = "http://localhost:8085/api/person/update";
    const delete_url = "http://localhost:8085/api/person/delete";

    function loginenter() {
        var Lusername = document.getElementById("loginusername").value;
        var Lpassword = document.getElementById("loginpassword").value;

        if (Lusername == "" || Lpassword == "") {
            alert("Try Again.");
            return;
        }

        const authBody = {
            username: Lusername,
            password: Lpassword
        };

    const authOptions = {
        method: 'POST',
        mode: 'cors',
        cache: 'no-cache',
        credentials: 'include',
        body: JSON.stringify(authBody),
        headers: {
            'Content-Type': 'application/json',
        },
        };

        // Fetch JWT
        fetch(login_url, authOptions)
            .then(response => response.json())
            .then(data => {
                if (data.token) {
                    // Save the token securely, you may use localStorage or sessionStorage
                    localStorage.setItem('jwtToken', data.token);
                    alert('Login successful!');
                    // Redirect or perform other actions as needed
                    read_pull();
                } else {
                    alert('Login failed. Please try again.');
                }
            })
            .catch(error => console.error('Error:', error));
    }

    function read_pull() {
        const jwtToken = localStorage.getItem('jwtToken');

        if (!jwtToken) {
            alert('JWT token not found. Please login first.');
            return;
        }

    const options = {
        method: 'GET',
        mode: 'cors',
        cache: 'default',
        credentials: 'include',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer ' + jwtToken,
            'Origin': allowedOrigin,  // Add this line
        },
    };

        fetch(read_url, options)
            .then(response => {
                if (response.ok) {
                    console.log(response);
                    alert("Signed In!")
                    return;
                }
                if (response.status !== 200) {
                    const errorMsg = 'Database response error: ' + response.status;
                    console.log(errorMsg);
                    return;
                }
            })
            .catch(error => console.error('Error:', error));
    }

    function changepasswordenter() {
        const newpassword = document.getElementById("changepassword").value;

        if (newpassword == "") {
            alert("Try Again.");
            return;
        }

        const userIdToUpdate = document.getElementById("userIdToUpdate").value;

        if (!userIdToUpdate || isNaN(userIdToUpdate)) {
            alert("Please enter a valid user ID.");
            return;
        }

        const jwtToken = localStorage.getItem('jwtToken');

        if (!jwtToken) {
            alert('JWT token not found. Please login first.');
            return;
        }

    const updateOptions = {
        method: 'PUT',
        mode: 'cors',
        cache: 'no-cache',
        credentials: 'include',
        body: JSON.stringify({ userId: userIdToUpdate, newPassword: newpassword }),
        headers: {
            'Content-Type': 'application/json',
        },
    };

        fetch(update_url, updateOptions)
            .then(response => {
                if (response.ok) {
                    console.log('Password updated successfully');
                    // You can perform additional actions or redirect as needed
                } else {
                    console.error('Error updating password:', response.status);
                    // Handle error, display a message, etc.
                }
            })
            .catch(error => console.error('Error:', error));
    }

    function deleteaccount() {
        const userIdToDelete = document.getElementById("userIdToDelete").value;

        if (!userIdToDelete || isNaN(userIdToDelete)) {
            alert("Please enter a valid user ID.");
            return;
        }

        const jwtToken = localStorage.getItem('jwtToken');

        if (!jwtToken) {
            alert('JWT token not found. Please login first.');
            return;
        }
    const deleteOptions = {
        method: 'DELETE',
        mode: 'cors',
        cache: 'no-cache',
        credentials: 'include',
        headers: {
            'Authorization': 'Bearer ' + jwtToken,
        },
    };

        fetch(delete_url + `/${userIdToDelete}`, deleteOptions)
            .then(response => {
                if (response.ok) {
                    console.log('Account deleted successfully');
                    // You can perform additional actions or redirect as needed
                } else {
                    console.error('Error deleting account:', response.status);
                    // Handle error, display a message, etc.
                }
            })
            .catch(error => console.error('Error:', error));
    }
    function signupenter() {
        const Semail = document.getElementById("signupemail").value;
        const Susername = document.getElementById("signupusername").value;
        const Spassword = document.getElementById("signuppassword").value;

        if (Semail == "" || Susername == "" || Spassword == "") {
            alert("Try Again.");
            return;
        }

        const signUpBody = {
            email: Semail,
            username: Susername,
            password: Spassword
        };

    const signUpOptions = {
        method: 'POST',
        mode: 'cors',
        cache: 'no-cache',
        credentials: 'include',
        body: JSON.stringify(signUpBody),
        headers: {
            'Content-Type': 'application/json',
        },
    };

        fetch(signup_url, signUpOptions)
            .then(response => {
                if (response.ok) {
                    console.log('User registered successfully');
                    // You can perform additional actions or redirect as needed
                } else {
                    console.error('Error registering user:', response.status);
                    // Handle error, display a message, etc.
                }
            })
            .catch(error => console.error('Error:', error));
    }
</script> -->