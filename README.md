# Go--TemperaturaPorCEP-OTel

Este repositório contém um sistema desenvolvido em Go que recebe um CEP válido de 8 dígitos, identifica a cidade correspondente e retorna o clima atual em Celsius, Fahrenheit e Kelvin.
O mesmo se trata de dois serviços onde o service_a é responsável pelo input e chama o service_b onde o mesmo é responsável pela orquestração, utilizando a Observabilidade Open Telemetry + Zipkin

Para rodar a aplicação use o docker-compose com o comando abaixo:

```
docker-compose up -d
```

Para acessar a rota do servico, utilize alguem aplicativo para fazer um `POST` no seguinte endereço, com os dados no CEP no body da requisição, como o exemplo a baixo:

```
http://localhost:8080
```
```
body:
{
  "cep": "29709090"
}
```

Link de acesso a telemetria do zipkin":

```
http://localhost:9411/zipkin
```
