# any-db-params

Collect parameters and create placeholders for SQL queries for any database supported by `any-db`.

## Synopsis

```javascript
createParams = require('any-db-params');

// Fake a queryable object (connection/transaction/pool etc)
var queryable = {
  adapter: { name: 'mssql' }
  query: function (sql, params) {
    console.log(sql, params);
  }
};

var param = createParams(queryable);
var sql = 'select * from people ' +
  'where age > ' + param('age', 30) +
  ' and weight > ' + param('weight', 70);

queryable.query(sql, param.values());

//=> 'select * from people where age > @age and weight > @weight' { age: 30, weight: 70 }
```

## API

```ocaml
module.exports =: (Queryable) => ParamFunction

ParamFunction = ( (name: String, value?: Any) => String ) & {
  values: () => Object|Array
}
```

Given a `Queryable`, returns a `ParamFunction`. The ParamFunction is an accessor that accepts 1 or 2 arguments. With 2 arguments it sets a named parameter value and returns a placeholder, with 1 argument it returns the placeholder for a previously defined parameter.

# License

MIT
