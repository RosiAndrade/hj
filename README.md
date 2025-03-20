//Rosilaine
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Tarefas</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        ul { list-style: none; padding: 0; }
        li { padding: 10px; border-bottom: 1px solid #ddd; }
        button { margin-left: 10px; }
    </style>
</head>
<body>
    <h1>Lista de Tarefas</h1>
    <input type="text" id="novaTarefa" placeholder="Digite uma nova tarefa">
    <button onclick="adicionarTarefa()">Adicionar</button>
    <ul id="listaTarefas"></ul>

    <script>
        const API_URL = 'http://localhost:3000/tarefas';

        async function carregarTarefas() {
            const resposta = await fetch(API_URL);
            const tarefas = await resposta.json();
            const lista = document.getElementById('listaTarefas');
            lista.innerHTML = '';
            tarefas.forEach(tarefa => {
                lista.innerHTML += `<li>${tarefa.descricao} - ${tarefa.status}
                    <button onclick="concluirTarefa(${tarefa.id})">Concluir</button>
                    <button onclick="deletarTarefa(${tarefa.id})">Excluir</button>
                </li>`;
            });
        }

        async function adicionarTarefa() {
            const descricao = document.getElementById('novaTarefa').value;
            await fetch(API_URL, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ descricao }) });
            carregarTarefas();
        }

        async function concluirTarefa(id) {
            await fetch(`${API_URL}/${id}`, { method: 'PUT' });
            carregarTarefas();
        }

        async function deletarTarefa(id) {
            await fetch(`${API_URL}/${id}`, { method: 'DELETE' });
            carregarTarefas();
        }

        carregarTarefas();
    </script>
</body>
</html>
