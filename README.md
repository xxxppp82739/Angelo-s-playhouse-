<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Just a Simple Quiz :)</title>

<style>
    body {
        font-family: "Times New Roman", serif;
        background: white;
        color: black;
        text-align: center;
        padding: 50px;
        transition: 1s;
    }

    .dark {
        background: black;
        color: red;
    }

    #box {
        max-width: 600px;
        margin: auto;
    }

    input, button {
        padding: 10px;
        margin-top: 15px;
        font-family: inherit;
    }

    .glitch {
        animation: shake 0.1s infinite;
    }

    @keyframes shake {
        0% { transform: translate(2px, 2px); }
        50% { transform: translate(-2px, -2px); }
        100% { transform: translate(2px, -2px); }
    }

    #screen {
        display: none;
        font-size: 24px;
        color: red;
    }
</style>
</head>

<body>

<div id="box">
    <h1 id="question">What's your name?</h1>
    <input type="text" id="answer">
    <br>
    <button onclick="next()">Submit</button>
</div>

<div id="screen"></div>

<script>
let step = 0;
let name = "";
let age = "";
let color = "";
let aloneAnswer = "";

/* ---------- VIBRATION ---------- */
function vibrate(pattern) {
    if (navigator.vibrate) navigator.vibrate(pattern);
}

/* ---------- FAKE NAME DETECTION ---------- */
function looksFakeName(n) {
    const badPatterns = ["test", "asdf", "user", "admin", "123", "lol", "idk", "no"];

    if (!n) return true;

    const lower = n.toLowerCase();

    if (lower.length <= 2) return true;
    if (/^\d+$/.test(lower)) return true;

    return badPatterns.some(p => lower.includes(p));
}

/* ---------- DARK WORD DETECTION ---------- */
function isDarkColor(input) {
    const darkWords = ["black", "dark", "red", "blood", "void", "night", "shadow"];
    return darkWords.some(word => input.toLowerCase().includes(word));
}

/* ---------- MAIN GAME ---------- */
function next() {
    const input = document.getElementById("answer").value.trim();
    const q = document.getElementById("question");

    /* STEP 0: NAME */
    if (step === 0) {
        name = input || "Stranger";

        if (looksFakeName(name)) {
            q.innerText = "That doesn't feel like a real name.";
            document.body.classList.add("glitch");
            vibrate([100, 50, 100, 200]);

            setTimeout(() => {
                q.innerText = "But I’ll still call you that.";
            }, 1500);
        } 
        else {
            q.innerText = `Nice to meet you, ${name}. How old are you?`;
        }
    }

    /* STEP 1: AGE */
    else if (step === 1) {
        age = input || "unknown";

        if (parseInt(age) <= 10) {
            q.innerText = `${name}... you're very young. Why are you here?`;
            vibrate([100, 50, 100, 50, 300]);
        } 
        else if (parseInt(age) >= 18) {
            q.innerText = "An adult... you should know better.";
            vibrate(150);
        } 
        else {
            q.innerText = "Noted.";
        }
    }

    /* STEP 2: COLOR */
    else if (step === 2) {
        color = input;

        if (isDarkColor(color)) {
            q.innerText = `${color}? I like that.`;
            document.body.classList.add("dark");
            vibrate(200);
        } 
        else {
            q.innerText = `${color} feels... too bright.`;
        }
    }

    /* STEP 3: ALONE */
    else if (step === 3) {
        aloneAnswer = input.toLowerCase();

        if (aloneAnswer.includes("yes")) {
            q.innerText = "Good.";
            vibrate(300);
        } 
        else if (aloneAnswer.includes("no")) {
            q.innerText = "That wasn't expected.";
            vibrate([100, 100, 100, 300]);
        } 
        else {
            q.innerText = "I think you're lying.";
        }
    }

    /* STEP 4 */
    else if (step === 4) {
        if (parseInt(age) < 13) {
            q.innerText = `${name}... you shouldn't be here.`;
            vibrate([200, 100, 200]);
        } 
        else {
            q.innerText = "Are you alone now?";
        }
    }

    /* STEP 5: GLITCH */
    else if (step === 5) {
        q.innerText = "Look behind you.";
        document.body.classList.add("glitch");
        vibrate([200, 100, 200, 100, 400]);
    }

    /* STEP 6 */
    else if (step === 6) {
        document.body.classList.remove("glitch");
        q.innerText = "Just kidding.";
    }

    /* STEP 7 */
    else if (step === 7) {
        q.innerText = `${name}, I remember your answers.`;
        vibrate(300);
    }

    /* STEP 8 */
    else if (step === 8) {
        q.innerText = `Age: ${age}, Color: ${color}. I know you now.`;
        document.body.classList.add("dark");
    }

    /* STEP 9 */
    else if (step === 9) {
        q.innerText = "Do you still feel safe?";
        vibrate([50,50,50,50,500]);
    }

    /* STEP 10 */
    else if (step === 10) {
        q.innerText = "Why did you keep going?";
    }

    /* STEP 11: BLACKOUT ENDING */
    else if (step === 11) {
        document.getElementById("box").style.display = "none";
        document.getElementById("screen").style.display = "block";

        setTimeout(() => {
            document.getElementById("screen").innerText = "…";
            vibrate(200);
        }, 1000);

        setTimeout(() => {
            document.getElementById("screen").innerText = `${name}... I saw everything.`;
            vibrate([300, 100, 300]);
        }, 3000);

        setTimeout(() => {
            document.getElementById("screen").innerText = "Now I know you.";
        }, 5000);

        setTimeout(() => {
            document.getElementById("screen").innerText = "You should not have answered.";
            vibrate([200, 100, 200, 100, 600]);
        }, 7000);
    }

    step++;
    document.getElementById("answer").value = "";
}
</script>

</body>
</html># Angelo-s-playhouse-
