# Mongoose schema inheritance

This npm module extends mongoose schema (returning new one) modified to use ES6 transpiled extends

NOT ~~mongoose-schema-extend~~ but **mongoose-extend-schema**

### Installation
```sh
$ npm install thxmike/mongoose-extend-schema --save
```

### Usage
```javascript
const extendSchema = require('mongoose-extend-schema');

const UserSchema = new mongoose.Schema({
  firstname: {type: String},
  lastname: {type: String}
});

const ClientSchema = extendSchema(UserSchema, {
  phone: {type: String, required: true}
});
```

### Example
```javascript

const mongoose = require('mongoose');
const extendSchema = require('mongoose-extend-schema');

const UserSchema = new mongoose.Schema({
  email: {type: String, unique: true, required: true},
  passwordHash: {type: String, required: true},

  firstname: {type: String},
  lastname: {type: String},
  phone: {type: String}
});

// Extend UserSchema
const AdminUserSchema = extendSchema(UserSchema, {
  firstname: {type: String, required: true},
  lastname: {type: String, required: true},
  phone: {type: String, required: true}
});

const User = mongoose.model('users', UserSchema);
const AdminUser = mongoose.model('admins', AdminUserSchema);

const john = new User({
  email: 'user@site.com',
  passwordHash: 'bla-bla-bla',
  firstname: 'John'
});

john.save();

const admin = new AdminUser({
  email: 'admin@site.com',
  passwordHash: 'bla-bla-bla',
  firstname: 'Henry',
  lastname: 'Hardcore',
  // phone: '+555-5555-55'
});

admin.save();
// Oops! Error 'phone' is required
```

### Source code
```javascript
const mongoose = require('mongoose');

function extendSchema (Schema, definition, options) {
  return new mongoose.Schema(
    Object.assign({}, Schema.obj, definition),
    options
  );
}

module.exports = extendSchema;
```

Author
----
@thxmike
