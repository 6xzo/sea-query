# Changelog

All notable changes to this project will be documented in this file.
 
The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## 0.16.4 - 2021-09-26

+ Fix table drop options for SQLite
+ Add `IndexCreateStatement::is_unique_key()`

## 0.16.3 - 2021-09-17

+ [[#131]] `CAST AS` expression
+ [[#131]] `InsertStatement` accepts `SimpleExpr`
+ [[#137]] SQLx Postgres driver bind `DateTime<FixedOffset>`

[#131]: https://github.com/SeaQL/sea-query/issues/131
[#137]: https://github.com/SeaQL/sea-query/pull/137

## 0.16.2 - 2021-09-15

+ [[#120]] Support `RETURNING` for `DeleteStatement`
+ [[#128]] Support `UNION` clause for `SelectStatement`

[#120]: https://github.com/SeaQL/sea-query/issues/120
[#128]: https://github.com/SeaQL/sea-query/pull/128

## 0.16.1 - 2021-09-10

+ [[#129]] MySql `ColumnType::Binary(None)` maps to "blob"

[#129]: https://github.com/SeaQL/sea-query/pull/129

## 0.16.0 - 2021-09-02

+ [[#112]] Introduce `Nullable` trait to permit custom `Option<T>`
+ [[#113]] `ValueType` trait should have a non-panic-ing method
+ [[#114]] `ValueType` revamp

    1. Remove `ValueTypeDefault`
    1. Change `type_name` to return `String`

+ [[#115]] Postgres concatenate operator (`||`)
+ [[#117]] Lock support (`FOR SHARE`, `FOR UPDATE`) for SELECT statement

[#112]: https://github.com/SeaQL/sea-query/pull/112
[#113]: https://github.com/SeaQL/sea-query/pull/113
[#114]: https://github.com/SeaQL/sea-query/pull/114
[#115]: https://github.com/SeaQL/sea-query/pull/115
[#117]: https://github.com/SeaQL/sea-query/pull/117

## 0.15.0 - 2021-08-21

+ [[#107]] Revamp `Value` to typed null value
+ Added `BigDecimal` support

The `Value::Null` variant is removed. You have to use a specific variant with a `None`.

Before:

```rust
Query::insert()
    .values_panic(vec![
        Value::Null,
        2.1345.into(),
    ])
```

After:

```rust
Query::insert()
    .values_panic(vec![
        Value::String(None),
        2.1345.into(),
    ])
```

Since we cannot handle the generic `Null` value on JSON, we removed the `json` method on `InsertStatement` and `UpdateStatement`. The following NO LONGER WORKS:

```rust
let query = Query::insert()
    .into_table(Glyph::Table)
    .json(json!({
        "aspect": 2.1345,
        "image": "24B",
    }));
```

```rust
let query = Query::update()
    .table(Glyph::Table)
    .json(json!({
        "aspect": 2.1345,
        "image": "235m",
    }));
```

In addition, if you constructed `Value` manually before (instead of using `into()` which is unaffected), you have to wrap them in an `Option`:

Before:

```rust
let (sql, values) = query.build(PostgresQueryBuilder);
assert_eq!(
	values,
	Values(vec![Value::String(Box::new("A".to_owned())), Value::Int(1), Value::Int(2), Value::Int(3)]))
);
```

After:

```rust
let (sql, values) = query.build(PostgresQueryBuilder);
assert_eq!(
	values,
	Values(vec![Value::String(Some(Box::new("A".to_owned()))), Value::Int(Some(1)), Value::Int(Some(2)), Value::Int(Some(3))]))
);
```

[#107]: https://github.com/SeaQL/sea-query/pull/107

## 0.14.1 - 2021-08-15

+ [[#87]] Fix inconsistent Ownership of self in Builder APIs
+ [[#105]] Use Arc for SeaRc with feature flag thread-safe

[#87]: https://github.com/SeaQL/sea-query/pull/87
[#105]: https://github.com/SeaQL/sea-query/pull/105

## 0.12.12 - 2021-08-14

+ [[#98]] Support Postgres full text search

[#98]: https://github.com/SeaQL/sea-query/pull/98

## 0.12.11 - 2021-08-13

+ Support SeaORM

## 0.12.10 - 2021-08-11

+ [[#89]] flattening iden enums in derive macro

[#89]: https://github.com/SeaQL/sea-query/pull/87

## 0.12.9 - 2021-08-08

+ [[#77]] Postgres `binary` type
+ [[#81]] example for CockroachDB
+ [[#84]] Fix Postgres constraint keywords
+ [[#75]] `DateTimeWithTimeZone` value type and `TimestampWithTimeZone` column type

[#77]: https://github.com/SeaQL/sea-query/pull/77
[#81]: https://github.com/SeaQL/sea-query/pull/81
[#84]: https://github.com/SeaQL/sea-query/pull/84
[#75]: https://github.com/SeaQL/sea-query/pull/75

## 0.12.8 - 2021-07-24

+ Fix Postgres `datetime` column type mapping
+ `Uuid` in schema builder

## 0.12.7 - 2021-07-13

+ `cust_with_values` allow escape `?` using `??`

## 0.12.6 - 2021-07-07

+ Fixed build error for `sqlx-sqlite`

## 0.12.5 - 2021-07-07

+ Support `Decimal` from rust_decimal

## 0.12.4 - 2021-06-23

+ Added `returning` for update statement

## 0.12.3 - 2021-06-19

+ Added `type_name` for ValueType
+ `Values` derive `Clone`
+ Added `Update::col_expr`
+ Type def Rc as `SeaRc`
+ getters for schema statements

## 0.12.2 - 2021-06-04

+ Fixed `and_where_option`
+ Added `Condition::add_option`

## 0.12.1 - 2021-06-03

+ Added `not_in_subquery`

## 0.12.0 - 2021-05-31

+ Unify `cond_where` and `and_where`. Note: will panic if calling `or_where` after `and_where`.

## 0.11.1 - 2021-05-22

+ Updated Readme

## 0.11.0 - 2021-05-19

+ Added APIs to support ORM
+ Backend and internal refactoring
+ Introduced `QueryStatementBuilder` and `SchemaStatementBuilder` traits
+ Introduced `ConditionalStatement` and `OrderedStatement` traits
+ Introduced any/all style conditions for `cond_where` and `cond_having`

## 0.10.6 - 2021-05-04

+ Postgres `ALTER TYPE` statements for `ENUM`

## 0.10.5 - 2021-05-02

+ Updated documentation

## 0.10.4 - 2021-05-02

+ `returning()` expression for Postgres insert statements
+ Remove redundant index name in foreign key expression of MySQL

## 0.10.3 - 2021-04-30

+ custom `Error` type
+ Empty value list for IN
+ Index prefix and `IndexOrder`

## 0.10.2 - 2021-04-27

+ Foreign key API `from` and `to`
+ Fix foreign key bug in `ON UPDATE`

## 0.10.1 - 2021-04-25

+ Added `index_type()` (`FullText` and `Hash`)
+ Added `unique()` to `Index`
+ Support composite primary key

## 0.10.0 - 2021-04-23

+ Use `IntoIterator` trait instead of `Vec` on most APIs
+ UUID support in `Value`
+ Rusqlite support

## 0.9.6 - 2021-04-18

+ Rename `create_if_not_exists` to `if_not_exists`
+ Remove `partition_option` from `TableAlterStatement`
+ Added `ColumnDef::extra()`

## 0.9.5 - 2021-04-17

+ Added `SchemaStatement`

## 0.9.4 - 2021-04-13

+ Fixed `DateTime` quoting bugs

## 0.9.3 - 2021-04-08

+ Update sea-query-derive to 0.1.2

## 0.9.2 - 2021-04-05

+ derive supporting enum tuple variant and custom method

## 0.9.1 - 2021-03-30

+ updated docs

## 0.9.0 - 2021-03-29

+ Introduced `IntoColumnRef` trait to consolidate `column` and `table.column`
+ Introduced `IntoTableRef` trait to consolidate `table` and `schema.table`
+ Introduced `IntoIden` trait to remove `*_dyn` methods

## 0.8.5 - 2021-03-29

+ added `into_simple_expr()`

## 0.8.4 - 2021-03-24

+ Fixing `IS NULL`

## 0.8.3 - 2021-03-23

+ derive `Debug` on most structs

## 0.8.2 - 2021-03-23

+ Added `unescape_string`

## 0.8.1 - 2021-03-23

+ Improve documentation

## 0.8.0 - 2021-03-14

+ `json` support behind features
+ backend as features (`backend-mysql`, `backend-postgres`, `backend-sqlite`)
+ added `from_schema()`, `from_schema_as()`

## 0.7.0 - 2021-03-06

+ Revamp `Value`
+ `build()` API change
+ `postgres` driver support
+ `json` and `chrono` support

## 0.6.1 - 2021-03-05

+ Added `join_as`
+ Deprecated `expr_alias`, `from_alias`

## 0.6.0 - 2021-02-20

+ Custom expression with parameters `Expr::cust_with_values()`
+ Custom function call `Func::cust()`
+ Postgres enum `Type::create().as_enum()`

## 0.5.0 - 2021-02-09

+ derive macro `#[derive(Iden)]`

## 0.4.0 - 2021-02-02

+ Added JSON binary column type `ColumnDef::json_binary()`
+ Custom column type `ColumnDef::custom()`

## 0.3.0 - 2020-12-29



## 0.2.0 - 2020-12-26



## 0.1.0 - 2020-12-16

Publish to crate.io
