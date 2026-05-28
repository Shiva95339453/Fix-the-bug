// Universal Login Bug Fix Example
// Works for Android, iPhone, Tablet, Desktop Web

async function loginUser(email, password) {
    try {
        // Check internet connection
        if (!navigator.onLine) {
            alert("No internet connection");
            return;
        }

        // Validate input
        if (!email || !password) {
            alert("Email and password required");
            return;
        }

        // API request
        const response = await fetch("https://your-api.com/login", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                email: email.trim(),
                password: password
            })
        });

        const data = await response.json();

        // Handle login success
        if (response.ok && data.token) {
            localStorage.setItem("token", data.token);

            // Device-safe redirect
            window.location.href = "/dashboard";

        } else {
            alert(data.message || "Login failed");
        }

    } catch (error) {
        console.error("Login Error:", error);

        // Common device compatibility fix
        alert("Server error. Please try again.");
    }
}

// Button event
document.getElementById("loginBtn").addEventListener("click", () => {
    const email = document.getElementById("email").value;
    const password = document.getElementById("password").value;

    loginUser(email, password);
});
