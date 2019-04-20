# Upgrade to v6

Sequelize v6 is the next major release after v4

## Breaking Changes

### Support for Node 8 and up

Sequelize v6 will only support Node 8 and up

### Removal of CLS

CLS allowed implicit passing of the `transaction` property to query options inside of a transaction.
This feature was removed for 3 reasons.
- It required hooking the promise implementation which is not sustainable for the future of sequelize.
- It's generally unsafe due to it's implicit nature.
- It wasn't always reliable when mixed promise implementations were used.

Check all your usage of the `.transaction` method and make you sure explicitly pass the `transaction` object for each subsequent query.

#### Example

```js
db.transaction(async transaction => {
  const mdl = await myModel.findByPk(1);
  await mdl.update({
    a: 1;
  });
});
```

should now be:

```js
db.transaction(async transaction => {
  const mdl = await myModel.findByPk(1, { transaction });
  await mdl.update({
    a: 1;
  }, { transaction });
});
```
