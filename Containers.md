# Containers diagram

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

!define DEVICONS2 https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master/devicons2
!define FONTAWESOME https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master/font-awesome-5
!define ICONURL https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/v2.4.0
!include media/rabbitmq.puml
!include DEVICONS2/spring.puml
!include DEVICONS2/mysql.puml
!include DEVICONS2/kotlin.puml
!include DEVICONS2/swift.puml
!include DEVICONS2/python.puml
!include DEVICONS2/redis.puml
!include FONTAWESOME/users.puml
!includeurl ICONURL/font-awesome/server.puml

LAYOUT_WITH_LEGEND()

Person(user, "Users", "System users", $sprite="users")

System_Boundary(c1, "Application") {
    Container(android, "Android", "android", "Android", $sprite="kotlin")
    Container(iOS, "iOS", "ios", "iOS", $sprite="swift")
    Container(backend, "Backend", "Java + Spring", "Business logic", $sprite="spring")
    ContainerDb(db, "Database", "SQL", "Data storing", $sprite="mysql")
    Container(recomendationApi, "Recomendation module", "Python + Flask", "Recomendation module", $sprite="python")
    Container(rabbitmq, "RabbitMQ", "RabbitMQ", "Message broker", $sprite="rabbit")
    Container(redis, "Redis", "Redis", "Cache", $sprite="redis")
}

Container_Ext(imageGenerationApi, "Image generation API", $sprite="server")
Container_Ext(chatGptApi, "Chat GPT API", $sprite="server")

Rel(user, android, "Uses", "https")
Rel(user, iOS, "Uses", "https")

Rel(android, backend, "API calls", "graphql")
Rel(iOS, backend, "API calls", "graphql")

Rel_D(db, backend, "Reads")
Rel_D(backend, db, "Writes")

Rel_R(backend, recomendationApi, "API calls", "REST")

Rel_D(backend, rabbitmq, "Produce")
Rel_D(rabbitmq, backend, "Consume")

Rel_D(redis, backend, "Reads")
Rel_D(backend, redis, "Writes")

Rel_U(backend, imageGenerationApi, "API calls", "REST")
Rel_L(backend, chatGptApi, "API calls", "REST")

@enduml
```

![](media/containerDiagram.png)
