<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Treino de Fogo - Tracker TAF/Academia</title>
    <!-- Carrega Tailwind CSS para estilização e responsividade -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Carrega a fonte Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7fafc; /* Cor de fundo clara */
        }
        .scrollable-content {
            /* Fixa o cabeçalho e permite que o conteúdo role */
            max-height: calc(100vh - 4rem); /* Ajuste conforme a altura do header */
            overflow-y: auto;
        }
        .timer-button {
            transition: all 0.2s ease-in-out;
        }
        .set-circle {
            /* Fixa o tamanho do círculo para melhor toque em dispositivos móveis */
            width: 40px; 
            height: 40px;
        }
    </style>
</head>
<body>

<!-- Importações do Firebase -->
<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, doc, getDoc, addDoc, collection, query, where, getDocs, limit, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
    import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    // Define o nível de log para debug no console
    setLogLevel('Debug');

    // Mapeia as variáveis globais do ambiente (MANDATÓRIO)
    const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
    const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
    const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

    // Variáveis globais para Firebase e estado da aplicação
    let app, db, auth;
    let currentUserId = null;
    let lastPerformanceCache = {}; // Cache para o último desempenho de cada exercício

    // Estado do Treino
    let currentDayIndex = new Date().getDay(); // 0=Dom, 1=Seg, ..., 6=Sáb (Default: Dia de hoje)
    let currentWorkout;
    const dayLabels = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
    
    // Estado da Conclusão de Séries da Sessão Atual
    let currentSetStatus = []; // Array de arrays: [[false, false, false, false], ...]

    // Estrutura de Treino (Derivada do Markdown original)
    const workoutPlan = {
        0: { name: 'Treino A: Peito Puxado + TAF (Dom)', exercises: [
            { group: 'PEITO', name: 'Supino Inclinado com Halteres', sets: 4, taf: 'TAF Barra Fixa (3x Máx. Reps)' },
            { group: 'PEITO', name: 'Crossover na Polia Alta', sets: 4, taf: '' },
            { group: 'PEITO', name: 'Peck Deck / Voador', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Tríceps Corda na Polia Alta', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Tríceps Francês Halteres (Sentado)', sets: 4, taf: '' },
            { group: 'ABDÔMEN', name: 'Abdominal Máquina (Crunch)', sets: 4, taf: '' }
        ]},
        1: { name: 'Treino B: Costas Pesado + Bíceps (Seg)', exercises: [
            { group: 'COSTAS', name: 'Puxada Alta (Pegada Pronada Aberta)', sets: 4, taf: '' },
            { group: 'COSTAS', name: 'Remada Curvada com Barra (Pegada Pronada)', sets: 4, taf: '' },
            { group: 'COSTAS', name: 'Remada na Polia Baixa (Triângulo/Fechada)', sets: 4, taf: '' },
            { group: 'BÍCEPS', name: 'Rosca Direta com Barra (W ou Reta)', sets: 4, taf: '' },
            { group: 'BÍCEPS', name: 'Rosca Martelo com Halteres (Alternada)', sets: 4, taf: '' },
            { group: 'TRAPÉZIO', name: 'Encolhimento com Halteres (Em Pé)', sets: 4, taf: '' }
        ]},
        2: { name: 'Treino C: Pernas Foco Quadríceps + TAF (Ter)', exercises: [
            { group: 'QUADRÍCEPS', name: 'Agachamento Livre / Hack Machine', sets: 4, taf: 'TAF Barra Fixa (3x Máx. Reps)' },
            { group: 'QUADRÍCEPS', name: 'Leg Press 45º (Pés Baixos)', sets: 4, taf: '' },
            { group: 'QUADRÍCEPS', name: 'Cadeira Extensora', sets: 4, taf: '' },
            { group: 'POSTERIOR', name: 'Cadeira Flexora', sets: 4, taf: '' },
            { group: 'PANTURRILHA', name: 'Elevação de Panturrilha em Pé', sets: 4, taf: '' }
        ]},
        3: { name: 'Treino D: Ombros Completo + Core (Qua)', exercises: [
            { group: 'OMBRO', name: 'Desenvolvimento Militar (Halteres ou Máquina)', sets: 4, taf: '' },
            { group: 'OMBRO', name: 'Elevação Lateral com Cabo (Unilateral)', sets: 4, taf: '' },
            { group: 'OMBRO', name: 'Crucifixo Inverso na Máquina', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Tríceps Coice (Unilateral)', sets: 4, taf: '' },
            { group: 'BÍCEPS', name: 'Rosca Scott (Máquina ou Banco)', sets: 4, taf: '' },
            { group: 'ABDÔMEN', name: 'Prancha (Máx. Tempo)', sets: 4, taf: '' }
        ]},
        4: { name: 'Treino E: Push Leve / Peito Leve + TAF (Qui)', exercises: [
            { group: 'PEITO', name: 'Supino Reto Máquina (ou Halteres)', sets: 4, taf: 'TAF Barra Fixa (3x Máx. Reps)' },
            { group: 'PEITO', name: 'Flexão de Braço (até a Falha)', sets: 4, taf: '' },
            { group: 'OMBRO', name: 'Elevação Frontal com Halteres', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Tríceps Pulley V invertido', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Tríceps Paralelas (máquina assistida)', sets: 4, taf: '' },
            { group: 'PANTURRILHA', name: 'Elevação de Panturrilha Sentado', sets: 4, taf: '' }
        ]},
        5: { name: 'Treino F: Pernas Foco Posterior e Glúteo (Sex)', exercises: [
            { group: 'POSTERIOR', name: 'Stiff com Halteres', sets: 4, taf: '' },
            { group: 'POSTERIOR', name: 'Mesa Flexora (Deitado)', sets: 4, taf: '' },
            { group: 'GLÚTEO', name: 'Elevação Pélvica (Hip Thrust)', sets: 4, taf: '' },
            { group: 'QUADRÍCEPS', name: 'Cadeira Extensora (Unilateral)', sets: 4, taf: '' },
            { group: 'PANTURRILHA', name: 'Elevação de Panturrilha no Leg Press', sets: 4, taf: '' },
            { group: 'ABDÔMEN', name: 'Rotação de Tronco (Máquina/Cabo)', sets: 4, taf: '' }
        ]},
        6: { name: 'Treino G: Braços Foco Isolação + Complementar (Sáb)', exercises: [
            { group: 'BÍCEPS', name: 'Rosca Concentrada (Sentado)', sets: 4, taf: '' },
            { group: 'BÍCEPS', name: 'Rosca Inversa (Barra/Cabo)', sets: 4, taf: '' },
            { group: 'BÍCEPS', name: 'Rosca 21 (Variação de Amplitude)', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Tríceps Testa com Barra W', sets: 4, taf: '' },
            { group: 'TRÍCEPS', name: 'Extensão de Tríceps Acima da Cabeça (Corda)', sets: 4, taf: '' },
            { group: 'ANTEBRAÇO', name: 'Rosca de Punho (Palma para Cima)', sets: 4, taf: '' },
            { group: 'ANTEBRAÇO', name: 'Rosca de Punho (Palma para Baixo)', sets: 4, taf: '' },
            { group: 'ABDÔMEN', name: 'Elevação de Pernas (Pendurado ou Deitado)', sets: 4, taf: '' }
        ]}
    };

    /**
     * Alterna o status de conclusão de uma série e atualiza a UI.
     * @param {number} exIndex - Índice do exercício.
     * @param {number} setIndex - Índice da série (0 a 3).
     */
    window.toggleSetCompletion = (exIndex, setIndex) => {
        if (currentSetStatus[exIndex] && setIndex < currentSetStatus[exIndex].length) {
            // Alterna o estado
            const isCompleted = currentSetStatus[exIndex][setIndex];
            currentSetStatus[exIndex][setIndex] = !isCompleted;
            
            // Atualiza a aparência do botão no DOM
            const button = document.querySelector(`#setBtn-${exIndex}-${setIndex}`);
            if (button) {
                if (isCompleted) {
                    // Se estava completo, torna incompleto
                    button.classList.remove('bg-red-600', 'text-white');
                    button.classList.add('bg-gray-200', 'text-gray-700', 'hover:bg-gray-300');
                } else {
                    // Se estava incompleto, torna completo
                    button.classList.remove('bg-gray-200', 'text-gray-700', 'hover:bg-gray-300');
                    button.classList.add('bg-red-600', 'text-white');
                }
            }
        }
    };

    /**
     * Alterna o dia de treino atual, recarrega o histórico e renderiza a UI.
     * @param {number} newIndex - O índice do novo dia de treino (0=A, 6=G).
     */
    function changeWorkoutDay(newIndex) {
        newIndex = parseInt(newIndex);
        if (newIndex >= 0 && newIndex <= 6 && newIndex !== currentDayIndex) {
            currentDayIndex = newIndex;
            currentWorkout = workoutPlan[currentDayIndex];
            
            // Inicializa o status das séries para o novo treino (todas incompletas)
            currentSetStatus = currentWorkout.exercises.map(ex => 
                Array(ex.sets).fill(false)
            );
            
            highlightSelectedDay();

            document.getElementById('loadingIndicator').classList.remove('hidden');
            document.getElementById('workoutHeader').textContent = 'Carregando...';

            loadLastPerformance().then(() => {
                renderWorkout();
                document.getElementById('loadingIndicator').classList.add('hidden');
            }).catch(e => {
                console.error("Erro ao carregar após mudança de dia:", e);
                document.getElementById('loadingIndicator').textContent = 'Erro ao carregar dados.';
            });
        }
    }
    window.changeWorkoutDay = changeWorkoutDay; // Expõe para uso nos botões HTML

    /**
     * Renderiza os botões do seletor de dias A-G.
     */
    function renderDaySelector() {
        const selectorContainer = document.getElementById('daySelector');
        selectorContainer.innerHTML = dayLabels.map((label, index) => `
            <button id="dayBtn-${index}" onclick="window.changeWorkoutDay(${index})"
                class="flex-1 py-1 px-1 text-xs font-bold rounded-lg transition duration-150 ease-in-out">
                ${label}
            </button>
        `).join('');
        highlightSelectedDay(); // Define a cor inicial do botão
    }
    
    /**
     * Atualiza o estilo do botão de dia de treino selecionado.
     */
    function highlightSelectedDay() {
        dayLabels.forEach((_, index) => {
            const btn = document.getElementById(`dayBtn-${index}`);
            if (btn) {
                 if (index === currentDayIndex) {
                    btn.className = 'flex-1 py-1 px-1 text-xs font-bold rounded-lg transition duration-150 ease-in-out bg-white text-red-700 shadow-inner';
                } else {
                    btn.className = 'flex-1 py-1 px-1 text-xs font-bold rounded-lg transition duration-150 ease-in-out bg-red-600 hover:bg-red-500 text-white opacity-70';
                }
            }
        });
    }

    // Exposição das funções e variáveis necessárias para o escopo global do HTML
    window.initializeApp = async () => {
        try {
            app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);

            // Tenta autenticar com o token customizado ou anonimamente
            const authPromise = initialAuthToken
                ? signInWithCustomToken(auth, initialAuthToken)
                : signInAnonymously(auth);

            await authPromise;
            currentUserId = auth.currentUser.uid;
            console.log("Firebase Autenticado. UserID:", currentUserId);

            // Exibe o UserID na UI
            document.getElementById('userIdDisplay').textContent = `ID do Usuário: ${currentUserId.substring(0, 8)}...`;

            // --- Configuração Inicial do Treino ---
            currentWorkout = workoutPlan[currentDayIndex];
            // Inicializa o status das séries para o treino do dia (todas incompletas)
            currentSetStatus = currentWorkout.exercises.map(ex => Array(ex.sets).fill(false)); 
            renderDaySelector();
            // --- FIM Configuração Inicial ---

            // Carrega o histórico e renderiza o treino
            await loadLastPerformance();
            renderWorkout();
            setupRestTimer();

        } catch (error) {
            console.error("Erro na inicialização do Firebase ou autenticação:", error);
            document.getElementById('loadingIndicator').textContent = "Erro ao carregar o app. Verifique a conexão.";
            window.showCustomAlert('Erro de Conexão', 'Não foi possível carregar o histórico. Tente recarregar a página.');
        }
    };

    /**
     * Busca a última carga e repetição para cada exercício no cache global.
     */
    async function loadLastPerformance() {
        if (!currentUserId) return;

        console.log("Buscando último desempenho...");
        document.getElementById('loadingIndicator').textContent = "Carregando histórico...";
        lastPerformanceCache = {}; // Limpa o cache

        // Crie um array de todas as promessas de busca
        const exerciseNames = currentWorkout.exercises.map(e => e.name);
        
        const collectionPath = `/artifacts/${appId}/users/${currentUserId}/workout_logs`;

        try {
            // Buscamos as 15 sessões mais recentes. 
            // O número 15 é heurístico para cobrir 2 ciclos completos de 7 dias, garantindo a busca.
            const sessionQuery = query(
                collection(db, collectionPath),
                orderBy('date', 'desc'),
                limit(15) 
            );
            
            const snapshot = await getDocs(sessionQuery);
            
            if (!snapshot.empty) {
                // Itera sobre as últimas 15 sessões para encontrar o recorde de cada exercício
                snapshot.docs.forEach(doc => {
                    const session = doc.data();
                    if (session.exercises) {
                        session.exercises.forEach(log => {
                            // Se ainda não cacheamos o recorde (primeiro encontrado é o mais recente)
                            if (!lastPerformanceCache[log.name]) {
                                lastPerformanceCache[log.name] = {
                                    reps: log.reps,
                                    load: log.load
                                };
                            }
                        });
                    }
                });
            }
        } catch (e) {
            console.error(`Erro ao buscar log:`, e);
        }

        document.getElementById('loadingIndicator').classList.add('hidden');
        console.log("Cache de desempenho carregado:", lastPerformanceCache);
    }


    /**
     * Renderiza o treino do dia (A-G) na interface.
     */
    function renderWorkout() {
        const workoutContainer = document.getElementById('workoutContainer');
        const header = document.getElementById('workoutHeader');

        header.textContent = currentWorkout.name;
        workoutContainer.innerHTML = '';

        currentWorkout.exercises.forEach((ex, index) => {
            const lastPerf = lastPerformanceCache[ex.name];
            const lastReps = lastPerf ? lastPerf.reps : '-';
            const lastLoad = lastPerf ? lastPerf.load : '-';
            
            // Gera o HTML dos círculos de sets
            const setCirclesHTML = currentSetStatus[index].map((isCompleted, setIdx) => `
                <button id="setBtn-${index}-${setIdx}" onclick="window.toggleSetCompletion(${index}, ${setIdx})"
                    class="set-circle w-10 h-10 rounded-full flex items-center justify-center text-sm font-bold transition duration-150 ease-in-out shadow-lg
                    ${isCompleted ? 'bg-red-600 text-white' : 'bg-gray-200 text-gray-700 hover:bg-gray-300'}"
                    aria-label="Set ${setIdx + 1} Completion">
                    ${setIdx + 1}
                </button>
            `).join('');

            const exerciseHTML = `
                <div class="bg-white p-4 rounded-xl shadow-md mb-4 border-l-4 border-red-600">
                    <h3 class="text-xl font-bold text-gray-800 mb-2 flex justify-between items-center">
                        <span>${index + 1}. ${ex.name}</span>
                        <span class="text-sm font-semibold text-red-600">${ex.group}</span>
                    </h3>
                    
                    <!-- Círculos de Conclusão de Séries -->
                    <div class="flex space-x-2 mb-4 justify-between">
                        ${setCirclesHTML}
                    </div>

                    <!-- Último Desempenho (Recorde) -->
                    <div class="text-xs text-gray-500 mb-3 p-2 bg-red-50 rounded-lg">
                        <p class="font-semibold">Último Recorde (Carga/Reps):</p>
                        <p class="font-mono text-base text-red-700">${lastLoad}kg / ${lastReps} Reps</p>
                    </div>

                    <!-- Detalhes e TAF -->
                    <p class="text-sm text-gray-600 mb-3">
                        <span class="font-semibold">Séries:</span> ${ex.sets} | 
                        <span class="font-semibold">Foco:</span> ${ex.taf || 'Ativação Máxima'}
                    </p>

                    <!-- Inputs para Reps e Carga ATUAL -->
                    <div class="flex space-x-2 mt-4">
                        <input type="number" data-id="${index}" data-type="reps" placeholder="Reps (Atual)" class="w-1/2 p-2 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500 text-sm" required>
                        <input type="number" data-id="${index}" data-type="load" placeholder="Carga (kg)" class="w-1/2 p-2 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500 text-sm" required>
                    </div>
                </div>
            `;
            workoutContainer.insertAdjacentHTML('beforeend', exerciseHTML);
        });

        // Adiciona o botão de salvar no final
        workoutContainer.insertAdjacentHTML('beforeend', `
            <button id="saveWorkoutBtn" class="w-full bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-4 rounded-xl shadow-lg transition duration-150 ease-in-out mt-6" onclick="window.saveWorkoutLog()">
                Finalizar Treino e Salvar Log
            </button>
        `);
    }

    /**
     * Salva o log do treino atual no Firestore.
     */
    window.saveWorkoutLog = async () => {
        const repsInputs = document.querySelectorAll('input[data-type="reps"]');
        const loadInputs = document.querySelectorAll('input[data-type="load"]');
        const workoutLog = {
            date: new Date().toISOString(),
            dayName: currentWorkout.name,
            workoutDayId: currentDayIndex,
            userId: currentUserId,
            exercises: []
        };
        
        let allInputsFilled = true;
        let allSetsCompleted = true; // Nova checagem para conclusão de séries

        currentWorkout.exercises.forEach((ex, index) => {
            const repsInput = repsInputs[index];
            const loadInput = loadInputs[index];

            const reps = parseInt(repsInput.value);
            const load = parseFloat(loadInput.value);
            
            // Checagem de Repetições e Carga (Inputs preenchidos)
            if (isNaN(reps) || isNaN(load) || reps <= 0 || load < 0) {
                repsInput.classList.add('border-yellow-500', 'ring-2');
                loadInput.classList.add('border-yellow-500', 'ring-2');
                allInputsFilled = false;
                return;
            } else {
                repsInput.classList.remove('border-yellow-500', 'ring-2');
                loadInput.classList.remove('border-yellow-500', 'ring-2');
            }

            // Checagem de Conclusão de Séries
            const setsCompleted = currentSetStatus[index].every(status => status === true);
            if (!setsCompleted) {
                allSetsCompleted = false;
            }

            // Prepara o log da série para o Firestore
            const setsLog = currentSetStatus[index].map((status, setIdx) => ({
                set: setIdx + 1,
                completed: status
            }));


            workoutLog.exercises.push({
                name: ex.name,
                group: ex.group,
                sets: ex.sets,
                reps: reps,
                load: load,
                restTime: '120s',
                setsStatus: setsLog // Adiciona o status das séries ao log
            });
        });

        if (!allInputsFilled) {
            window.showCustomAlert('Atenção', 'Por favor, preencha as Repetições e Cargas para todos os exercícios antes de salvar. Valores devem ser maiores que zero (exceto carga, que pode ser zero).');
            return;
        }
        
        if (!allSetsCompleted) {
            window.showCustomAlert('Atenção', 'Você deve marcar todas as séries (os círculos 1 a 4) como concluídas para todos os exercícios antes de salvar o treino.');
            return;
        }


        try {
            const collectionPath = `/artifacts/${appId}/users/${currentUserId}/workout_logs`;
            await addDoc(collection(db, collectionPath), workoutLog);
            
            // Sucesso: atualiza o cache e re-renderiza para mostrar os novos recordes
            await loadLastPerformance(); 
            // Reinicializa o status das séries para a nova sessão
            currentSetStatus = currentWorkout.exercises.map(ex => Array(ex.sets).fill(false)); 
            renderWorkout();
            
            window.showCustomAlert('Sucesso!', 'Log de treino salvo com sucesso no Firestore. Seus novos recordes estão definidos!');
        } catch (e) {
            console.error("Erro ao salvar o log:", e);
            window.showCustomAlert('Erro', 'Não foi possível salvar o log. Verifique o console para detalhes.');
        }
    };

    // --- Lógica do Timer de Descanso ---
    let timerInterval = null;
    let timeRemaining = 120; // 2 minutos em segundos
    const timerDisplay = document.getElementById('timerDisplay');
    const timerBtn = document.getElementById('timerButton');
    const timerResetBtn = document.getElementById('timerResetButton');

    function setupRestTimer() {
        timerBtn.onclick = toggleTimer;
        timerResetBtn.onclick = resetTimer;
        updateTimerDisplay(); // Garante o display inicial
    }

    function formatTime(seconds) {
        const minutes = Math.floor(seconds / 60);
        const secs = seconds % 60;
        return `${minutes.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
    }

    function updateTimerDisplay() {
        timerDisplay.textContent = formatTime(timeRemaining);
        
        // Atualiza a cor de fundo do botão baseado no tempo
        if (timeRemaining <= 10) {
             timerBtn.style.backgroundColor = '#ef4444'; // Vermelho
        } else if (timeRemaining <= 30) {
            timerBtn.style.backgroundColor = '#f97316'; // Laranja
        } else {
            timerBtn.style.backgroundColor = '#10b981'; // Verde
        }
    }

    function toggleTimer() {
        if (timerInterval) {
            // Pausa
            clearInterval(timerInterval);
            timerInterval = null;
            timerBtn.textContent = 'INICIAR DESCanso (2:00)';
            timerBtn.classList.remove('bg-yellow-500', 'hover:bg-yellow-600');
            timerBtn.classList.add('bg-green-500', 'hover:bg-green-600');
        } else {
            // Inicia
            if (timeRemaining === 0) {
                timeRemaining = 120; // Reseta se já tiver terminado
            }
            timerBtn.textContent = 'PAUSAR DESCanso';
            timerBtn.classList.remove('bg-green-500', 'hover:bg-green-600');
            timerBtn.classList.add('bg-yellow-500', 'hover:bg-yellow-600');

            timerInterval = setInterval(() => {
                if (timeRemaining > 0) {
                    timeRemaining--;
                    updateTimerDisplay();
                } else {
                    clearInterval(timerInterval);
                    timerInterval = null;
                    timerBtn.textContent = 'DESCANSO CONCLUÍDO!';
                    timerBtn.classList.remove('bg-yellow-500', 'hover:bg-yellow-600');
                    timerBtn.classList.add('bg-red-600');
                    window.showCustomAlert('Atenção', 'Seu tempo de descanso de 2 minutos terminou. Hora de voltar à ação!');
                }
            }, 1000);
        }
    }

    function resetTimer() {
        clearInterval(timerInterval);
        timerInterval = null;
        timeRemaining = 120;
        updateTimerDisplay();
        timerBtn.textContent = 'INICIAR DESCanso (2:00)';
        timerBtn.classList.remove('bg-yellow-500', 'bg-red-600');
        timerBtn.classList.add('bg-green-500', 'hover:bg-green-600');
    }


    // --- Lógica do Modal/Alerta Personalizado (Substitui alert()) ---
    window.showCustomAlert = (title, message) => {
        document.getElementById('modalTitle').textContent = title;
        document.getElementById('modalMessage').textContent = message;
        document.getElementById('customAlertModal').classList.remove('hidden');
    };

    document.getElementById('closeModalButton').onclick = () => {
        document.getElementById('customAlertModal').classList.add('hidden');
    };

    // Inicia o app ao carregar a janela
    window.onload = window.initializeApp;

</script>

<!-- Estrutura Principal do App -->
<div class="min-h-screen bg-gray-100 flex flex-col">

    <!-- Header Fixo (Informa o Treino do Dia) -->
    <header class="bg-red-700 text-white p-4 shadow-lg sticky top-0 z-10 rounded-b-xl">
        <h1 class="text-2xl font-extrabold mb-1">🔥 Treinamento Bombeiro</h1>
        
        <!-- Seletor de Dia de Treino (A-G) -->
        <div id="daySelector" class="flex justify-between space-x-1 mb-3">
            <!-- Buttons A to G serão renderizados aqui pelo JS -->
        </div>

        <h2 id="workoutHeader" class="text-xl font-bold bg-red-800 p-2 rounded-lg text-center">Carregando...</h2>
        <p id="userIdDisplay" class="text-xs mt-1 opacity-75">ID do Usuário: Carregando...</p>
    </header>

    <!-- Indicador de Carregamento -->
    <div id="loadingIndicator" class="text-center p-4 text-red-600 font-semibold">
        Inicializando Firebase...
    </div>

    <!-- Conteúdo Principal do Treino (Rolável) -->
    <main class="flex-grow p-4 scrollable-content">
        
        <!-- Timer de Descanso Integrado -->
        <section class="bg-gray-800 text-white p-4 rounded-xl shadow-2xl mb-6 flex flex-col sm:flex-row items-center justify-between gap-4">
            <div class="flex flex-col items-center">
                <p class="text-lg font-bold mb-2 text-red-400">Tempo de Descanso (2 Minutos)</p>
                <div id="timerDisplay" class="text-6xl font-extrabold font-mono">02:00</div>
            </div>
            <div class="flex flex-col gap-2 w-full sm:w-auto">
                <button id="timerButton" class="timer-button w-full sm:w-48 bg-green-500 hover:bg-green-600 text-white font-bold py-3 rounded-xl shadow-md">
                    INICIAR DESCanso (2:00)
                </button>
                <button id="timerResetButton" class="w-full sm:w-48 bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 rounded-xl text-sm">
                    Reiniciar
                </button>
            </div>
        </section>

        <!-- Lista de Exercícios -->
        <section id="workoutContainer" class="mt-4">
            <!-- Exercícios serão renderizados aqui pelo JS -->
        </section>

    </main>
</div>

<!-- Modal/Alerta Personalizado (Substitui alert()) -->
<div id="customAlertModal" class="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center hidden z-50">
    <div class="bg-white p-6 rounded-xl shadow-2xl max-w-sm w-full mx-4">
        <h3 id="modalTitle" class="text-xl font-bold mb-3 text-red-600">Título</h3>
        <p id="modalMessage" class="text-gray-700 mb-4">Mensagem</p>
        <button id="closeModalButton" class="w-full bg-red-600 hover:bg-red-700 text-white font-bold py-2 rounded-lg transition duration-150">
            Entendido
        </button>
    </div>
</div>

</body>
</html>