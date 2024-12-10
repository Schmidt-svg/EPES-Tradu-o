<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EPES TALK LIFE</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css"></link>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
        }
    </style>
</head>
<body class="bg-gray-900 text-white">
    <div class="container mx-auto p-4">
        <header class="text-center mb-8">
            <h1 class="text-4xl font-bold text-neon-green">EPES TALK LIFE</h1>
            <p class="text-lg">Tradução ao vivo em Português e Inglês</p>
        </header>
        <main class="flex flex-col items-center">
            <div class="mb-4">
                <button id="startButton" class="bg-neon-green text-gray-900 px-4 py-2 rounded-full text-lg font-bold hover:bg-green-500 transition duration-300">
                    <i class="fas fa-microphone"></i> Iniciar Tradução
                </button>
                <button id="stopButton" class="bg-red-600 text-white px-4 py-2 rounded-full text-lg font-bold hover:bg-red-700 transition duration-300 ml-4">
                    <i class="fas fa-stop"></i> Parar Tradução
                </button>
            </div>
            <div class="w-full max-w-2xl">
                <div class="bg-gray-800 p-4 rounded-lg shadow-lg mb-4">
                    <h2 class="text-2xl font-bold mb-2">Texto Original</h2>
                    <p id="originalText" class="text-lg">Pressione "Iniciar Tradução" para começar...</p>
                </div>
                <div class="bg-gray-800 p-4 rounded-lg shadow-lg mb-4">
                    <h2 class="text-2xl font-bold mb-2">Tradução</h2>
                    <p id="translatedText" class="text-lg">A tradução aparecerá aqui...</p>
                </div>
            </div>
        </main>
    </div>

    <script>
        const startButton = document.getElementById('startButton');
        const stopButton = document.getElementById('stopButton');
        const originalText = document.getElementById('originalText');
        const translatedText = document.getElementById('translatedText');

        let recognition;
        let isTranslating = false;

        if ('webkitSpeechRecognition' in window) {
            recognition = new webkitSpeechRecognition();
            recognition.continuous = true;
            recognition.interimResults = true;
            recognition.lang = 'pt-BR';

            recognition.onresult = (event) => {
                let interimTranscript = '';
                let finalTranscript = '';

                for (let i = event.resultIndex; i < event.results.length; ++i) {
                    if (event.results[i].isFinal) {
                        finalTranscript += event.results[i][0].transcript;
                    } else {
                        interimTranscript += event.results[i][0].transcript;
                    }
                }

                originalText.innerHTML = finalTranscript + '<i>' + interimTranscript + '</i>';
                translateText(finalTranscript);
            };
        } else {
            alert('Seu navegador não suporta reconhecimento de voz.');
        }

        startButton.addEventListener('click', () => {
            if (!isTranslating) {
                recognition.start();
                isTranslating = true;
                originalText.innerHTML = 'Escutando...';
                translatedText.innerHTML = 'A tradução aparecerá aqui...';
            }
        });

        stopButton.addEventListener('click', () => {
            if (isTranslating) {
                recognition.stop();
                isTranslating = false;
                originalText.innerHTML = 'Tradução parada.';
                translatedText.innerHTML = 'A tradução foi interrompida.';
            }
        });

        function translateText(text) {
            const apiUrl = href=https=apimymemorytranslatednet,constget=$={text}&&langpair;pt|en;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    translatedText.innerHTML = data.responseData.translatedText;
                })
                .catch(error => {
                    translatedText.innerHTML = 'Erro na tradução.';
                    console.error('Erro:', error);
                });
        }
    </script>
</body>
</html>
