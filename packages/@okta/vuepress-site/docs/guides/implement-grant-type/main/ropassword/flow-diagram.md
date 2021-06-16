#### Resource Owner Password flow

![Resource Owner Password flow](/img/oauth_password_flow.png "Sequence diagram that shows the back and forth between the resource owner, authorization server, and resource server for Resource Owner Password flow")

<!-- Source for image. Generated using http://www.plantuml.com/plantuml/uml/

skinparam monochrome true

actor "Resource Owner (User)" as user
participant "Client" as client
participant "Authorization Server (Okta)" as okta
participant "Resource Server (Your App)" as app

user -> client: Authenticates
client -> okta: Access token request to /token
okta -> client: Access token (+optional Refresh Token) response
client -> app: Request with access token
app -> client: Response

-->