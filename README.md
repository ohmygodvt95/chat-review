# @ohmygodvt95/chat-review

This is a modification based on [ikoofe/chat-review](https://github.com/ikoofe/chat-review).

## Install

```sh
npm i @ohmygodvt95/chat-review
```

## Usage

### Node.js

```js
import review from '@ohmygodvt95/chat-review';

review({
  gitlabConfig: {
    host: 'https://gitlab.com/',
    mrIId: 2001,
    projectId: 200,
    token: 'glpat-xxxxxx',
  },
  chatgptConfig: {
    apiKey: 'sk-xxxxxxxxx',
  },
});
```

### Shell

```sh
chat-review --chatgpt sk-xxxxxxxxx --token 'glpat-xxxxxx' --project 200 --mr 2001
```

A CLI tool for code review using ChatGPT, primarily consisting of the following command options:

- `--chatgpt`: ChatGPT API Token.
- `--token`: GitLab access token.
- `--project`: GitLab project ID.
- `--mr`: GitLab Merge Request ID.
- `--model`: ChatGPT model type, default is `gpt-3.5-turbo`.
- `--language`: ChatGPT language type, default is Vietnamese.
- `--host`: GitLab access address, default is `https://gitlab.com`.
- `--proxyHost`: ChatGPT API host, default is `https://api.openai.com`.
- `--target`: GitLab Review files, default is /\.(j|t)sx?$/.

### CI

Set the CHATGPT_KEY and GITLAB_TOKEN variables in Gitlab CI/CD, the `.gitlab-ci.yml` would be as follows:

```yml
stages:
  - merge-request

Code Review:
  stage: merge-request
  image: node:latest
  script:
    - npm i @ohmygodvt95/chat-review -g
    - echo "$CI_MERGE_REQUEST_PROJECT_ID" 
    - echo "$CI_MERGE_REQUEST_IID"
    - chat-review run --chatgpt "$CHATGPT_KEY" --token "$GITLAB_TOKEN" --project "$CI_MERGE_REQUEST_PROJECT_ID" --mr "$CI_MERGE_REQUEST_IID"
  only:
    - merge_requests
  except:
    variables:
      - $CI_MERGE_REQUEST_TARGET_BRANCH_NAME !~ /^(main|release)$/
  when: manual
```
