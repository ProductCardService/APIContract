# Сервис продуктовых карточек

## Назначение репозитория
Данный репозиторий содержит следующую информацию:
- текущая архитектура сервиса в целом
- соглашение о контракте взаимодействия (API)
- схема развертывания Docker Compose

## Архитектура сервиса (C4 Containers)
```mermaid
graph LR
    %% C4 style classes c4model.com %%
    classDef person fill:#08427b,stroke:black,color:white;
    classDef container fill:#1168bd,stroke:black,color:white;
    classDef database fill:#1168bd,stroke:black,color:white;
    classDef software fill:#1168bd,stroke:black,color:white;
    classDef existing fill:#999999,stroke:black,color:white;
    classDef boundary fill:white,stroke:black,stroke-width:2px,stroke-dasharray: 5 5;
    classDef frame fill:white,stroke:black;


    %% nodes %%
    Person((Manager)):::person
    WebApp("Web Application<br>[Container: React]"):::container
    WebServer("Web Server<br>[Container: nginx]"):::container
    CardService("Card Service<br>[Container: Java]"):::container
    GenerationService("Generation Service<br>[Container: Python]"):::container
    CardsDB[("Cards<br>[Container: PostgreSQL]")]:::database
    ImageGenerationService["Kandinsky"]:::existing
    TextGenerationService["GigaChat"]:::existing

    %% connections and boundaries %%
    
    subgraph Legend [Containers]
        Person-.->|Uses| WebApp
        subgraph Boundary["Boundary: System under development"]
            WebApp-.->|"Makes requests <br> [HTTP/HTTPS]"| WebServer
            WebServer-.->|"Forwards requests <br> [HTTP/HTTPS]"| CardService
            WebServer-.->|"Forwards requests <br> [HTTP/HTTPS]"|GenerationService
            CardService-.->|"Sends SQL"|CardsDB
        end
        class Boundary boundary
        GenerationService-.->|"Makes requests <br> [HTTP/HTTPS]"| ImageGenerationService
        GenerationService-.->|"Makes requests <br> [HTTP/HTTPS]"| TextGenerationService
    end
    class Legend frame

```
## Контракт взаимодействия между фронтендом и бэкендом
В контракте используются следующие теги:
- card - соответствует API для сервиса карточек
- ai - соответствует API для сервиса генерации контента

Подробнее о контракте можно почитать в файле api.yaml или посмотреть в Swagger по 
[ссылке](https://kortkamp.github.io/swagger-viewer/?host=https%3A%2F%2Fraw.githubusercontent.com%2FProductCardService%2FProductCardService%2Fmaster%2Fapi.yaml)

## Демонстрация приложения
![image](https://github.com/ProductCardService/ProductCardService/assets/80649413/ff72cf48-fbb8-4bee-8e10-78833664c7d8)
![image](https://github.com/ProductCardService/ProductCardService/assets/80649413/bb2a8f16-6edc-4fca-9b02-62c4078a33b0)


## Команды для запуска
Собрать все Docker Image
```bash
docker build -t product-card-service/web:latest .
docker build -t product-card-service/card-service:latest .
docker build -t product-card-service/ai-service:latest .
```
Запустить всё одной командой
```bash
docker compose up
```

