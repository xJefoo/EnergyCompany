# EnergyCompany
openapi: 3.0.3
info:
  title: Sensedia Energy API
  description: API para gestão de faturas e serviços de energia elétrica
  version: 1.0.0
tags:
  - name: Regra de Negócio
    externalDocs:
      description: Documentação
      url: https://docs.google.com/document/d/1BKY7FFuiM0_929unEMPbzIEqIr6nMPjOJkxZU4yXBGg/edit?usp=sharing
  - name: Microserviços
    description: Java e funções SAP
    externalDocs:
      description: Fluxo Webservice
      url: https://mermaid.live/view#pako:eNqdVV1P2zAU_StXeSlIRSoTT0XalDaUdYNSKOOrRcgkbrGW2JHtVGyEH7PtYdrzfkL_2G7stA5dWyTeGveec88998R58kIRUa_pTSRJH-A82B_xEVfZvX0eeZ04exRNaEtGJPTElECH6EySkTfiAP6wHTPKNb2FnZ33-UDELGSazH7PfglIiSQQGiAvgGMDzKE19Pvd2_0C3zKwPpGznwnVUiiYzn7ELBLqQw7trS3Y3i7K2padJTkEBdiwVkktW2DKfC3ZfcYgS4BFd_OmB4asUhbQMeMUlMYCBaFIBNQK2ojUcugsV59RLSTHcWZ_IzYREFGQVKUC4VB719hFzKHDHBrMMf6LDixEwKKba_TRgeyQPXQuh-5wY8O9RqNmR-5WOyWUKzKhSVFMpcSByu3AGLloiL7C7A8wXpqM_T_N-1Merd78yb2mEgKqSfxAFUQvA_D51QAIg48q-PlKjlwOjgy6U9r0yJSmGIBjZ86xS0DPJECW_vxPbBl7K1dQhymJhawjTOEOQiJwKVwk9C60Y-RwstzUbqTvjvvmuBvMs4pKT19ost7vNfZgpzQLeOEH5aHgqAc3b0Webt7eGmwOZ8tirMbBShUNVIFiF1t3YShVDDarWIPN4fzV7PSlCKlSeAH0yYQgtTZBrgboy6sBShckaZUkS1ySLlySLlYm6dIZdumSdDV0saYV9sUdZCmvHOLaODxXVIHg_QEqK06FBV3bu6iyj6qzuMsxk4kdM4cbJ-9m3TKWIL7vMFeVDPitlSHYxRC4HZhEkQxL2HeyyIHf2hyE9Xhs2152uJQTvO3N8IM3vhr-QSWVXt1LKHrGIvzCPRW8I08_0ISOvCb-jIj8WqTwGeuKaQbfeOg1tcxo3cvSiGgaMIKRTuaHKeE3QuDjmMSKPv8Dclp3YQ
  - name: Banco de Dados
    description: Queries e funções SQL
    externalDocs:
      description: Fluxo SQL
      url: https://mermaid.live/view#pako:eNptkU1OwzAQha9ied1KLLpKJaRASBtUidKwIu5iEg_UwrEj20FFbc_DQbgYk_QvSOxmrO-9N3re8cpK5BF_d9Bs2EsyFUYY35bHXfBUt1sbsacyoGML5QMwiWzprGyD9YILw1icFvdaoQm4ZuPx7T63WlWKSH3mmxO_Z_GsiJfZetrrZj3-sMWqJa6yFKxpyJ8XBM6LOzCV7eQJSOtPmnmvWZGkQQcXZ4bMo_tUP9_HlGHACoN15v9z0qFrjvXVkAQleOx42eUTnA1uz_5Yo3OWTW4mBD1eyiAQjeQjXqOrQUmqeddpBQ8brFHwiEYJ7qOr8UAcUHD-ZSoeBdfiiLeNhICJAvqN-vzYgHm1ltY30B4Pv4fFmGQ

paths:
  /api/faturas:
    post:
      tags:
      - Microserviços
      summary: Criar uma nova fatura
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                cliente_id:
                  type: number
                valor:
                  type: number
                descricao:
                  type: string
              required:
                - cliente_id
                - valor
      responses:
        '201':
          description: Fatura criada com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  id_fatura:
                    type: string
                  status:
                    type: string
                    enum: [criada, paga, cancelada]
              example:
                id_fatura: "12345"
                status: "criada"
        '400':
          description: Cliente inválido fornecido
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
              example:
                message: "Cliente inválido fornecido"

  /api/faturas/{numero_fatura}:
    get:
      tags:
      - Microserviços
      summary: Obter detalhes de uma fatura
      parameters:
        - name: numero_fatura
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Detalhes da fatura recuperados com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  id_fatura:
                    type: string
                  valor:
                    type: number
                  descricao:
                    type: string
                  nome_cliente:
                    type: string
              example:
                id_fatura: "12345"
                valor: 100.0
                descricao: "Serviços de energia"
                nome_cliente: "Cliente Exemplo"
        '400':
          description: ID inválido fornecido
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
              example:
                message: "ID inválido fornecido"
        '404':
          description: Fatura não encontrada
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
              example:
                message: "Fatura não encontrada"

  /api/faturas/{numero_fatura}/pagamento:
    post:
      tags:
      - Microserviços
      summary: Processar pagamento de uma fatura
      parameters:
        - name: numero_fatura
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                forma_pagamento:
                  type: string
                numero_cartao:
                  type: string
                valor:
                  type: number
              required:
                - forma_pagamento
                - numero_cartao
                - valor
      responses:
        '200':
          description: Pagamento processado com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
              example:
                message: "Pagamento processado com sucesso"
        '401':
          description: Pagamento não autorizado
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
              example:
                message: "Pagamento não autorizado"

  /api/produtos/:
    get:
      tags:
      - Banco de Dados
      summary: Retorna os produtos existentes na base
      responses:
        '200':
          description: Lista de produtos e serviços recuperada com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  products:
                    type: array
                    items:
                      type: object
                      properties:
                        product_id:
                          type: string
                        name:
                          type: string
              example:
                products:
                  - product_id: "1"
                    name: "Energia Elétrica"
                  - product_id: "2"
                    name: "Manutenção de Equipamentos"
        '404':
          description: Produto não encontrado
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
              example:
                message: "Produto não encontrado"
![image](https://github.com/xJefoo/EnergyCompany/assets/81267408/758c386f-5c0f-470e-879f-a516bf963659)
