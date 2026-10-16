# Forca: como publicar no GitHub

São duas partes:

- **Parte A (GitHub):** coloca o jogo no ar com um link fixo. Leva uns 10 minutos.
- **Parte B (Firebase):** liga o ranking entre todos os aparelhos. Leva uns 15 minutos.

O jogo já funciona depois da Parte A. Sem a Parte B, os pontos ficam só no aparelho de cada pessoa.

Arquivos desta pasta:

| Arquivo | Para que serve |
|---|---|
| `index.html` | O jogo |
| `firebase-config.js` | Onde você cola a chave do Firebase (Parte B) |
| `firestore.rules` | Regras de segurança do ranking (Parte B) |
| `LEIA-ME.md` | Este guia |

---

## Parte A: publicar no GitHub Pages

1. Crie uma conta grátis em **github.com**, caso ainda não tenha. O nome de usuário vai aparecer no link do jogo.
2. No canto superior direito, clique em **+** e depois em **New repository**.
   - Em *Repository name*, escreva `forca`.
   - Deixe marcado **Public**.
   - Clique em **Create repository**.
3. Na página do repositório, clique no link **uploading an existing file**.
4. Arraste os arquivos `index.html` e `firebase-config.js`. Depois clique em **Commit changes**.
5. Vá em **Settings**, depois em **Pages** no menu da esquerda.
   - Em *Source*, escolha **Deploy from a branch**.
   - Em *Branch*, escolha **main** e a pasta **/ (root)**.
   - Clique em **Save**.
6. Espere 1 ou 2 minutos e atualize a página. O link vai aparecer no topo:

   `https://SEU-USUARIO.github.io/forca/`

Esse é o link para mandar no WhatsApp. O jogo fica no ar 24 horas por dia, de graça.

---

## Parte B: ligar o ranking com o Firebase

### 1. Criar o projeto
1. Acesse **console.firebase.google.com** e entre com uma conta Google.
2. Clique em **Criar projeto**, dê o nome `forca` e siga as telas. O Google Analytics pode ficar desligado.

### 2. Ativar o login anônimo
Esse login identifica cada aparelho sem pedir nada ao jogador.

1. No menu da esquerda, abra **Criação**, depois **Authentication**, e clique em **Vamos começar**.
2. Na aba **Método de login**, escolha **Anônimo**, ative e clique em **Salvar**.
3. Na aba **Configurações**, abra **Domínios autorizados** e adicione `SEU-USUARIO.github.io`.

### 3. Pegar a chave do projeto
1. Clique na engrenagem ao lado de *Visão geral do projeto* e depois em **Configurações do projeto**.
2. Em *Seus apps*, clique no ícone **</>** (Web). Dê um apelido qualquer e clique em **Registrar app**. Não precisa marcar o Hosting.
3. O Firebase vai mostrar um bloco `const firebaseConfig = { ... }`. Copie os valores que estão dentro dele.
4. No GitHub, abra o arquivo `firebase-config.js` e clique no **lápis** para editar. Troque o conteúdo para ficar assim, usando os seus valores:

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "forca-xxxx.firebaseapp.com",
  projectId: "forca-xxxx",
  storageBucket: "forca-xxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

5. Clique em **Commit changes**.

Essa chave pode ficar pública. Quem protege o ranking são as regras do passo 4.

### 4. Criar o banco do ranking
1. No menu da esquerda, abra **Criação** e depois **Firestore Database**. Clique em **Criar banco de dados**.
2. Em local, escolha **southamerica-east1 (São Paulo)**.
3. Escolha **Modo de produção** e conclua.
4. Abra a aba **Regras**. Apague tudo, cole o conteúdo do arquivo `firestore.rules` e clique em **Publicar**.

### 5. Testar
Abra o link do jogo, ganhe uma partida e toque no troféu. O seu aparelho deve aparecer no ranking.

Se não aparecer, abra o jogo no computador, aperte F12 e veja a aba *Console*. As mensagens de erro começam com "Firebase" ou "Ranking" e dizem qual passo faltou.

---

## Bom saber
- **Custo:** o plano gratuito (Spark) aguenta com folga milhares de partidas por dia. Não é preciso cadastrar cartão.
- **Cada aparelho tem uma linha no ranking.** Se a pessoa limpar os dados do navegador, ela volta como um aparelho novo.
- **Trapaça:** como o jogo roda no navegador, alguém que entenda de programação conseguiria inventar uma pontuação. Para jogar entre amigos, isso raramente é problema. Se acontecer, você pode apagar a linha da pessoa no Firestore Database.
- **Atualizar o jogo:** para trocar o jogo, basta enviar um novo `index.html` para o repositório. O link continua o mesmo.
