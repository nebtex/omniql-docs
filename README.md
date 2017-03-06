<img src="omni.png" />
---

omniql is opinionated way to create schema and defied commons operation over them, is intended to be useful  for large project where tems of diferent background converge,
omniql schemas  can be used in any devices, for internal  or externals application, collaborate and create api, api versioning and export them to the world


Key aspects:
 - it uses cqrs (Command and Query Responsibility Segregation) if you don't know what is this please read https://martinfowler.com/bliki/CQRS.html
 - all the communication is planned to be  over nats or kafka or similar software (centrifuge for the frontend)
 - it should try to use the open api 3 spec https://github.com/OAI/OpenAPI-Specification/blob/3.0.0-rc0/versions/3.0.md, like a guide when is possible
 - also we should use the google api design spec https://cloud.google.com/apis/design/
 - large inspired by kubernetes and swagger 

what omniql should offer:
 - creation of schemas in a nice interface (api and ui)
 - auto-generation of code for different programming languages and provide the tooling to do this
 - auto-generation of entry docker containers that can handle well known patterns like crud, subscriptions, permissions

plan:
 - do an initial version, working
 - build omniql on omniql
 
oriented to:
 - large organization that has many team of different backgrounds
 - microservice infrastructures
 - iot
 - rapid development thanks to the auto component creation (this mean that omniql can create entery or partial application, with all the bussiness logic in it), manage github, repos and creation of docker containers 

Why is this project needed?

is still difficult to create good schema, maintain track of them,  share between the team  with the current tech,
if we have a good tool for create schemas, this will decrease the work needed to get things done, 
in organization that worry about infrastructure evolution. more easy work, more people happy


similar software:

- apache thirft
- swagger
- grpc


# spec 

this is the spec for the version V1Alpha, it will change almost every time possible, nothing that is written here is the last word

## Resources 

#### V1Alpha:

things that should be ready for the first alpha release

 - [V1Alpha.Application](V1Alpha/Application.md)




#### V1Next

things that would be  nice to have 

 - [V1Alpha.Branch](V1Next/branch.md)
 - [V1Alpha.Version](V1Alpha/Version.md)
 - [V1Alpha.Pipelines](V1Alpha/Pipeline.md) [planning]
 - [V1Alpha.PipelineBranch](V1Alpha/PipelineBranch.md) [planning]
 - [V1Alpha.Subscription](V1Alpha/Subscription.md) [planning]
 - [V1Alpha.Environment](V1Alpha/Environment.md)
 - [V1Alpha.Command](V1Alpha/Command.md)
 - [V1Alpha.Query](V1Alpha/Query.md)
 - [V1Alpha.Component](V1Alpha/Component.md)

### Omniql APp
   Omniql application on top of omniql definition that allow to manage resources by federation, orgs , team and users

 - [V1Alpha.Federation](V1Alpha/Federation.md)
 - [V1Alpha.Organization](V1Alpha/Organization.md)
 - [V1Alpha.Team](V1Alpha/Team.md)
 - [V1Alpha.Users](V1Alpha/Users.md)

## Code generation










{% mermaid %}
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
{% endmermaid %}