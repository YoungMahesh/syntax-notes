### typescript/javascript project

```bash
npm i -D prettier  # install prettier
npx prettier --write src/  # execute prettier to format all files in folder`src`
```

### +solidity support

```bash
npm i -D prettier prettier-plugin-solidity
npx prettier --write --plugin=prettier-plugin-solidity contracts/ test/  # format files in `contracts` and `test` folders
    # here prettier will also format ".sol" files, as it is using plugin for solidity
```
