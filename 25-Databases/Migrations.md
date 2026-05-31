# Migrations

## Definition

Migrations **manage database schema** changes.

## Knex.js Example

```javascript
// Create table
exports.up = function(knex) {
    return knex.schema.createTable('users', (table) => {
        table.increments('id');
        table.string('name');
        table.string('email');
    });
};

// Drop table
exports.down = function(knex) {
    return knex.schema.dropTable('users');
};
```

## Quick Revision

- Migrations = version control for DB
- Up: apply changes
- Down: revert changes
- Use Knex, Sequelize, or Prisma

---

## Related Topics

- [[What-is-Migrations]] - [[What-is-Migrations|Migrations]]
- [[Migrations]] - [[Migrations|Migrations]]
- [[What-is-MongoDB]] - [[What-is-MongoDB|MongoDB]]
- [[What-is-MySQL]] - [[What-is-MySQL|MySQL]]
