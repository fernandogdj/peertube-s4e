# PeerTube Local com Docker

Sobe o [PeerTube](https://github.com/chocobozzz/peertube) localmente no Windows usando Docker Compose. Sem complicação — três containers, um comando, e a plataforma está rodando no navegador.

---

## O que você precisa ter instalado

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) — precisa estar rodando antes de qualquer coisa
- Git (opcional, só se quiser clonar em vez de baixar o zip)

> Se o Docker Desktop não estiver aberto, nada vai funcionar. Começa por aí.

---

## Como rodar

### 1. Baixe o projeto

```powershell
git clone https://github.com/fernandogdj/peertube-s4e.git
cd peertube-s4e
```

Ou baixe o ZIP pelo GitHub e extraia numa pasta de sua preferência.

---

### 2. Adicione o domínio local ao hosts do Windows

O PeerTube precisa de um hostname para funcionar. Vamos usar `peertube.localhost`.

Abra o **Bloco de Notas como Administrador** e edite:

```
C:\Windows\System32\drivers\etc\hosts
```

Adicione essa linha no final:

```
127.0.0.1   peertube.localhost
```

Prefere pelo PowerShell (como Administrador)?

```powershell
Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "127.0.0.1 peertube.localhost"
```

---

### 3. Suba os containers

```powershell
docker compose up -d
```

Na primeira vez vai demorar um pouco — ele precisa baixar as imagens. Depois disso é rápido.

Para acompanhar o que está acontecendo:

```powershell
docker compose logs -f
```

---

### 4. Pegue a senha do admin

Aguarde uns 2 minutos para o PeerTube inicializar completamente, depois rode:

```powershell
docker compose logs peertube | Select-String -Pattern "root"
```

Vai aparecer o usuário `root` e a senha gerada automaticamente. **Anota essa senha**, ela só aparece uma vez.

---

### 5. Acesse

Abra o navegador em:

```
http://peertube.localhost:9000
```

Login com `root` e a senha que você anotou. Pronto.

---

## Comandos do dia a dia

```powershell
# Parar tudo
docker compose down

# Parar e apagar todos os dados (volumes)
docker compose down -v

# Ver status dos containers
docker compose ps

# Atualizar para versão mais nova
docker compose pull && docker compose up -d
```

---

## Problemas comuns

**Porta 9000 ocupada**
Edite o `docker-compose.yml` e troque `"9000:9000"` por `"9001:9000"`. Acesse na porta 9001.

**Porta 1935 ocupada**
Comente ou remova a linha `- "1935:1935"` no `docker-compose.yml`. Isso só afeta live streaming.

**Senha não aparece nos logs**
Tente com o `findstr` no lugar do `Select-String`:
```powershell
docker compose logs peertube 2>&1 | findstr root
```

**Container do PeerTube fica reiniciando**
Veja o que está acontecendo com:
```powershell
docker compose logs peertube
```
O erro vai estar lá.

---

## Estrutura dos arquivos

```
peertube-s4e/
├── docker-compose.yml   # PeerTube + PostgreSQL + Redis
├── .env                 # Configurações e variáveis de ambiente
├── README.md
└── docker-volume/       # Criado automaticamente, aqui ficam os dados
    ├── config/
    ├── data/
    ├── db/
    └── redis/
```

---

## Observações

Esta configuração é para rodar localmente. Para um ambiente de produção seria necessário HTTPS, um nginx na frente e um domínio real.

Os dados ficam na pasta `docker-volume/` e persistem entre reinicializações. Só são apagados se você rodar `docker compose down -v`.
