# System requirements

## Functional

- Every user can search recipe by attributes like:
    - Search phrase
    - Time of preparation (range)
    - Tags (searchable multiselect)
    - Nutrition
        - calories (range)
        - total fat (range)
        - sugar (range)
        - sodium (range)
        - protein (range)
        - saturated fat (range)
        - carbohydrates (range)
    - Number of steps (range)
    - Number of ingredients (multiselect)
    - Ingredients (multiselect)
- Search result will be paging
- Logged in user have extra features:
    - favourite recipes
    - saved searches
- Integrations:
    - Translator
    - Chat GPT – resume of your search parameters and results
    - Image generation - https://github.com/lucidrains/big-sleep or https://deepai.org/

## Non-functional

- Python or Database
    - for data set operations
- Java
    - call database/Python
    - searching and filtering
    - create API for frontend applications (GraphQL to communication)
    - external translation API calls
- Kotlin
    - Android Frontend Application
- Swift
    - iOS Frontend Application

## Use cases diagram

### Not logged in user

```plantuml
@startuml
left to right direction

actor "Not logged in user" AS user

rectangle Application {
    usecase "Search recipes" as search
    usecase "Filter recipes" as filter
    usecase "View recipe" as view
    
    usecase "Login" as login
    usecase "Register" as register
    
    search <.. filter: <<extend>>
}

user --> search
user --> view
user --> login
user --> register
@enduml
```
![](media/NotLoggedInUserUseCases.png)

### Logged in user

```plantuml
@startuml
left to right direction

actor "Logged in user" AS loggedUser

rectangle Application {
    usecase "Search recipes" as search
    usecase "Filter recipes" as filter
    usecase "View recipe" as view
    
    usecase "Display favourite recipes" as favourite
    usecase "Add recipe to favourites" as addFavourite
    
    usecase "Display saved searches" as savedSearches
    usecase "Save search" as saveSearch
    
    search <.. filter: <<extend>>
    view <.. addFavourite: <<extend>>
    search <.. saveSearch: <<extend>>
}

loggedUser --> search
loggedUser --> view
loggedUser --> favourite
loggedUser --> savedSearches
@enduml
```
![](media/LoggedInUserUseCases.png)