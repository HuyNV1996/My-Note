Cài commitlint

```bash
yarn add -D @commitlint/cli @commitlint/config-conventional
```

Tạo file commitlint.config.js

```bash
module.exports = {
  ignores: [commit => commit.includes('init')],
  extends: ['@commitlint/config-conventional'],
  rules: {
    'body-leading-blank': [2, 'always'],
    'footer-leading-blank': [1, 'always'],
    'header-max-length': [2, 'always', 108],
    'subject-empty': [2, 'never'],
    'type-empty': [2, 'never'],
    'subject-case': [0],
    'type-enum': [
      2,
      'always',
      [
        'feat',
        'fix',
        'perf',
        'style',
        'docs',
        'test',
        'refactor',
        'build',
        'ci',
        'chore',
        'revert',
        'wip',
        'workflow',
        'types',
        'release',
      ],
    ],
  },
  prompt: {
    /** @use `yarn commit :f` */
    alias: {
      f: 'docs: fix typos',
      r: 'docs: update README',
      s: 'style: update code format',
      b: 'build: bump dependencies',
      c: 'chore: update config',
    },
    allowEmptyIssuePrefixs: false,
    allowCustomIssuePrefixs: false,

    // English
    typesAppend: [
      { value: 'wip', name: 'wip:      work in process' },
      { value: 'workflow', name: 'workflow: workflow improvements' },
      { value: 'types', name: 'types:    type definition file changes' },
    ],
  },
};

```

Cài đặt husky

```bash
yarn add -D husky
```

Enable husky

```bash
yarn husky install
```

add pre-commit hook to run commitlint

```bash
yarn husky add .husky/commit-msg "yarn commitlint --edit $1"
```

Test

```bash
git add -A
```

ví dụ

```bash
git commit -m "set up a basic js project, added commitlint and husky for liniting commit messages"
```
