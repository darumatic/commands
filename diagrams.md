#Sequence diagram

Cool an easy sequence diagram with PlantUML 
```
@startuml
skinparam handwritten true
User -> Browser : Get application
Browser -> Kubernetes_Ingress : HTTP Get
Kubernetes_Ingress -> Kubernetes_Service : HTTP Get
note right: Ingress configuration is loaded from the Ingress resources
Kubernetes_Service -> Kubernetes_Pod : HTTP Get
note right: Kubernetes Service does a round robin towards the multiple pods
Kubernetes_Pod -> Browser : HTTP Get response
Browser -> Browser : Render Information
@enduml
```
Online generation:
http://www.plantuml.com/plantuml/uml/TP71JiCm38RlUGg_02-m0vgumCHbWd56MUEs5gaTsGvzVTAXibKQ5ykE_rzVz7jl7jNhb8Dz4PUeSO8nSfgLt971jMBuC5HuU8GdbNbfT_2C3h6KJ5rq4WxhfebUwncLqT4-3pmecZNzo-bqN4pXpdRhUKVzfWvlFIoEe3ICleFLr0dtD5Izu84CiiI42NgL2Np4Fs4hKhL32tSWM_HHqd_OgmfMRv8sH52fd5ezKmjBvgZ9BlofPgUI4Oga3NkXRB9SWSjNYx3XRPfNsHjoQis1Uz7fD_LzUsgd-m40
