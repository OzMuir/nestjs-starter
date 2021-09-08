# NestJS Starter

Opinionated NestJS MVC boilerplate for rapid development with battle-tested standards.

[Use this template](https://github.com/thisismydesign/nestjs-starter/generate)

## Stack

It has
- Example REST and GraphQL modules, DB using TypeORM as seen on https://docs.nestjs.com/
- [Next.js](https://nextjs.org/) integration for React on the frontend ([howto article](https://csaba-apagyi.medium.com/nestjs-react-next-js-in-one-mvc-repo-for-rapid-prototyping-faed42a194ca))
- Typed queries & results with GraphQL out of the box ([howto article](https://csaba-apagyi.medium.com/automagically-typed-graphql-queries-and-results-with-apollo-3731bad989aa))
- Authentication via [Passport.js](http://www.passportjs.org/) including Social providers ([howto article](https://medium.com/csaba.apagyi/oauth2-in-nestjs-for-social-login-google-facebook-twitter-etc-8b405d570fd2)), [AWS Cognito](https://aws.amazon.com/cognito/) ([howto article](https://medium.com/csaba.apagyi/cognito-via-oauth2-in-nestjs-outsourcing-authentication-without-vendor-lock-in-ce908518f547)), and JWT strategy for REST and GraphQL
- Docker setup
- Typescript, ESLint
- CI via GitHub Actions
- Running tasks (e.g. DB seeding) via [nestjs-console](https://github.com/Pop-Code/nestjs-console)
- Unit and integration testing via Jest
- Heroku deployment setup

## Usage

```sh
cp .env.example .env
docker-compose up
docker-compose exec web yarn lint
docker-compose exec db psql -U postgres -c 'create database test;'
docker-compose exec web yarn test
docker-compose exec web yarn test:e2e
docker-compose exec web yarn build
```

## Functionality

REST endpoint via Nest
- http://localhost:3000/

JWT-protected REST endpoint via Nest
- http://localhost:3000/private

GraphQL playground (`query WhoAmI` is JWT-protected)
- http://localhost:3000/graphql
```qgl
query Public {
  things {
    id
    name
  }

  users {
    id
    provider
  }
}

# Add Header: { "Authorization": "Bearer <token>" }
query Private {
  whoAmI {
    id,
    provider,
    providerId,
    username,
    name
  }
  
  orders {
    id
    
    alias
    thing {
      name
    }
  }
}

mutation createOrder {
  createOrder(alias: "myname", thingName: "this is a thing you can order") {
    id
    alias
  }
}
```

Cognito auth (redirects to hosted Cognito UI)
- http://localhost:3000/auth/cognito

Google auth
- http://localhost:3000/auth/google

Next.js page
- http://localhost:3000/home

JWT-protected Next.js page
- http://localhost:3000/profile

## Deloyment

```sh
heroku git:remote --app <app-name>
heroku stack:set container
cp .env .env.production # Fill production values
xargs -a .env.production -I {} heroku config:set {}
heroku addons:create heroku-postgresql:hobby-dev
```

### Useful commands

Nest CLI:
```
docker-compose exec web yarn nest -- --help
```

TypeORM CLI:
```
docker-compose exec web yarn typeorm -- --help
```

## Resources

- https://github.com/jmcdo29/testing-nestjs
