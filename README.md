pip install flask flask-socketio
from flask import Flask, render_template
from flask_socketio import SocketIO, send

app = Flask(__name__)
app.config['SECRET_KEY'] = 'segredo123'
socketio = SocketIO(app)

@app.route('/')
def index():
    return render_template('index.html')

@socketio.on('message')
def handle_message(msg):
    print('Mensagem recebida:', msg)
    send(msg, broadcast=True)

if __name__ == '__main__':
    socketio.run(app, host='0.0.0.0', port=5000)<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <title>Chat Simples</title>
    <script src="https://cdn.socket.io/4.5.4/socket.io.min.js"></script>
</head>
<body>
    <h2>💬 App de Mensagens</h2>

    <ul id="messages"></ul>

    <input id="messageInput" placeholder="Digite sua mensagem">
    <button onclick="sendMessage()">Enviar</button>

    <script>
        const socket = io();

        socket.on('message', function(msg) {
            const li = document.createElement("li");
            li.innerText = msg;
            document.getElementById("messages").appendChild(li);
        });

        function sendMessage() {
            const input = document.getElementById("messageInput");
            socket.send(input.value);
            input.value = '';
        }
    </script>
</body>
</html>python app.pyhttp://localhost:5000
