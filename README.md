# 🌡️ Temperatura por CEP — Go + OpenTelemetry

Dois microsserviços em **Go** que recebem um CEP, descobrem a cidade e retornam a temperatura atual em **Celsius, Fahrenheit e Kelvin**, com observabilidade via **OpenTelemetry** e visualização dos traces no **Zipkin**.

![Go](https://img.shields.io/badge/Go-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-000000?style=for-the-badge&logo=opentelemetry&logoColor=white)
![Zipkin](https://img.shields.io/badge/Zipkin-FE7139?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

## 🏗️ Arquitetura

```
 Cliente ── POST / {"cep"} ──► service_a :8080 ── GET /{cep} ──► service_b :8081 ──► ViaCEP (cidade)
                                (valida o CEP)                   (orquestra)      └──► WeatherAPI (clima)
                                     │                                │
                                     └──────── spans ─────► Zipkin :9411 ◄──┘
```

| Serviço | Responsabilidade | Spans |
|---|---|---|
| `service_a` | Recebe o input, valida o CEP (8 dígitos) e chama o `service_b` | `validate-cep`, `request-service-b` |
| `service_b` | Busca a cidade no ViaCEP, consulta o clima e converte as temperaturas | `get-cep-temperature`, `get-cep-location`, `get-weather` |

## 🚀 Como rodar

Crie uma chave gratuita em [weatherapi.com](https://www.weatherapi.com/) e exporte antes de subir:

```bash
export WEATHER_API_KEY=sua_chave
docker compose up -d
```

## 📬 Exemplo

```bash
curl -X POST http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{"cep": "29709090"}'
```

**Resposta `200`**

```json
{ "city": "Colatina", "temp_C": 28.5, "temp_F": 83.3, "temp_K": 301.5 }
```

| Situação | Status | Mensagem |
|---|---|---|
| CEP com formato inválido | `422` | `invalid zipcode` |
| CEP não encontrado | `404` | `can not find zipcode` |

## 🔎 Traces

Abra o Zipkin em **http://localhost:9411/zipkin**, clique em **Run Query** e veja os spans de cada serviço com o tempo gasto em cada etapa (validação, ViaCEP e WeatherAPI).

---

Desenvolvido por **Yuri Bertoldi** como desafio da pós-graduação em Go da Full Cycle — [LinkedIn](https://www.linkedin.com/in/yuri-bulh%C3%B5es-bertoldi-b62459180/)
