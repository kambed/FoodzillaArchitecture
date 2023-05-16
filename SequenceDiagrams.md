# Sequence diagrams
## Frontend calls backend
```plantuml
@startuml
actor User
participant Frontend
participant Backend

User -> Frontend: Do action
activate Frontend
Frontend -> Backend: Call to /graphql
activate Backend
opt Call to Database
    ref over Backend
        Database: Call to Database
    end
end
opt Call to Redis
    ref over Backend
        Redis: Call to Redis
    end
end
opt Call to RabbitMQ
    ref over RabbitMQ
        RabbitMQ: Produce
    end
end
opt Call to Chat GPT API
    ref over Backend
        Chat GPT API: Call to Chat GPT API
    end
end
opt Call to Image generation API
    ref over Backend
        Image generation API: Call to Image generation API
    end
end
opt Call to Recommendation service
    ref over Backend
        Recommendation service: Call to Recommendation service
    end
end
return Response
return Display data due to response
@enduml
```
![](media/sequence/frontendBackendFlow.png)
## Backend calls Chat GPT API

```plantuml
@startuml
participant AnyBackendClass << (C,#ADD1B2) >>
participant CompletionsAdapter  << (C,#ADD1B2) >>
participant "Chat GPT API" as chatGpt

AnyBackendClass -> CompletionsAdapter: String
activate CompletionsAdapter
CompletionsAdapter -> chatGpt: /v1/chat/completions with string
activate chatGpt
return Response
return String Response
@enduml
```
![](media/sequence/backendGptApi.png)
## Backend calls Image generation API

```plantuml
@startuml
participant AnyBackendClass << (C,#ADD1B2) >>
participant "RabbitMQ" as rabbitmq
participant RecipeImageConsumer  << (C,#ADD1B2) >>
participant "Database" as db
participant "ImageGeneration API" as imageGenerationApi

activate AnyBackendClass
activate rabbitmq
AnyBackendClass -> rabbitmq: Message
rabbitmq -> RecipeImageConsumer: Message
activate RecipeImageConsumer
RecipeImageConsumer -> imageGenerationApi: API call
activate imageGenerationApi
return Image
RecipeImageConsumer -> db: Save image
activate db
return Image
return Image
rabbitmq -> AnyBackendClass: Image


@enduml
```
## Backend calls Database
```plantuml
@startuml
participant Backend
participant Redis
participant Hibernate
participant Database

alt Request for recipe
    Backend -> Redis: Request for recipe
    activate Redis
    return Response
    opt Recipe not found
        Backend -> Hibernate: Request for data
        activate Hibernate
        Hibernate -> Database: SQL query
        activate Database
        return Response
        return Data
    end
else Request for another data
    Backend -> Hibernate: Request for data
    activate Hibernate
    Hibernate -> Database: SQL query
    activate Database
    return Response
    return Data
end
@enduml
```
![](media/sequence/backendDatabase.png)
## Backend calls Recommendation service
### Async request for recommendations
```plantuml
@startuml
participant Frontend as fe
participant Backend as be
participant "Recommendation service" as recommend
participant Database as db

fe -> be: Register/Login/Add review
activate be
be --> fe: Response
be -> recommend: Request for recommendation
activate recommend
recommend -> db: SQL query
activate db
return Response
return Data
be -> db: Save recommendation
activate db
return Response
@enduml
```
![](media/sequence/backendAsyncRecommendation.png)
### Get recommendations
```plantuml
@startuml
participant Frontend as fe
participant Backend as be
participant Redis as redis

fe -> be: Request for recommendations
activate be
be -> redis: SQL query
activate redis
return Response
return Data
@enduml
```
![](media/sequence/backendReadRecommendationsFromDatabase.png)