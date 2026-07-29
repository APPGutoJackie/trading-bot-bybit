# trading-bot-bybit
Robô de trading automatizado - Bybit


​1. ​Arquitetura Geral: Um resumo visual/textual de como o TradingView se conecta ao VPS via Webhook HTTP (porta 80) e como o Flask processa os JSONs.
​Ambiente Virtual e Dependências: Comandos exatos para criar o ambiente (venv), instalar o Flask e rodá-lo em segundo plano.
​Modelos de JSON: Os payloads utilizados para Long, Short, Fim de Long e Fim de Short.
​Comandos Úteis de VPS: O passo a passo para gerenciar o processo do Flask via terminal (pkill, nohup ou logs).
​2. A Planilha de Desafios Superados (Log de Aprendizado)
​Podemos criar uma tabela estruturada para você acompanhar a evolução técnica. Um exemplo de como podemos formatar:

Desafio / ObstáculoCausa RaizAção Realizada para SoluçãoAprendizado / Boas Práticas
Bloqueio de porta pelo TradingViewO TradingView exige HTTP na porta 80 ou HTTPS na 443, mas o Flask rodava na porta 5000.Redirecionamento/execução do bot na porta 80 utilizando privilégios de administrador (sudo).Sempre verificar as restrições de protocolo e portas de plataformas externas de webhook.
Frequência de disparo de alertasAlertas configurados como "uma vez por minuto" geravam múltiplos disparos desnecessários.Ajuste para disparar "uma vez por barra" (fechamento) para validar o sinal definitivo.Garantir que o sinal de trading seja confirmado para evitar falsos gatilhos por oscilação intrabar.
Estruturação de Alertas de SaídaO sistema inicial só previa entradas, faltando o controle de encerramento de posições.Criação de quatro alertas distintos mapeando ações específicas (long, close_long, short, close_short).O ciclo de vida de uma ordem automatizada exige controle explícito de abertura e fechamento.

2. Configuração do Ambiente no VPS
​2.1. Criação e Ativação do Ambiente Virtual (venv)

# Criar o ambiente virtual
python3 -m venv venv

# Ativar o ambiente virtual
source venv/bin/activate

2.2. Instalação de Dependências

pip install flask requests ccxt

2.3. Executando o Servidor Flask na Porta 80

​Como o TradingView exige requisições HTTP na porta padrão 80 (ou HTTPS na 443), o bot deve ser executado com privilégios de administrador (sudo):

pkill -f bot.py && sudo python3 bot.py > flask.log 2>&1 &

3. Payloads de Alertas (JSON para o TradingView)

No TradingView, o campo Mensagem do alerta deve conter um JSON estruturado com a ação desejada:
​Long (Abertura de Compra)

{
  "action": "long",
  "symbol": "XPLUSDC.P",
  "price": "{{close}}"
}

Fim de Long (Fechamento de Compra)

{
  "action": "close_long",
  "symbol": "XPLUSDC.P",
  "price": "{{close}}"
}

Short (Abertura de Venda)

{
  "action": "short",
  "symbol": "XPLUSDC.P",
  "price": "{{close}}"
}

4. Boas Práticas e Governança de Versões
​Controle de Versão: Utilize o Git para registrar o histórico de evolução do código.
​Segurança: Mantenha credenciais de API em arquivos ocultos ou variáveis de ambiente e utilize sempre o .gitignore para evitar vazamentos no GitHub.
​Evolução Futura: Planeje a migração da arquitetura para suportar múltiplos usuários (multi-tenant) nas próximas versões (v1.0.0 e superiores).
"""
​with open("README.md", "w", encoding="utf-8") as f:
f.write(readme_content)
​challenges_data = [
["Desafio / Obstáculo", "Causa Raiz", "Ação Realizada para Solução", "Aprendizado / Boas Práticas"],
["Bloqueio de porta pelo TradingView", "O TradingView exige HTTP na porta 80 ou HTTPS na 443, mas o Flask rodava na porta 5000.", "Redirecionamento/execução do bot na porta 80 utilizando privilégios de administrador (sudo).", "Sempre verificar as restrições de protocolo e portas de plataformas externas de webhook."],
["Frequência de disparo de alertas", "Alertas configurados como 'uma vez por minuto' geravam múltiplos disparos desnecessários.", "Ajuste para disparar 'uma vez por barra' (fechamento) para validar o sinal definitivo.", "Garantir que o sinal de trading seja confirmado para evitar falsos gatilhos por oscilação intrabar."],
["Estruturação de Alertas de Saída", "O sistema inicial só previa entradas, faltando o controle de encerramento de posições.", "Criação de quatro alertas distintos mapeando ações específicas (long, close_long, short, close_short).", "O ciclo de vida de uma ordem automatizada exige controle explícito de abertura e fechamento."]
]
​with open("challenges_log.csv", "w", newline="", encoding="utf-8") as f:
writer = csv.writer(f)
writer.writerows(challenges_data)
​print("Files generated successfully.")


```text?code_stdout&code_event_index=1
Files generated successfully.

Seu guia passo a passo em formato Markdown (README.md) e sua planilha de acompanhamento de desafios superados (challenges_log.csv) estão prontos e estruturados para você incluir no seu repositório do GitHub!
​O guia técnico detalha a arquitetura, comandos do ambiente virtual no VPS, tratamento da porta 80 e os payloads JSON para abertura e fechamento de posições.
​A planilha consolida os obstáculos técnicos superados e serve como seu diário de bordo para futuros projetos.
​Aqui estão os arquivos gerados:
​Guia do Projeto (Markdown):
[file-tag: code-generated-file-99180112-79df-4e0d-ab6f-ee6e8246e10f]
​Planilha de Desafios e Soluções (CSV):
[file-tag: code-generated-file-6e397a1b-7024-4cb6-8662-774084b991a6]

Hyperliquid com DEEPSEEK
Lista Atualizada de Etapas (Setup Completo Hyperliquid Testnet + Bot)
Este é o roteiro definitivo, incluindo ações manuais na plataforma, comandos no terminal e orientações para a IA que for auxiliar nas próximas versões. Seguindo esta ordem, você não enfrentará os mesmos erros.

🔹 1. Conta e Carteira
Ação na plataforma:

Tenha uma carteira Ethereum (Rabby, MetaMask) configurada para a testnet da Hyperliquid (chain Arbitrum Sepolia).

Crie/importe uma conta com saldo de gás (ETH de Sepolia) se necessário, mas o faucet da Hyperliquid geralmente fornece USDC diretamente, sem exigir gás.

Nota para a IA: Certifique-se de que o endereço da carteira está acessível e que a chave privada pode ser exportada.

🔹 2. Faucet (Obter USDC de Teste)
Ação na plataforma:

Acesse https://testnet.hyperliquid.xyz com a carteira.

Vá até o faucet oficial (ou use o link da documentação) e solicite USDC para o endereço da carteira.

O saldo aparece em Spot (não em Perp).

Importante: O valor solicitado (ex: 999 USDC) estará disponível apenas na carteira Spot. Ele não pode ser usado diretamente para ordens de futuros (perpétuos).

🔹 3. Criar API Wallet (conta separada para o bot)
Ação na plataforma:

Dentro da interface da Hyperliquid, vá em API Wallets (Settings → API).

Clique em Create API Wallet. Serão exibidos:

Endereço da API Wallet

Chave privada (aparece apenas uma vez – copie e guarde)

Essa chave privada será usada no bot, não a chave da sua carteira principal.

Por quê? A API Wallet é um endereço interno que opera em nome da sua conta principal, mas com permissões limitadas. O saldo precisa ser enviado da conta principal para ela.

🔹 4. Transferir fundos da Conta Principal → API Wallet (Spot)
Ação na plataforma:

Com a conta principal conectada, faça uma Internal Transfer (dentro da rede Hyperliquid) de USDC do Spot da conta principal para o Spot da API Wallet.

Destino: endereço da API Wallet (ex: 0xFf4...).

Quantidade sugerida: pelo menos 100 USDC para testes.

Validação: Na interface da Hyperliquid, alterne para a API Wallet (importando-a com a chave privada, se necessário) e verifique que o saldo Spot aparece.

🔹 5. Transferir USDC do Spot → Perp (Margem) dentro da API Wallet
Ação na plataforma:

Ainda conectado como API Wallet, vá em Portfolio → Transfer.

From: Spot

To: Perpetuals (ou Cross Margin)

Amount: 50 USDC (por exemplo).

Confirme a transação.

Validação: No terminal (com o SDK), execute:

bash
python -c "from exchange_api import ExchangeAPI; api = ExchangeAPI(); print(api.obter_saldo_total())"
Deverá retornar o valor transferido (ex: 50.0). Isso confirma que a margem está habilitada.

🔹 6. Configurar o Servidor (estrutura do bot)
Ações no terminal (ordem exata):

bash


# Criar diretório e venv
mkdir -p ~/bot_trading && cd ~/bot_trading
python3 -m venv venv
source venv/bin/activate

# Instalar dependências essenciais
pip install flask eth-account web3 python-dotenv requests

# Instalar SDK oficial da Hyperliquid
pip install hyperliquid-python-sdk



Arquivos necessários (criar via nano):

.env contendo PRIVATE_KEY=0x... (chave privada da API Wallet).

exchange_api.py (versão final com SDK, já fornecida abaixo).

app.py (Flask + lógica de trading, fornecida abaixo).

bot_gestao.py (gestão de banca, fornecida abaixo).

config/parametros.json e utils/db_manager.py (arquivos de suporte, já fornecidos anteriormente).

Nota para a IA: Esses arquivos já foram testados e aprovados. Para evitar perda de tempo, use as versões mais recentes fornecidas no histórico.

🔹 7. Testar Ordem Manual (via SDK)

bash



cd ~/bot_trading && source venv/bin/activate
python -c "from exchange_api import ExchangeAPI; print(ExchangeAPI().criar_ordem('JUP','short',50.0))"



Regra de ouro: O valor nocional da ordem (preço × quantidade) deve ser ≥ 10 USD. Ajuste a quantidade conforme o preço do ativo.

🔹 8. Subir o Sistema Completo
Três terminais:

Flask: python app.py

Túnel: cloudflared tunnel --url localhost:5000

Gestão: python bot_gestao.py

Teste com curl:

bash


curl -X POST https://<URL_TUNEL>/webhook \
  -H "Content-Type: application/json" \
  -d '{"ativo":"JUP","direcao":"short","entrada":0.20,"stop_loss":0.21,"take_profit":0.19}'


Neste momento, o bot enviará a ordem automaticamente.

🧭 Orientações para a IA (Resumo de Erros Comuns e Soluções)
Nunca pule a transferência Spot → Perp. Ordens de perpétuo exigem saldo na margem.

Use sempre o SDK oficial, não tente assinar EIP-712 manualmente (a estrutura muda com frequência).

A API Wallet é obrigatória para o bot; não use a chave da carteira principal.

Valor mínimo de ordem: 10 USD nocionais. Se o pote calculado pelo gestor for menor que isso, ajuste a faixa de alocação ou defina um floor mínimo.

Importação da API Wallet no Rabby: Se a Hyperliquid não mostrar a API Wallet, adicione a chave privada no Rabby e alterne a conta ativa. O site lê a conta selecionada na carteira.

Endpoints corretos: saldo spot via spotClearinghouseState; saldo margem via clearinghouseState.

Nunca use rm + nano sem antes verificar se o arquivo está atualizado. Mantenha backups.

Para testes rápidos, use python -c "..." em vez de subir Flask.

📦 Containerização (Docker) – Próximo Passo
Assim que o sistema completo estiver operando, posso gerar um Dockerfile e um docker-compose.yml simples, contendo:

O projeto inteiro (~/bot_trading).

🎉 Sucesso total! Ordem executada via IP público
O webhook funcionou perfeitamente: 60 JUP short preenchidos a ~0,19487 USDT.
Seu sistema está completo:

✅ Flask recebendo sinais via webhook

✅ Bot de Gestão monitorando o saldo (250 USDC, 2 bots, pote 37,5)

✅ Ordem enviada com sucesso via SDK da Hyperliquid

✅ IP público acessível (http://191.252.181.191:5000/webhook)

🐳 Container Docker – agora!
Vou gerar os arquivos necessários para empacotar todo o sistema. Você poderá copiar a pasta bot_trading para qualquer servidor e subir tudo com um único comando.

1. Criar o Dockerfile

bash

cd ~/bot_trading
nano Dockerfile

Conteúdo:

dockerfile
FROM python:3.12-slim

WORKDIR /app

# Instalar dependências do sistema

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Copiar requirements e instalar

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

# Copiar o restante do projeto

COPY . .

# Expor a porta 5000
EXPOSE 5000

# Comando padrão: iniciar o Flask
CMD ["python", "app.py"]

2. Criar o requirements.txt

bash

nano requirements.txt
Conteúdo:

text
flask
eth-account
web3
python-dotenv
requests
hyperliquid-python-sdk

3. Criar o docker-compose.yml

bash

nano docker-compose.yml

Conteúdo:

yaml
version: '3.8'

services:
  flask:
    build: .
    container_name: bot_operacional
    ports:
      - "5000:5000"
    env_file:
      - .env
    restart: unless-stopped

  gestao:
    build: .
    container_name: bot_gestao
    command: python bot_gestao.py
    env_file:
      - .env
    restart: unless-stopped
    depends_on:
      - flask

4. Criar o .dockerignore

bash

nano .dockerignore

Conteúdo:

text
venv
__pycache__
*.pyc
db/*.db

🚀 Como usar o container
Construir e subir

bash

cd ~/bot_trading
docker-compose up -d --build

Verificar logs

bash

docker-compose logs -f

Parar tudo

bash

docker-compose down

📋 Resumo do que você tem agora
Sistema funcionando na testnet da Hyperliquid

Webhook acessível via http://191.252.181.191:5000/webhook

Container Docker pronto para ser copiado para qualquer servidor

Estrutura de pastas organizada e documentada

⏭️ Próximos passos (opcionais)
Adicionar stop loss / take profit via SDK (já temos a base, é só complementar)

Substituir o IP público por um domínio com HTTPS (ex: usando Nginx + Let's Encrypt)

Melhorar o contador de bots com verificação de posições reais na API

Ajustar as faixas de capital para a Mainnet

Seu bot está pronto para operar. Parabéns pela persistência! 🎉

O venv (ou recriação das dependências).

Os bots rodando em segundo plano (Flask, gestão, túnel).

Isso permitirá que você copie a imagem para qualquer servidor e execute com um único comando. Podemos fazer isso na próxima iteração.
