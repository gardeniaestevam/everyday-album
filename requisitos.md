# Everyday Album - Documento de Requisitos

Este documento descreve as especificações, requisitos e regras de negócio para o desenvolvimento do **Everyday an Album**, um jogo web diário inspirado no Heardle/Wordle onde o objetivo é adivinhar um álbum secreto por dia.

---

## 1. Visão Geral do Sistema
O **Everyday an Album** é um jogo de adivinhação musical. Todos os dias, um novo álbum é selecionado como o "álbum secreto".

---

## 2. Requisitos Funcionais (RF)

### RF01 - Seleção do Álbum Diário **MVP**
* **Descrição**: O sistema backend deve selecionar automaticamente um único álbum secreto para cada dia.
* **Detalhes**: A troca do álbum deve ocorrer à meia-noite (00:00) de acordo com o fuso horário configurado.

### RF02 - Input com Autocomplete/Sugestões de Busca **MVP**
* **Descrição**: O campo de texto para o usuário digitar o seu palpite deve exibir sugestões em tempo real (autocomplete) para evitar erros de digitação.
* **Origem**: A busca deve consultar a API do Spotify/Last.fm e retornar opções no formato `Nome do Álbum - Nome do Artista`.

### RF03 - Validação de Palpites 
* **Descrição**: O sistema deve comparar o palpite selecionado pelo usuário com o álbum secreto do dia.
* **Feedback**:
  * Se estiver correto: Finalizar o jogo como vitória.
  * Se estiver incorreto: Registrar uma tentativa.

### RF04 - Ranking de similaridade **MVP**
* **Descrição**: O sistema deve comparar o palpite selecionado pelo usuário com o álbum secreto do dia.
* **Feedback**:
  * Registrar no mínimo uma ou no máximo duas tags de similaridade do álbum. Para isso, comparar as tags do álbum secreto com as tags do palpite e registrar as tags que estiverem corretas. Ordenar as tags por similaridade e mostrar as tags com maior similaridade primeiro.

### RF05 - Dicas de Texto (Metadados)
* **Descrição**: Opção de dicas de texto obtidas da API do Last.fm 
* **Gatilhos de Dicas**:
  * **1ª escolha de dica**: Revelar as "Tags" (Gêneros musicais) do álbum (ex: Rock, 80s, Pop). **MVP**
  * **2ª escolha de dica**: Revelar o resumo da Biografia/Wiki do álbum obtida no Last.fm, censurando menções diretas ao nome do álbum e do artista. **MVP**
  * **3ª escolha de dica**: Revelar a capa do álbum borrada. **MVP**
  * **4ª escolha de dica**: Revelar um audio da primeira musica mais tocada (com mais streams) do album por 2 segundos.
  * **5ª escolha de dica**: Revelar o título de uma musica do album. **MVP**

### RF06 - Salvamento de Estado Local
* **Descrição**: O progresso do dia atual do usuário deve ser salvo localmente para evitar perda de dados em recarregamentos de página.
* **Detalhes**: Usar `LocalStorage` para armazenar as tentativas já feitas e o estado de finalização (vitória/derrota) do dia.

### RF07 - Compartilhamento de Resultados
* **Descrição**: Após o término do jogo (acerto ou fim das tentativas), o sistema deve gerar uma mensagem de texto compartilhável com quadradinhos coloridos representando a performance do usuário.
* **Exemplo**:
  > Everyday Album #42
  > 🟥🟥🟨🟥🟩⬜ (Acertou na 5ª tentativa)
  > jogue em: [link]

---

## 3. Requisitos Não-Funcionais (RNF)

### RNF01 - Design Responsivo (Mobile-First) **MVP**
* **Descrição**: A interface deve ser projetada prioritariamente para dispositivos móveis, mas funcionar perfeitamente em desktop.
* **Estilo**: Preferencialmente dark mode por padrão, porém com uma pegada pop teen, colorida, divertida e moderna.

### RNF02 - Otimização de Busca (Debounce) **MVP**
* **Descrição**: A busca por palpites no input deve possuir um mecanismo de *debounce* (esperar o usuário parar de digitar por ~300ms) para não sobrecarregar as APIs com requisições repetidas a cada caractere digitado.

### RNF03 - Segurança das Credenciais **MVP**
* **Descrição**: As chaves de API do Spotify e Last.fm **nunca** devem ser expostas diretamente no navegador do cliente (frontend).
* **Solução**: O frontend deve se comunicar apenas com um servidor backend próprio, que por sua vez fará a ponte segura com as APIs externas utilizando as variáveis do `.env`.

### RNF04 - Disponibilidade **MVP**
* **Descrição**: O servidor deve estar disponível 24/7.

### RNF05 - Desempenho **MVP**
* **Descrição**: O servidor deve responder às requisições em menos de 2 segundos.

### RF08 - Login 
* **Descrição**: O usuário deve fazer login com o spotify para jogar.
* **Detalhes**: O usuário deve fazer login com o spotify para jogar. O login deve ser feito com o spotify.

---

## 4. Regras de Negócio (RN)

### RN01 - Uma Partida por Dia **MVP**
* Cada usuário pode jogar apenas uma vez por dia. Uma vez finalizado o jogo do dia (vitória ou derrota), a tela de resultados será mostrada fixamente até o próximo reset diário.

### RN02 - Jogos antigos
* O usuário deve poder jogar os jogos dos dias anteriores.

