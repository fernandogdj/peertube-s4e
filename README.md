# PeerTube Local com Docker

Sobe o [PeerTube](https://github.com/chocobozzz/peertube) localmente no Windows usando Docker Compose. Sem complicação — três containers (postgres, redis e peertube), um comando, e a plataforma está rodando no navegador.

---

## O que você precisa ter instalado

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) — precisa estar rodando antes de qualquer coisa
- Git (opcional, só se quiser clonar em vez de baixar o zip)

> Se o Docker Desktop não estiver aberto, nada vai funcionar. Começa por aí, tive este problema.

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
---

### 3. Suba os containers

```powershell
docker compose up -d
```

Para acompanhar o que está acontecendo:

```powershell
docker compose logs -f
```

---

### 3. Acesse

Abra o navegador em:

```
http://peertube.localhost:9000
```

Login é automatico. Pronto.

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

**Container do PeerTube fica reiniciando**
Veja o que está acontecendo com:
```powershell
docker compose logs peertube
```
O erro vai estar lá.


## Observações


Os dados ficam na pasta `docker-volume/` e persistem entre reinicializações. Só são apagados se você rodar `docker compose down -v`.
