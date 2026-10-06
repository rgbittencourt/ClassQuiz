<!--
SPDX-FileCopyrightText: 2026 Rogério Bittencourt

SPDX-License-Identifier: MPL-2.0
-->

# Implantação do ClassQuiz em português do Brasil

Esta versão inicia em português do Brasil (`pt-BR`) e usa a tradução brasileira como idioma de contingência.

## Hospedagem recomendada

O ClassQuiz completo não é um site estático. Ele usa SvelteKit com renderização no servidor, FastAPI, WebSockets, PostgreSQL, Valkey/Redis, Meilisearch e um processo de trabalho em segundo plano. Por isso:

- GitHub Pages não executa a aplicação completa;
- Cloudflare Pages isoladamente não executa esta pilha;
- ChatGPT Sites exigiria reescrever o backend e a camada de dados.

A implantação fiel usa uma máquina virtual ou servidor com Docker Compose. A Cloudflare pode ficar na frente do servidor, fornecendo DNS, proxy, HTTPS, CDN e proteção.

## Pré-requisitos

- um servidor Linux com Docker e Docker Compose;
- um domínio adicionado à Cloudflare;
- portas HTTP/HTTPS acessíveis ou um Cloudflare Tunnel;
- ao menos 2 GB de memória (4 GB recomendados para maior folga).

## Configuração

1. Troque `TOP_SECRET` em `docker-compose.yml` por uma chave aleatória forte:

   ```bash
   openssl rand -hex 32
   ```

2. Ajuste `ROOT_ADDRESS` para o endereço público, sem barra no final.
3. Configure e-mail, captcha e login social apenas se forem utilizados.
4. Para produção, troque as senhas padrão do PostgreSQL e mantenha os segredos fora do Git.
5. Inicie os serviços:

   ```bash
   docker compose up -d --build
   ```

6. Aponte o proxy ou o Cloudflare Tunnel para `http://localhost:8000`.

## Cloudflare Tunnel

No painel Zero Trust da Cloudflare, crie um túnel e associe o hostname desejado ao serviço HTTP `http://localhost:8000`. Instale o conector `cloudflared` no servidor seguindo o comando gerado pelo painel. O token do túnel é secreto e nunca deve ser salvo neste repositório.

## Licença e atribuição

O projeto original é distribuído sob a Mozilla Public License 2.0. Preserve os avisos de copyright, os arquivos de licença e disponibilize publicamente as modificações feitas nos arquivos cobertos pela MPL-2.0.
