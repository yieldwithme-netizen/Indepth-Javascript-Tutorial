# What-is-Migrations

## Definition

Migrations manage **database schema** changes.

## Example

```javascript
// Knex.js migration
exports.up = function(knex) {
    return knex.schema.createTable('users', (table) => {
        table.increments('id');
        table.string('name');
    });
};

exports.down = function(knex) {
    return knex.schema.dropTable('users');
};
```

## Quick Revision

- Migrations = DB version control
- Up: apply changes
- Down: revert changes
- Use Knex, Sequelize, Prisma

---

## Related Topics

- [[What-is-Migrations]] - [[What-is-Migrations|Migrations]]
- [[What-is-Migrations]] - [[What-is-Migrations|Migrations]]
- [[Migrations]] - [[Migrations|Migrations]]
- [[What-is-ORM]] - [[What-is-ORM|ORM]]
