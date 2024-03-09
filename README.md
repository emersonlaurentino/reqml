# ReqML

Request Markup Language

## Objectives

ReqML aims to be a configuration file format for requests that's easy to read
due to the obvious semantics of TOML. It was initially designed so that it would
be possible to document API endpoints in a versioned way along with the code.
As well as facilitating the use of Client APIs directly from your preferred IDE.

## Example

```toml
title = "Email"

[request]
url = "{{ base_url }}/email"
method = "post"
body = { email: "bob@example.com" }

[headers]
Content-Type = "application/json"

[params]
redirect = "home"
```

## What is the difference from TOML?

The .toml extension is extremely common for project configurations and this
would make it difficult to find TOML files for Requests, so the `.req` extension
allows you to continue writing in TOML, but with benefits from ReqML.

## Especificação

### Metadata

The metadata below can be used by endpoints or folders.

`title`: Obrigatório
`description`: Descreve a pasta ou endpoint
`parent`: Define qual a pasta

### [request]

This is the main table, where you configure the endpoint, method and body if it
is POST.

`url`: The API endpoint
`method`: HTTP Methods
`body`: Optional, common used when the method is post, put or patch.

### [headers]

This table is responsible for defining the request headers.

### [params]

This table is responsible for passing additional information to the server.

### Environments Variables

Environment variables must be declared in a `.[env].vars` file, for example:

`.dev.vars` 

```toml
base_url="http://localhost:8787"
```

`prod.vars`

```toml
base_url="https://api.example.com"
```

In addition to environment variables, it is also possible to declare variables
in folders.

Variables can be deactivated, for this they need to have a `-` prefix.

```toml
-port = 8787
base_url = "http://localhost:{{ port }}"
```

In the example above, the variable is `port` and not `-port` because `-` defines
that the variable is disabled.

### Folders

Folders are useful for grouping endpoints, where metadata and variables are
defined. An endpoint defines which folder it belongs to by providing the folder
title in the `parent` metadata. A folder can be part of another folder.

## Variables

Variables in a folder are only available to child endpoints and respect
environments. It is also possible to overwrite.

```toml
title = "Login"
parent = "Auth"

[vars]
base_url = "{{ base_url }}/login"
```

In the example above, the environment variable `base_url` is overwritten to add
the string `/login` at the end, so the child endpoints of the Login folder when
accessing the variable will read it overwritten by the folder.

## Get Involved

Documentation, bug reports, pull requests, and all other contributions are
welcome!

## Credits

Thank you [TOML](https://github.com/toml-lang/toml) for your obvious and simple
way of writing configurations.
