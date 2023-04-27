# Components
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

!define DEVICONS2 https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master/devicons2
!define FONTAWESOME https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master/font-awesome-5
!include DEVICONS2/spring.puml
!include DEVICONS2/flask_original.puml
!include DEVICONS2/python.puml

LAYOUT_WITH_LEGEND()

Container_Boundary(c2, Backend) {
    Component_Ext(be,"backend", "spring", "Backend", "spring")
}
Container_Boundary(c3, Recommendation module) {
    Component(aibecommunication,"Communication with BE", "flask", "Recommendation module module to backend communication", "flask_original")
    Component(ai,"Recommendation module module", "python", "Recommendation module module", "python")
}
Container_Boundary(c4, Database) {
    Component_Ext(db,"Database", "MySQL", "Database", $sprite="mysql")
}
Rel(be, aibecommunication, "API Calls")
Rel(aibecommunication, ai, "Uses")
Rel(aibecommunication,be, "API Calls")
Rel(ai, aibecommunication, "Uses")
Rel(ai, db, "SQL")


@enduml
```