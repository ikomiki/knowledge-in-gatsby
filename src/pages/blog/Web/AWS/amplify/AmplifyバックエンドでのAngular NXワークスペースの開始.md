---
title: "AmplifyバックエンドでのAngular NXワークスペースの開始"
templateKey: blog-post
date: 2020-07-30T02:42:00+09:00
tags: ["Angular", "Nx", "monorepo", "amplify"]
draft: false
---

## [Getting started with a Angular/NX workspace backed by an AWS Amplify GraphQL API \- Part 1 \- DEV](https://dev.to/beavearony/getting-started-with-a-angularnx-workspace-backed-by-an-aws-amplify-graphql-api---part-1-24m0)

```bash
npx create-nx-workspace amplify-graphql-angular-nx-todo
? What to create in the new workspace empty             [an empty workspace with
 a layout that works best for building apps]
? CLI to power the Nx workspace       Nx           [Recommended for all applicat
ions (React, Node, etc..)]
? Use the free tier of the distributed cache provided by Nx Cloud? No  [Only use
 local computation cache]
Creating a sandbox with Nx...

cd amplify-graphql-angular-nx-todo

npm i -D @nrwl/angular  aws-amplify @aws-amplify/ui-angular aws-amplify-angular
npx nx g @nrwl/angular:app todo-app
? Which stylesheet format would you like to use? CSS
? Would you like to configure routing for this application? No
⠹ Installing packages...

npx nx g @nrwl/angular:library  appsync --tags=shared --style=css
npx nx g @nrwl/angular:library  todo-ui --tags=ui --style=css
```

```apps/todo-app/src/polyfills.ts
// 追記
(window as any).global = window;
(window as any).process = {
  env: { DEBUG: undefined },
};
```

```apps/todo-app/tsconfig.app.json
// 更新
    "types": ["node"]
```

```bash
amplify init
Note: It is recommended to run this command from the root of your app directory
? Enter a name for the project amplifynx
? Enter a name for the environment master
? Choose your default editor: Visual Studio Code
? Choose the type of app that you're building javascript
Please tell us about your project
? What javascript framework are you using angular
? Source Directory Path:  libs/appsync/src/
? Distribution Directory Path: dist/apps/todo-app
? Build Command:  npm run-script build
? Start Command: npm run-script start
Using default provider  awscloudformation
```

```package.json(scripts内)
置き換え
    "start": "npm run rename:aws:config || nx serve",
    "build": "npm run rename:aws:config || nx build",
追加
    "rename:aws:config": "cd libs/appsync/src && [ -f aws-exports.js ] && mv aws-exports.js aws-exports.ts || true"
```

```bash
amplify add auth
Using service: Cognito, provided by: awscloudformation

 The current configured provider is Amazon Cognito.

 Do you want to use the default authentication and security configuration? Default configuration
 Warning: you will not be able to edit these selections.
 How do you want users to be able to sign in? Username
 Do you want to configure advanced settings? No, I am done.
Successfully added resource amplifynx3a27b0e1 locally
```

```bash
 amplify add api
? Please select from one of the below mentioned services: GraphQL
? Provide API name: amplifynx
? Choose the default authorization type for the API Amazon Cognito User Pool
Use a Cognito user pool configured as a part of this project.
? Do you want to configure advanced settings for the GraphQL API No, I am done.
? Do you have an annotated GraphQL schema? No
? Choose a schema template: Single object with fields (e.g., “Todo” with ID, name, description)

? Do you want to edit the schema now? Yes
```

```amplify/backend/api/amplifynx/schema.graphql
type Todo @model @auth(rules: [{ allow: owner }]) {
  id: ID!
  name: String!
  completed: Boolean!
  createdOnClientAt: AWSDateTime!
}
```

```bash
amplify push
? Do you want to generate code for your newly created GraphQL API Yes
? Choose the code generation language target angular
? Enter the file name pattern of graphql queries, mutations and subscriptions src/graphql/**/*.graphql
? Do you want to generate/update all possible GraphQL operations - queries, mutations and subscriptions Yes
? Enter maximum statement depth [increase from default if your schema is deeply nested] 2
? Enter the file name for the generated code src/graphql/API.service.ts
⠋ Updating resources in the cloud. This may take a few minutes...
```

```bash
npx apollo-codegen generate src/graphql/*.graphql --schema src/graphql/schema.json --addTypename --target typescript --output libs/appsync/src/lib/api.types.ts
```

```package.json
// 追加
  "generate": "npx apollo-codegen generate src/graphql/*.graphql --schema src/graphql/schema.json --addTypename --target typescript --output libs/appsync/src/lib/api.types.ts",
  "push": "amplify push && npm run generate"
```

```.gitignore
# GraphQL
.graphqlconfig.yml
```

```bash
npm i -D aws-appsync graphql-tag localforage
```

```libs/appsync/src/lib/gql-operations.ts
import gql from 'graphql-tag';

export const CREATE_TODO = gql`
  mutation CreateTodo($input: CreateTodoInput!) {
    createTodo(input: $input) {
      __typename
      id
      name
      completed
      createdOnClientAt
    }
  }
`;

export const LIST_TODOS = gql`
  query ListTodos($filter: ModelTodoFilterInput, $nextToken: String) {
    listTodos(filter: $filter, limit: 999, nextToken: $nextToken) {
      __typename
      items {
        __typename
        id
        name
        completed
        createdOnClientAt
      }
    }
  }
`;

export const UPDATE_TODO = gql`
  mutation UpdateTodo($input: UpdateTodoInput!) {
    updateTodo(input: $input) {
      __typename
      id
      name
      completed
      createdOnClientAt
    }
  }
`;

export const DELETE_TODO = gql`
  mutation DeleteTodo($input: DeleteTodoInput!) {
    deleteTodo(input: $input) {
      __typename
      id
    }
  }
`;
```

```libs/appsync/src/lib/appsync.module.ts
import { NgModule } from '@angular/core';
import Amplify, { Auth } from 'aws-amplify';
import { AmplifyAngularModule, AmplifyService } from 'aws-amplify-angular';
import AWSAppSyncClient, { AUTH_TYPE } from 'aws-appsync';
import * as localForage from 'localforage';
import config from '../aws-exports';
import { AmplifyUIAngularModule } from '@aws-amplify/ui-angular';

Amplify.configure(config);

export const AppSyncClient = new AWSAppSyncClient({
  url: config.aws_appsync_graphqlEndpoint,
  region: config.aws_appsync_region,
  auth: {
    type: AUTH_TYPE.AMAZON_COGNITO_USER_POOLS,
    jwtToken: async () =>
      (await Auth.currentSession()).getIdToken().getJwtToken(),
  },
  complexObjectsCredentials: () => Auth.currentCredentials(),
  cacheOptions: { addTypename: true },
  offlineConfig: { storage: localForage },
});

@NgModule({
  imports: [AmplifyAngularModule],
  exports: [AmplifyUIAngularModule],
  providers: [AmplifyService],
})
export class AppsyncModule {}
```

```libs/appsync/src/lib/offline.helpers.ts
import { OperationVariables } from 'apollo-client';
import {
  buildMutation,
  CacheOperationTypes,
  CacheUpdatesOptions,
  VariablesInfo
} from 'aws-appsync';
import { DocumentNode } from 'graphql';
import { AppSyncClient } from './appsync.module';

export interface TypeNameMutationType {
  __typename: string;
}

export interface GraphqlMutationInput<T, R extends TypeNameMutationType> {
  /** DocumentNode for the mutation */
  mutation: DocumentNode;
  /** An object with the mutation variables */
  variablesInfo: T | VariablesInfo<T>;
  /** The queries to update in the cache */
  cacheUpdateQuery: CacheUpdatesOptions;
  /** __typename from your schema */
  typename: R['__typename'];
  /** The name of the field with the ID (optional) */
  idField?: string;
  /** Override for the operation type (optional) */
  operationType?: CacheOperationTypes;
}

/**
 * Builds a MutationOptions object ready to be used by the ApolloClient to automatically update the cache according to the cacheUpdateQuery
 * parameter
 */
export function graphqlMutation<
  T = OperationVariables,
  R extends TypeNameMutationType = TypeNameMutationType
>({
  mutation,
  variablesInfo,
  cacheUpdateQuery,
  typename,
  idField,
  operationType
}: GraphqlMutationInput<T, R>) {
  return buildMutation<T>(
    AppSyncClient,
    mutation,
    variablesInfo,
    cacheUpdateQuery,
    typename,
    idField,
    operationType
  );
}

export function executeMutation<
  T = OperationVariables,
  R extends TypeNameMutationType = TypeNameMutationType
>(mutationInput: GraphqlMutationInput<T, R>) {
  return AppSyncClient.mutate(graphqlMutation<T, R>(mutationInput));
}
```

```libs/appsync/src/index.ts
export * from './lib/appsync.module';
export * from './lib/api.types';
export * from './lib/gql-operations';
export * from './lib/offline.helpers';
```

---

## [Creating Angular UI components and connecting them to the GraphQL AppSync API \- Part 2 \- DEV](https://dev.to/beavearony/creating-angular-ui-components-and-connecting-them-to-the-graphql-appsync-api-part-2-33n0)

```bash
npx nx g c --project=todo-ui todo-page --export
npx nx g c --project=todo-ui -c=OnPush create-form
npx nx g c --project=todo-ui -c=OnPush todo-list
npx nx g c --project=todo-ui -c=OnPush todo-item
npx nx g c --project=todo-ui -c=OnPush filter-bar
npx nx g c --project=todo-ui -c=OnPush error-alert
```

```apps/todo-app/src/styles.css
/* You can add global styles to this file, and also import other style files */
@import '~aws-amplify-angular/theme.css';

html,
body {
  height: 100%;
}
body {
  margin: 0;
  background-color: var(--color-accent-brown);
}

/* Amplify UI fixes */
.amplify-form-input {
  width: calc(100% - 1em);
  box-sizing: border-box;
}
.amplify-alert {
  margin-top: 2em;
}
.amplify-greeting > * {
  white-space: nowrap;
}
.amplify-greeting-flex-spacer {
  width: 100%;
}

.card,
.amplify-authenticator {
  width: var(--component-width-desktop);
  margin: 1em auto;
  border-radius: 6px;
  background-color: var(--color-white);
  box-shadow: var(--box-shadow);
}
.card-container {
  width: 400px;
  margin: 0 auto;
  padding: 1em;
}

@media (min-width: 320px) and (max-width: 480px) {
  .card,
  .amplify-authenticator {
    width: var(--component-width-mobile);
    border-radius: 0;
    margin: 0.5em auto;
  }
  .card-container {
    width: auto;
  }
  .amplify-alert {
    left: 4%;
  }
}
```

コンポーネントの設定は元ページを参照のこと。
