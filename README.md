# libsql-stateless-easy

> A wrapper for `libsql-stateless` that provides a much better DX
- ✅ **Supported runtime environments:** Web API (browser, serverless), Bun, Node.js (>=18)
- ✅ **Is built for:** Quick stateless query execution. (Mainly for serverless and edge functions.)
- ⚠️ Supports everything in `@libsql/client` **except (explicit) `transactions` and local or in-memory DBs.**
- ✅ **The API provided by `libsql-stateless-easy` is simple** and (almost) exactly the same as `@libsql/client`.
- ⚠️ `libsql-stateless-easy` comes with the cost of (computational and memory) overheads potentially unneeded by you. But is still very very very slim compared to `@libsql/client`.

<br>

**For much better performance, consider using [`libsql-stateless`](https://github.com/DaBigBlob/libsql-stateless) instead**: it, however, has a pretty raw and explicit API unappreciated by many developers.

# Installation
```sh
$ npm i libsql-stateless-easy #pnpm, yarn, etc.
# or
$ bun add libsql-stateless-easy
```

# Client Usage
`libsql-stateless-easy`'s `client`'s `batch` and `execute` have the exact same API as [`@libsql/client/web`](https://docs.turso.tech/libsql/client-access/javascript-typescript-sdk#create-a-database-client-object-for-local-and-remote-access)'s `client`'s `batch` and `execute`.
```ts
    import { createClient } from "libsql-stateless-easy";
    //or
    const { createClient } = require("libsql-stateless-easy");

    const client = createClient({
        db_url: "https://da-fish-mandible.turso.io",
        authToken: "fksdgfgksdgfksdg.javsdKDGKSBkgsdfg289374dg"
    });
    
    const res = await client.batch([
        {
            sql: "select * from contacts where contact_id = ?;",
            args: [3]
        },
        "select first_name, last_name, email from contacts where contact_id = 2",
        {
            sql: "insert into contacts (contact_id,first_name,last_name,email,phone) values (?,?,?,?,?);",
            args: [6,"glomm","feru","moca@doro.co","001"]
        },
        {
            sql: "delete from contacts where contact_id = :kkl",
            args: {kkl: 6}
        }
    ]);
    console.log(res);

    const res2 = await client.execute({
        sql: "select first_name, last_name, email, contact_id from contacts where contact_id = ?;",
        args: [1]
    });
    console.log(res2);

    const res3 = await client.serverOk();
    if (res3) console.log("Server Compat Check OK");
    else console.error("Server Compat Check NOT OK");
```

- This library has the exact `LibsqlError` API as `@libsql/client`

# This Documentation is incomplete
I'll complete it when I have more time.
### List of other stuff in this library
Feel free to explore them (or even contribute to the documentation!)
```ts
import {
    libsqlValueBuilder, libsqlStatementBuilder, libsqlBatchReqStepsBuilder,
    libsqlBatchReqStepExecCondBuilder, libsqlValueParser, libsqlStatementResParser,
    libsqlBatchStreamResParser,
    libsqlExecute, //has easier API than `libsql-stateless`'s function of the same name
    libsqlBatch, //has easier API than `libsql-stateless`'s function of the same name
    libsqlServerCompatCheck, //has easier API than `libsql-stateless`'s function of the same name
    createClient //used above
} from "libsql-stateless-easy";
```

# TODO (roadmap)
- Add drizzle ORM support