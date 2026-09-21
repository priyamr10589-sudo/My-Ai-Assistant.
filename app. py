from flask import Flask, request
import requests

app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def home():
    answer = ""

    if request.method == "POST":
        question = request.form["question"].strip()

        # Search Wikipedia
        search_url = "https://en.wikipedia.org/w/api.php"

        params = {
            "action": "query",
            "list": "search",
            "srsearch": question,
            "format": "json"
        }

        headers = {
            "User-Agent": "MyWikipediaBot/1.0"
        }

        response = requests.get(
            search_url,
            params=params,
            headers=headers
        )

        if response.status_code == 200:
            data = response.json()
            results = data["query"]["search"]

            if results:
                title = results[0]["title"]

                # Get the summary
                summary_url = (
                    "https://en.wikipedia.org/api/rest_v1/page/summary/"
                    + requests.utils.quote(title)
                )

                summary_response = requests.get(
                    summary_url,
                    headers=headers
                )

                if summary_response.status_code == 200:
                    summary_data = summary_response.json()
                    answer = summary_data.get(
                        "extract",
                        "No answer found."
                    )
                else:
                    answer = "I found the topic, but couldn't get its summary."
            else:
                answer = "I couldn't find anything on Wikipedia."

    return """
   <html>
<head>
<div class="particles">
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    <span></span>
</div>
    <title>Elite AI</title>

    <style>
    .particles {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: -1;
}

.particles span {
    position: absolute;
    width: 5px;
    height: 5px;
    background: white;
    border-radius: 50%;
    box-shadow: 0 0 12px white;
    animation: float 8s infinite ease-in-out;
}

/* Different particle positions */
.particles span:nth-child(1) { left: 10%; top: 20%; }
.particles span:nth-child(2) { left: 25%; top: 70%; animation-delay: 1s; }
.particles span:nth-child(3) { left: 40%; top: 30%; animation-delay: 2s; }
.particles span:nth-child(4) { left: 55%; top: 80%; animation-delay: 3s; }
.particles span:nth-child(5) { left: 70%; top: 25%; animation-delay: 1.5s; }
.particles span:nth-child(6) { left: 85%; top: 60%; animation-delay: 2.5s; }
.particles span:nth-child(7) { left: 15%; top: 90%; animation-delay: 4s; }
.particles span:nth-child(8) { left: 60%; top: 10%; animation-delay: 3.5s; }
.particles span:nth-child(9) { left: 90%; top: 35%; animation-delay: 5s; }
.particles span:nth-child(10) { left: 35%; top: 50%; animation-delay: 4.5s; }

@keyframes float {
    0%, 100% {
        transform: translateY(0) scale(1);
        opacity: 0.3;
    }

    50% {
        transform: translateY(-40px) scale(1.5);
        opacity: 1;
    }
}
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #0f172a;
            color: white;
        }

        .header {
            text-align: center;
            padding: 25px;
            background: #111827;
            box-shadow: 0 2px 10px rgba(0,0,0,0.4);
        }

        .header h1 {
            margin: 0;
            font-size: 30px;
        }

        .header p {
            color: #94a3b8;
        }

        .chat {
            max-width: 750px;
            margin: 30px auto;
            padding: 20px;
        }

        .user {
            background: #2563eb;
            padding: 14px 18px;
            border-radius: 18px 18px 4px 18px;
            margin: 15px 0;
            margin-left: 20%;
            text-align: right;
        }

        .bot {
            background: #1e293b;
            padding: 18px;
            border-radius: 18px 18px 18px 4px;
            margin: 15px 0;
            margin-right: 10%;
            line-height: 1.6;
        }

        form {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            padding: 15px;
            background: #111827;
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        input {
            width: 60%;
            padding: 14px;
            border: none;
            border-radius: 25px;
            font-size: 16px;
            outline: none;
        }

        button {
            padding: 14px 22px;
            border: none;
            border-radius: 25px;
            background: #2563eb;
            color: white;
            font-size: 16px;
            cursor: pointer;
        }

        button:hover {
            background: #1d4ed8;
        }
        /* Animated background */
body {
    margin: 0;
    font-family: Arial, sans-serif;
    color: white;
    overflow-x: hidden;

    background:
        radial-gradient(circle at 20% 20%, rgba(0, 200, 255, 0.15), transparent 30%),
        radial-gradient(circle at 80% 70%, rgba(120, 0, 255, 0.15), transparent 30%),
        #0f172a;

    background-size: 200% 200%;
    animation: backgroundMove 10s ease-in-out infinite alternate;
}

@keyframes backgroundMove {
    0% {
        background-position: 0% 0%;
    }

    100% {
        background-position: 100% 100%;
    }
}
    </style>
</head>

<body>

    <div class="header">
        <h1>🤖 Elite AI</h1>
        <p>Powered by Wikipedia 🔎</p>
    </div>

    <div class="chat">

        <div class="bot">
            👋 Hello! Ask me something.
        </div>

        """ + ("""
        <div class="user">
            """ + question + """
        </div>

        <div class="bot">
            """ + answer + """
        </div>
        """ if answer else "") + """

    </div>

    <form method="POST">
        <input name="question"
               placeholder="Ask me anything..."
               required>

        <button type="submit">Send 🚀</button>
    </form>

</body>
</html>
    """

app.run(host="0.0.0.0", port=8080)

