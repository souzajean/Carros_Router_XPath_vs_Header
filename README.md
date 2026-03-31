# 🚀 SAP CPI - Router com XPath vs Header (Dynamic Routing)

🔷 Este projeto demonstra como implementar **roteamento dinâmico no SAP Cloud Integration (CPI)** utilizando duas abordagens:

- 🔹 **Header (Non-XML)**
- 🔹 **XPath (XML)**

O objetivo é evidenciar diferenças de performance, cenários de uso e boas práticas dentro de um iFlow real.

📌 Neste cenário, a integração recebe um XML com dados de veículos e decide a rota com base no ID ou no status do carro.

![Fluxo](imagens/capa-linkedin.png)

---

🖼️ Visão Geral do Fluxo

![Fluxo](imagens/1.png)

<br>

# :building_construction: Arquitetura do iFlow

# 🔄 Fluxo da Integração

# Entrada no Postman

1. 📥 Entrada (Postman)
Endpoint:
```
POST /carros
```
Payload de exemplo:
```
<carros>
    <carro>
        <ID>1</ID>
        <marca>Fiat</marca>
        <modelo>Argo</modelo>
        <ano>2021</ano>
        <status>Reservado</status>
    </carro>   
</carros>
```
<br>

## 📦 2. Criação do Pacote 

### Criando o Package
![Fluxo](imagens/Screenshot_1.png)

<br>

### Nome do Package
![Fluxo](imagens/Screenshot_2.png)
```
ZPKG_CPI_ROUTING_SCENARIOS
```
<br>

## 🧩 3. Criação do Integration Flow

### Adicionando o Artefato
![Fluxo](imagens/Screenshot_3.png)

<br>

### Nome do iFlow
![Fluxo](imagens/Screenshot_4.png)
```
IF_Carros_Router_XPath_vs_Header
```
<br>

## ⚙️ 4. Configuração Inicial do iFlow

### Editando o iFlow

![Fluxo](imagens/Screenshot_5.png)

<br>

## 🌐 5. Configuração do Adapter HTTPS

### Adicionando o Adapter
![Fluxo](imagens/Screenshot_6.png)

<br>

### Endpoint configurado
![Fluxo](imagens/Screenshot_7.png)
```
/carros
```

<br>

## 🧩 6. Content Modifier (Header Enrichment)

## Adicionando o Content Modifier
![Fluxo](imagens/Screenshot_8.png)

<br>

## Renomeando o componente
![Fluxo](imagens/Screenshot_9.png)
```
Name: CM_setHeader
```
## 📌 Extração de dados do XML para Header

Neste passo, extraímos informações do payload XML e transformamos em headers, permitindo roteamento sem necessidade de parsing XML no Router.

### Configuração:
![Fluxo](imagens/Screenshot_10.png)

```
Message Header
Create -	Status	-	XPath	-	/carros/carro/status	-	java.lang.String
Create -	ID	   -	XPath	-   /carros/carro/ID	    -	java.lang.String
```
💡 Vantagem:
Permite utilizar roteamento mais performático (Non-XML).

<br>

## 🔀 7. Router (Decisão de Rota)

4. 🔀 Router (Decisão de Rota)

### Adicionando o Router
![Fluxo](imagens/Screenshot_11.png)






































































<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>











🟢 Rota 1 — Baseada em Header (Non-XML)

Condição:

${header.ID} = '1'

✔ Mais performática <br>
✔ Ideal para integrações complexas <br>
✔ Evita parsing XML repetido <br>

🔵 Rota 2 — Baseada em XPath (XML)

Condição:

/carros/carro/status = 'Disponível'

✔ Direto no payload <br>
✔ Útil quando não há header <br>

⚠ Mais custosa (parse XML) <br>

⚪ Rota Default <br>

Caso nenhuma condição seja atendida.

5. 📤 Resposta por Rota

Cada rota retorna um XML diferente:

🟢 Rota ID <br>
```
<resultado>
    <rota>ROTA Non-XML</rota>
    <tipo>Carro ID 1</tipo>
    <mensagem>Carro 1 está Reservado</mensagem>
</resultado>
```

<br> <br>

🔵 Rota Status <br>
```
<resultado>
    <rota>ROTA XML</rota>
    <tipo>Carro ID 1</tipo>
    <mensagem>Carro 1 está Disponível</mensagem>
</resultado>
```

<br>

⚪ Default
```
<resultado>
    <tipo>Outros</tipo>
    <mensagem>Rota padrão</mensagem>
</resultado>
```

<br>

2. 🧩 Content Modifier (CM_setHeader)

Extrai dados do XML e transforma em headers:

ID → /carros/carro/ID
Status → /carros/carro/status

👉 Isso permite usar lógica Non-XML no Router

3. 🔀 Router (Decisão de Rota)

O roteamento possui 3 caminhos:

🟢 Rota 1 — Baseada em Header (Non-XML)

Condição:

${header.ID} = '1'

✔ Mais performática <br>
✔ Ideal para integrações complexas <br>
✔ Evita parsing XML repetido <br>

🔵 Rota 2 — Baseada em XPath (XML) <br>

Condição:

/carros/carro/status = 'Disponível'

✔ Direto no payload <br>
✔ Útil quando não há header <br>

⚠ Mais custosa (parse XML)

⚪ Rota Default

Caso nenhuma condição seja atendida.

4. 📤 Resposta por Rota

Cada rota retorna um XML diferente:

🟢 Rota ID
```
<resultado>
    <rota>ROTA Non-XML</rota>
    <tipo>Carro ID 1</tipo>
    <mensagem>Carro 1 está Reservado</mensagem>
</resultado>
```
🔵 Rota Status
```
<resultado>
    <rota>ROTA XML</rota>
    <tipo>Carro ID 1</tipo>
    <mensagem>Carro 1 está Disponível</mensagem>
</resultado>
```
⚪ Default
```
<resultado>
    <tipo>Outros</tipo>
    <mensagem>Rota padrão</mensagem>
</resultado>
```


---

## 📦 Exemplo prático – iFlow para baixar

📦 [Download do iFlow – Carros_Router_XPath_vs_Header](https://github.com/souzajean/Carros_Router_XPath_vs_Header/raw/main/Package/IF_Carros_Router_XPath_vs_Header.zip)
