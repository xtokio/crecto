# Crecto

![crecto](crecto.png)

[https://www.crecto.com/](https://www.crecto.com/)

[![Build Status](https://travis-ci.org/Crecto/crecto.svg?branch=master)](https://travis-ci.org/Crecto/crecto) [![Join the chat at https://gitter.im/crecto/Lobby](https://badges.gitter.im/crecto/Lobby.svg)](https://gitter.im/crecto/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Robust database wrapper for Crystal.  Inspired by [Ecto](https://github.com/elixir-ecto/ecto) for Elixir language.

With built in query composer, associations, transactions, validations, constraints, and more.

Website with guides and examples - [https://www.crecto.com/](https://www.crecto.com/)

See api docs - <http://docs.crecto.com>

## Example

```crystal
user = User.new
user.name = "Shakira"

changeset = Repo.insert(user)
changeset.errors.any?

inserted_user = changeset.instance
inserted_user.name = "Keanu"

changeset = Repo.update(user)
changeset.errors.any?

updated_user = changeset.instance

changeset = Repo.delete(updated_user)
```

## Full Example
```crystal
# shard.yml
dependencies:
  crecto:    
    github: xtokio/crecto
  sqlite3:
    github: crystal-lang/crystal-sqlite3

# src
require "sqlite3"
require "crecto"

module ConnDB
  extend Crecto::Repo

  config do |conf|
      conf.adapter = Crecto::Adapters::SQLite3
      conf.database = "/Users/luis/Desktop/Code/Crystal/apps/orm/src/db/simplecrm.db"
  end
end

class Usuario < Crecto::Model
  set_created_at_field nil
  set_updated_at_field nil

  schema "usuarios" do # table name
      field :iUsuario, Int32, primary_key: true
      field :usuario, String
      field :password, String
      field :nombre, String
      field :foto, String
      field :extension, Int32
      field :email, String
      field :tipo, String
      field :activo, Int32
  end

  validate_required [:usuario, :password]
end

usuarios = ConnDB.all(Usuario,Query.where(iUsuario: 1))

# Iterate records
usuarios.each do |record|
    puts record.iUsuario
    puts record.usuario
    puts record.nombre
end

# Convert to JSON
puts usuarios.to_json
```

## Usage and Guides

Visit [www.crecto.com](https://www.crecto.com)

#### Benchmarks

* [VS raw crystal-pg](https://github.com/Crecto/crecto/wiki/Benchmarks)

## Contributing

1. Fork it ( https://github.com/fridgerator/crecto/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request

### Development Notes

When developing against crecto, the database must exist prior to
testing. There are migrations for each database type in `spec/migrations`,
and references on how to migrate then in the `.travis.yml` file.

Create a new file `spec/repo.cr` and create a module name `Repo` to use for testing.
There are example repos for each database type in the spec folder: `travis_pg_repo.cr`,
`travis_mysql_repo.cr`, and `travis_sqlite_repo.cr`

When submitting a pull request, please test against all 3 databases.

## Thanks / Inspiration

* [Ecto](https://github.com/elixir-ecto/ecto)
* [AciveRecord](https://github.com/rails/rails/tree/master/activerecord)
* [active_record.cr](https://github.com/waterlink/active_record.cr)
* [crystal-api-backend](https://github.com/dantebronto/crystal-api-backend)
